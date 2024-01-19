+++
title = "Disable IPV6 for Requests"
weight = 600
date = 2024-01-18
draft = true
+++

Today I encountered the strangest issue for the first time. 

```python
requests.packages.urllib3.util.connection.HAS_IPV6 = False
```

https://stackoverflow.com/a/62599037

https://stackoverflow.com/a/72440253