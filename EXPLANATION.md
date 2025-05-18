# Okay, so some of this is kinda strange, so I'll try to explain as much as I can

Let's take a look at `coro`
```py
lambda f, name=None, type_hint=None: (y := types.coroutine(f), setattr(y, "__code__", (z := y.__code__).replace(co_flags=z.co_flags | 128)), name is not None and setattr(y, "__name__", name), type_hint is not None and setattr(y, "__annotations__", type_hint))
```

The first thing we're doing is making the function into a coroutine using `types.coroutine`
Usually, this would be all we have to do to make a function a coroutine, but discord.py does some... funky stuff and this won't make it think it's actually a coroutine.
So we have to update the code's flags to pass `inspect.iscoroutine`

Next, we set the `__name__` of the function, if it's passed to the decorator, this is to allow discord.py to correctly pass `ctx` to the function.
If I was making a cog in this, I would have to set `__qualname__` to allow discord.py to pass the cog, ctx, and other variables to the function.