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

