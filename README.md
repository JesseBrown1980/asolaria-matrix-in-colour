# ASOLARIA MATRIX IN COLOUR

Ultra-high-quality **lossless** colour photographs of the Asolaria matrix, taken once per minute
while the subagent rooms flow.

This is a **time-boxed** test, not a quantity-boxed one. The simulator is designed never to be
turned off; for this run we simply let it run for ten minutes and **measured the in-betweens**.

## What you are looking at

Each photograph is the whole room substrate: **10,000 rooms = exactly a 100×100 lattice**
(`ROOM_COUNT` from `agent-runtime/rooms.rs`). One cell is one room.

- **Colour** = `sha256(pid)[0:3]` — the room's own digest. **Derived, never chosen.**
- **Create-only**: the first arrival's colour is that room's address forever. Later arrivals add
  moss *depth*; they never overwrite the colour. **0 cells overwritten.**
- **Brightness** = that room's moss depth relative to the deepest room alive at that minute.

So the rock does not roll. It sits, and the moss settles on it, minute by minute, as the next
subagents spawn.

## Honest technical notes

- **PNG written byte-by-byte in pure Rust std** — CRC32 per chunk, Adler-32 over the zlib stream,
  DEFLATE *stored* blocks. **0 external crates.** There is no lossy step anywhere, so every frame is
  bit-exact: the highest quality a photograph can be, at the cost of file size.
- **Rust 1.81**, `cargo clippy --release -- -D warnings` exit 0.
- **Integer only.** `float_used=0` on this path.
- **`os_process_spawn=0`, `node_used=0`, `json=0`.** A room is a function call, not a process. That
  is the whole reason this runs at all: the process-per-agent lane measured a ceiling of **one**
  concurrent agent on this machine; the room lane runs millions.
- Every frame is sealed **HBI → HBP → SHA → SH → HASH** in `receipts/`, each one chained to the
  previous frame's hash. `INDEX.hbp` is the roll-up.

## Layout

- `frames/matrix-min-NN.png` — the photographs
- `receipts/matrix-min-NN.hbp` / `.hbi` / `.sha256` — the sealed receipt for each frame
- `INDEX.hbp` / `.hbi` — one row per frame

Seat **ACER-CLAUDE-FABLE5** · pid `8467a937cba309f7` · owner **OP-JESSE** · `E=0`
