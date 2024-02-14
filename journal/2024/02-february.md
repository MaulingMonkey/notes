# Wednesday February 14th, 2024

*   Recently created [`valrow`](https://docs.rs/valrow/).
*   Acquired thin mints.
*   Watched [RustConf 2023 - The standard library is special. Let's change that.](https://www.youtube.com/watch?v=vPOlYFs6Bs0).  Takeaways:
    Opt out of auto traits with:
    ```rust
    PhantomData<MutexGuard<'_, ()>> : !Send
    PhantomData<Cell<()>>           :         !Sync
    PhantomData<*mut ()>            : !Send + !Sync
    ```
    A thought occurs to me: can I (ab)use to create my own "negative" traits on stable?
