## Saturday June 4th, 2022
*   Acquired a NAS
    *   Opinion: NAS projects should be bare git repositories (eliminates target dir cruft)
*   Acquired extra cooking gear.
    *   Starting cooking before cleaning old dishes = win
    *   Batch cooking = lose, I batch immediately before I get tired of the thing I batch cook
*   Fantasized a lot about psuedo-OSes and combining [chromium's sandbox](https://chromium.googlesource.com/chromium/src/+/HEAD/docs/design/sandbox.md) with a wasm runtime.

#### 2D Engine Design Fantasizing / Strawmaning
```rust
#![windows_subsystem = "windows"]

engine::app!(Game::new());
// or:
fn main() {
    engine::init(include_dir!(...));
    engine::d3d11::spawn_window(Game::new());
    engine::wait_for_views_to_close();
}

struct Game {}

impl Game {
    pub fn new() -> Self { Self {} }
}

impl engine::View for Game {
    fn render(&mut self, ctx: &mut engine::RenderCtx) {
        let dt = ...;
        let sprite = ctx.sprites.get("something"); // assets/sprites.json: { "something": { ... } }
        let [w, h] = ctx.view_size();

        ctx.clear(0xFF112233);
        ctx.set_texture(2); // assets/textures/2.png ?
        ctx.draw_sprites(&[
            Sprite { id: sprite, xy: [0, 0], .. Default::default() },
            Sprite { id: sprite, xy: [0, 0], .. Default::default() },
        ]);
    }
}
```

## Thursday June 16th, 2022
*   Satisfactory yeet
*   Small notepad ftw

#### Projects to consider
*   [ ] engine2 font
*   [ ] sandboxing playground (apply/demo chromium sandbox techniques in Rust)
*   [ ] hello world dll for x64 decompiling might be easier than hello world exe
*   [ ] web flamegrapher
    *    Use [Server Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)
*   [ ] d3d flamegrapher
*   [ ] RIIR (DCSS? OpenTTD?)

#### Minimal HTTP Server Notes
* <https://datatracker.ietf.org/doc/html/rfc7231>
    * ~100 pages
* **"MUST:"**
    * Generate `CR` `LF`
    * Generate `Content-Encoding: gzip` etc. if compressing
    * Support `GET` and `HEAD`
    * Not abuse "safe" methods
    * Not send a body in response to `HEAD`
    * Handle `Expect: 100-continue` for `HTTP/1.1` requests
    * Not send a `1xx` response to an `HTTP/1.0` client
    * Not generate a payload in a 205 (Reset Content) response
    * Generate an `Allow` header field in a 405 (Method Not Allowed) response containing a list of the target resource's currently supported methods.
    * Send an `Upgrade` header field in a 426 (Upgrade Required) response to indicate the required protocol(s)
    * Generate any header timestamps in the IMF-fixdate format (e.g. `Sun, 06 Nov 1994 08:49:37 GMT`.)
    * Not generate additional whitespace in an HTTP-date beyond that specifically included as SP in the grammar
    * Interpret a timestamp that appears to be more than 50 years in the future as representing the most recent year in the past that had the same last two digits.
    * Not send a `Date` header field if it does not have a clock capable of providing a reasonable approximation of the current instance in Coordinated Universal Time.
    * MAY send a `Date` header field if the response is in the 1xx (Informational) or 5xx (Server Error) class of status codes.
    * MUST send a `Date` header field in all other cases.
