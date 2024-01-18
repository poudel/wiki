+++
title = "Exploring data on the shell"
weight = 20
date = 2024-01-09
draft = true
+++


## Interactive shell

Once in a while, I want to create a sort of breakpoint while exploring data 
and play with the context. I tried using the built in `breakpoint()` function
to start a pdb session but it's not ideal for that. 

I found out that there are several ways to start a "nested" interactive python 
shell from python.

### IPython embed

If I have `ipython` installed on this environment, then I could simply run the 
following.

```python
from IPython import embed
embed()
```

I haven't verified this but I imagine this copies the current context and starts a 
shell with it. I wanted to know if mutating variables inside the nested shell context 
changes it in the parent context. So I ran the following

```python
from IPython import embed

a = [1, 2]
embed()
print(a)
```

The output below suggests that it does mutate the parent context

```txt
In [1]: a.append(3)

In [2]: exit

[1, 2, 3]
```

# Paging

Another thing I really miss is the pager. 

# Progress report

Usually iterating through

```python
a = f"{idx}".zfill(5)
print(f"Done: {a}/35000", end="\r")
```
