# FocusFlow Desktop

A Rust Pomodoro timer for Linux system trays. It demonstrates state transitions, asynchronous command/event channels, local persistence and automated tests. It is a desktop utility, not a backend service.

## Implemented

- Tray controls for start/pause, reset, skip and quit.
- Focus/break phases with configurable intervals and optional automatic continuation.
- TOML configuration with bounded intervals and presets.
- Persistent timer state, focused time and session statistics.
- Desktop notifications and optional finish/test sounds.
- Desktop-entry and optional autostart installer.

## Run locally

Requires Rust and a Linux desktop session with a compatible tray and D-Bus/notification services. Audio helpers are optional. No cloud account or paid service is required.

```bash
git clone https://github.com/matheussricardoo/focusflow-desktop.git
cd focusflow-desktop
cargo check --locked
cargo test --locked
cargo run --locked
```

Use the tray menu after launch; there is no main application window. A graphical tray/session is required for interactive validation.

## Configuration and storage

On a typical Linux installation, config and state files are stored under `~/.config/focusflow-desktop/`:

```toml
work_minutes = 25
break_minutes = 5
sound_on_finish = true
auto_start_next_phase = false
```

Work duration is clamped to 1–180 minutes and break duration to 1–60. Daily statistics use UTC day boundaries. Runtime state and statistics are TOML files, not a shared database or cloud sync service.

Sound playback tries paplay, pw-play and aplay for each known sound file, then canberra-gtk-play and a terminal bell. Availability and audio output depend on the desktop environment.

## Optional installation

```bash
cargo build --release --locked
sh scripts/install-desktop.sh --help
sh scripts/install-desktop.sh --no-build
```

The installer writes the binary/desktop entry into user-local directories. `--autostart` additionally enables login startup. The installer was not executed during the portfolio review.

## Validation and limits

The existing **61 tests passed** during the review. They exercise configuration, state transitions, persistence, sound fallback logic and tray actions. They do not prove visual tray behavior, notification delivery or working audio on every Linux desktop. Manual graphical validation remains required.

## Author and license

[Matheus Ricardo](https://github.com/matheussricardoo) · [LinkedIn](https://www.linkedin.com/in/matheussricardoo/). See [LICENSE](LICENSE).
