+++
title = "Django KSUID field"
weight = 402
date = 2023-11-09
+++

Essentially a copy of [shortuuid/django_fields](https://raw.githubusercontent.com/skorokithakis/shortuuid/master/shortuuid/django_fields.py)

```python
import ksuid
from django.db.models import TextField
from django.utils.translation import gettext_lazy as _

class KSUIDField(TextField):
    description = _("KSUID field with optional prefix")

    def __init__(self, *args, **kwargs):
        self.prefix = kwargs.pop("prefix", "")

        if "editable" not in kwargs:
            kwargs["editable"] = False
        kwargs["default"] = self._generate_ksuid
        super().__init__(*args, **kwargs)

    def _generate_ksuid(self):
        return self.prefix + str(ksuid.Ksuid())

    def deconstruct(self):
        name, path, args, kwargs = super().deconstruct()
        kwargs["prefix"] = self.prefix
        kwargs.pop("default", None)
        return name, path, args, kwargs

```

A primary key subclass

```python
from django.db import models


class KSUIDPrimaryKeyField(KSUIDField):

    def __init__(self, prefix, *args, **kwargs):
        if not prefix.endswith("_"):
            prefix = f"{prefix}_"
        kwargs["prefix"] = prefix
        kwargs["primary_key"] = True
        kwargs["unique"] = True
        super().__init__(*args, **kwargs)
```
