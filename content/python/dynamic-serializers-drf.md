+++
title = "Dynamic serializers in DRF"
weight = 400
+++

# Dynamic serializers

Normally, you define serializers as classes. Something like this:

```python
from rest_framework import serializers

class Wonderful(serializers.Serializer):
    is_it = serializers.BooleanField()
```

But what if you don't have the fields yet? For example, you need to validate
dynamic data coming from user. And the user also defines the field types which
is stored in the database.

It's possible to do that with python meta classes.

```python
from rest_framework import serializers

Wonderful = type("Wonderful", (serializers.Serializer,), dict(is_it=serializers.BooleanField()))
```

There must be a simpler way to do this, without using `type`. 


## Passing field definitions

This seems to work. Is also much simpler and has the benefit of not using meta programming.

```python
from rest_framework import serializers

class Wonderful(serializers.Serializer):

    def __init__(self, field_defs, *args, **kwargs):
        super().__init__(*args, **kwargs)
        for fn, fd in field_defs.items():
            self.fields[fn] = fd


Wonderful(
    field_defs=dict(is_it=serializers.BooleanField()),
    data={"is_it": False}
)
```

