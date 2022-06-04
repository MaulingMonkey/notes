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
}

struct Game {}

impl Game {
    pub fn new() -> Self { Self {} }
}

impl engine::App for Game {
    fn update(&mut self, ctx: &mut engine::UpdateCtx) { // optional? skip for v1?
        let dt = ...;
        // ...
    }

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
