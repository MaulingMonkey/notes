### From [`#general`](https://discord.com/channels/676678179678715904/676685797524766720/1027385603018477595) in the `Game Development in Rust` Discord

```lua
        -- We MUST iterate over it because the Lua length operator does not work
        -- as you might expect when a table has nil values in it.
        -- See for yourself: Lua says #{nil,nil,nil,1,nil} is 0!
```

> so JS has array spread, like [...a, ...b] that's pretty nifty, right?
> 
> Lua has had something similar for a very long time, called unpack (5.1-) or table.unpack (5.2+)
> 
> and due to a weird quirk of Lua's calling convention, the equivalent construct to that JS snippet DOES NOT do what you want
> 
> so if you write this in JS:
> ```js
> let a = [1, 2, 3];
> let b = [4, 5, 6];
> console.log(...a, ...b);
> ```
> you get
> ```text
> 1, 2, 3, 4, 5, 6
> ```
>
> but if you write the equivalent Lua:
> ```lua
> local a = {1, 2, 3}
> local b = {4, 5, 6}
> print(unpack(a), unpack(b))
> ```
> any guesses what you'll get?
> 
> that's right!
> ```text
> 1 4 5 6
> ```
>
> whenever an expression that returns a number of values != 1 is put onto the stack before another expression, it is expanded or truncated to exactly one value
> 
> Lua having multiple return has other funny ramifications too 
>
> Consider this function:
> ```lua
> local function foo(condition)
>     if condition then
>         return 5
>     end
> end
> ```
> In a normal programming language, you might say that this function returns a number or null
> 
> but in Lua, there's a distinction between `return nil` and `return`/not having a return statement
> 
> which comes up with a common standard library function, `table.insert`, the equivalent to Rust's `Vec::push`
> 
> ```lua
> local list = {}
> table.insert(list, 5) -- list is now {5}
> table.insert(list, foo(true)) -- list is now {5, 5}
> table.insert(list, foo(false)) -- RUNTIME ERROR! Wrong number of arguments to table.insert!
> ```
> 
> when I was at Roblox I wrote a big Confluence page with all of these footguns because each one of them had caused production issues
> 
> this all made me even start an entire blog to just complain about languages I had to write every day
> 
> it's got one Lua article: <https://horriblesoftware.com/2018/09/19/Lua-debug-traceback.html>
> > Lua: debug.traceback: Horrible Software
>
> The place where poor software engineering decisions come back to haunt everyone.


> []----[] — Today at 6:20 PM<br>
> wait.  i just tried this online.
> https://tio.run/##bY3BCoAgEETvfcUcW@hSYbc@RkpLiDVMv99WiboEw8DOzmOOpHO2iZfoPMN63@qw9R3Eh@oj4QyOI34ehtemeWF9rbaWCM6WRo95hkLcDSOYmALLJczDla2CKOqgRPRFk0STiHK@AQ

...

> Lucien — Today at 6:21 PM<br>
> print(asdf(5)) will not print anything

> []----[] — Today at 6:21 PM<br>
> Well, two wrongs make a right.  It's a sane language now.

> Lucien — Today at 6:21 PM<br>
> but print((asdf(5))) will print nil<br>
> notice the extra parens<br>
> Lua's () operator is the "exactly one value" operator<br>
> amusingly, `print(asdf(5), asdf(5))` will print `nil`<br>
> if you ever call a function and pass its result directly to table.insert, I recommend<br>
> `table.insert(list, (foo()))`<br>
> because `table.insert` has an optional middle argument that has caused production issues in my life, too<br>
