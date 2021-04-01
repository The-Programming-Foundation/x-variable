## x-variable

x-variable is based on v86 which emulates an x86-compatible CPU and hardware. Machine code is translated to WebAssembly modules at runtime in order to achieve decent performance. x-variables empowers the interactive experiences of The Programming Foundation's Learn. Here's a list of emulated hardware:

- An x86-compatible CPU. The instruction set is around Pentium III level,
 including full SSE2 support. Some features are missing, in particular:
  - Task gates, far calls in protected mode
  - Some 16 bit protected mode features
  - Single stepping (trap flag, debug registers)
  - Some exceptions, especially floating point and SSE
  - Multicore
  - PAE
  - 64-bit extensions
- A floating point unit (FPU). Calculations are done using the Berkeley
  SoftFloat library and therefore should be precise (but slow). Trigonometric
  and log functions are emulated using 64-bit floats and may be less precise.
  Not all FPU exceptions are supported.
- A floppy disk controller (8272A).
- An 8042 Keyboard Controller, PS2. With mouse support.
- An 8254 Programmable Interval Timer (PIT).
- An 8259 Programmable Interrupt Controller (PIC).
- Partial APIC support.
- A CMOS Real Time Clock (RTC).
- A generic VGA card with SVGA support and Bochs VBE Extensions.
- A PCI bus. This one is partly incomplete and not used by every device.
- An IDE disk controller.
- An NE2000 (8390) PCI network card.
- A virtio filesystem.
- A SoundBlaster 16 sound card.

How to build, run and embed?
-

You need [important]:

- java (for Closure Compiler, not necessary when using `debug.html`)
- make
- gcc and libc-i386 for building some of the test binaries
- nasm, gdb and qemu-system (for running tests)
- rust-nightly with the wasm32-unknown-unknown target
- A version of clang compatible with rust-nightly
- nodejs (a recent version is required, 10.11.0 is known to be working)

See `tools/docker/test-image/Dockerfile` for a full setup on Debian.


- Run `make` to build the debug build (at `debug.html`).
- Run `make all` to build the optimized build (at `index.html`). [USE THIS]
- ROM and disk images are loaded via XHR, so if you want to try out `index.html`
  locally, make sure to serve it from a local webserver. You can use `make run`
  to serve the files using Python's http module.
- If you only want to embed v86 in a webpage you can use libv86.js. For
  usage, check out the [examples](examples/).

API examples
-

- [Basic](examples/basic.html)
- [Programatically using the serial terminal](examples/serial.html)
- [A Lua interpreter](examples/lua.html)
- [Two instances in one window](examples/two_instances.html)
- [Saving and restoring emulator state](examples/save_restore.html)

Using v86 for your own purposes is as easy as:

```javascript
var emulator = new V86Starter({
    screen_container: document.getElementById("screen_container"),
    bios: {
        url: "../../bios/seabios.bin",
    },
    vga_bios: {
        url: "../../bios/vgabios.bin",
    },
    cdrom: {
        url: "../../images/linux.iso",
    },
    autostart: true,
});
```

See [starter.js](src/browser/starter.js)


Credits
-

- CPU test cases via [QEMU](https://wiki.qemu.org/Main_Page)
- More tests via [kvm-unit-tests](https://www.linux-kvm.org/page/KVM-unit-tests)
- [zstd](https://github.com/facebook/zstd) support is included for better compression of state images
- [Berkeley SoftFloat](http://www.jhauser.us/arithmetic/SoftFloat.html) is included to precisely emulate 80-bit floating point numbers
- [The jor1k project](https://github.com/s-macke/jor1k) for 9p, filesystem and uart drivers
