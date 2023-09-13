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

```
Wonderful = type("Wonderful", (serializers.Serializer,), dict(is_it=serializers.BooleanField()))
```

There must be a better way to do this without using `type`. 