# Embedded Systems Overview

Personally, I don't really have a lot of experience with embedded systems so this is a basic overview from what I
understand of the ecosystem.

Embedded systems are systems that are "embedded" in specialised devices. Things like IoT devices, robots, electronic
tools, MP3 players, electronic home appliances, and so on. Software that runs on these systems often runs on
microcontrollers. Memory and computing power is limited, and the presence of an operating system is not guaranteed.

Low-level languages like C/C++ or Rust would be beneficial here, given the memory and computing limitations.
Alternatively, using MicroPython or a small Python or Java runtime environment can be used, if resources allow and the
development experience is better.

## Build

C/C++ tooling:

- Development using a text editor, VS Code or CLion.
- Building is done with Makefiles and/or CMake, with compilation and linking.
- Dependency / Package Management can be done with Conan, or with a carefully constructed build environment that
  contains the dependencies needed.
- [CMake](https://cmake.org/)
- [Conan - Package Manager](https://conan.io/)
- [GNU make](https://www.gnu.org/software/make/manual/html_node/index.html)
- [GTK Docs](https://docs.gtk.org/)
- [learn-cpp.org](https://www.learn-cpp.org/)
- [learncpp.com](https://www.learncpp.com/)
- [Qt Framework Reference](https://doc.qt.io/qt-6.2/reference-overview.html)

Rust tooling:

- Development using a text editor, VS Code or RustRover.
- Building is done with Cargo.
- Dependency / Package Management is done with Cargo.
- [Cargo - Reference](https://doc.rust-lang.org/cargo/reference/index.html)
- [Rust Playground](https://play.rust-lang.org/)
- [The Rust Book](https://doc.rust-lang.org/book/title-page.html)
- [crates.io](https://crates.io/)
- [gtk - crate](https://crates.io/crates/gtk)
- [imgui - crate](https://crates.io/crates/imgui)
- [qt_core - crate](https://crates.io/crates/qt_core)

Python / [MicroPython](https://docs.micropython.org) tooling:

- Development using a text editor, VS Code or PyCharm, with Python virtual environments.
- Building is done with individual tools as needed for specific purposes (Python is an interpreted language so no
  compiling is needed).
- Dependency / Package Management is done with `pip` (or `mip` for MicroPython)

Java:

- Development using IntelliJ or Eclipse.
- Building is done with Maven or Gradle, usage of GraalVM to get native / compiled binaries could be useful(?)
- Dependency / Package Management is done with Maven or Gradle.

## Release

Not really sure. I assume some copying all required artifacts from a device onto the microcontroller via some USB /
serial port.
