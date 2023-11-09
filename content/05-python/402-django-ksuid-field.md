+++
title = "Django KSUID field"
weight = 402
date = 2023-11-09
+++

Essentially a copy of [shortuuid/django_fields](https://raw.githubusercontent.com/skorokithakis/shortuuid/master/shortuuid/django_fields.py)

```python
import ksuid
from django.db.models import CharField
from django.utils.translation import gettext_lazy as _


class KSUIDField(CharField):
    description = _("KSUID field with optional prefix")

    def __init__(self, *args, **kwargs):
        self.prefix = kwargs.pop("prefix", "")
        base62_length = 27

        if "max_length" not in kwargs:
            kwargs["max_length"] = base62_length + len(self.prefix)

        if "editable" not in kwargs:
            kwargs["editable"] = False

        super().__init__(*args, **kwargs)

    def _generate_ksuid(self):
        return self.prefix + str(ksuid.Ksuid())

    def deconstruct(self):
        name, path, args, kwargs = super().deconstruct()
        kwargs["prefix"] = self.prefix
        kwargs.pop("default", None)
        return name, path, args, kwargs
```

And a little more complicated setup with a base model class that has a prefixed ksuid as the primary key.

```python
from django.db import models

class KSUIDPrimaryKeyField(KSUIDField):
    __prefix_registry = {}

    def __init__(self, *args, **kwargs):
        kwargs["primary_key"] = True
        kwargs["unique"] = True
        super().__init__(*args, **kwargs)

    def contribute_to_class(self, cls, name, private_only=False):
        super().contribute_to_class(cls, name, private_only=private_only)

        try:
            prefix = getattr(cls, "id_prefix")
            if not prefix.endswith("_"):
                prefix = f"{prefix}_"

            self.prefix = prefix

            if self.prefix in self.__prefix_registry and cls != self.__prefix_registry[self.prefix]:
                raise ImproperlyConfigured(
                    f"Duplicate id_prefix for {cls} and {self.__prefix_registry[self.prefix]}"
                )

            self.__prefix_registry[self.prefix] = cls
        except AttributeError:
            if not cls._meta.abstract:
                raise ImproperlyConfigured(f"id_prefix missing from model definition: {cls}")


class BaseModel(models.Model):
    idx = KSUIDPrimaryKeyField()

    class Meta:
        abstract = True
```

Now I just inherit the `BaseModel` and specify an `id_prefix`. Like this:

```python
class Something(BaseModel):
    id_prefix = "sth"
    name = models.CharField(max_length=100)
```
