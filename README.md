# Windows Capture
![Crates.io](https://img.shields.io/crates/l/windows-capture) ![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/NiiightmareXD/windows-capture/rust.yml) ![Crates.io](https://img.shields.io/crates/v/windows-capture)

**Windows Capture** is a highly efficient Rust library that enables you to effortlessly capture the screen using the Graphics Capture API. This library allows you to easily capture the screen of your Windows-based computer and use it for various purposes, such as creating instructional videos, taking screenshots, or recording your gameplay. With its intuitive interface and robust functionality, Windows-Capture is an excellent choice for anyone looking for a reliable and easy-to-use screen capturing solution.

## Features

- Only Updates The Frame When Required.
- High Performance.
- Easy To Use.
- Latest Screen Capturing API.

## Installation

Add this library to your `Cargo.toml`:

```toml
[dependencies]
windows-capture = "1.0.19"
```
or run this command

```
cargo add windows-capture
```

## Usage

```rust
use std::time::Instant;

use windows_capture::{
    capture::{WindowsCaptureHandler, WindowsCaptureSettings},
    frame::Frame,
    window::Window,
};

struct Capture {
    fps: usize,
    last_output: Instant,
}

impl WindowsCaptureHandler for Capture {
    type Flags = ();

    fn new(_: Self::Flags) -> Self {
        Self {
            fps: 0,
            last_output: Instant::now(),
        }
    }

    fn on_frame_arrived(&mut self, _frame: &Frame) {
        self.fps += 1;

        if self.last_output.elapsed().as_secs() >= 1 {
            println!("{}", self.fps);
            self.fps = 0;
            self.last_output = Instant::now();
        }
    }

    fn on_closed(&mut self) {
        println!("Closed");
    }
}

fn main() {
    let settings = WindowsCaptureSettings::new(Monitor::get_primary(), true, false, ());

    Capture::start(settings).unwrap();
}
```

## Documentation

Detailed documentation for each API and type can be found [here](https://docs.rs/windows-capture).

## Contributing

Contributions are welcome! If you find a bug or want to add new features to the library, please open an issue or submit a pull request.

## License

This project is licensed under the [MIT License](LICENSE).