# Research Topic: Best practices for software partitioning within OS kernels (e.g., monolithic vs. microkernel vs. hybrid approaches, loadable kernel modules, driver models). Include common patterns in boot sequences and initialization processes that support modular kernel design.
# Research Level: L0
# Date: 2024-07-27

## Key Resources for Kernel Software Partitioning (L0)

This list includes resources cited for the main overview and specific sub-topics in the `expert_L0.md` document.

### General OS Concepts & Kernel Architecture:

*   Tanenbaum, A. S., & Bos, H. (2015). *Modern Operating Systems* (4th ed.). Pearson.
    *   Cited for: Kernel Architectural Approaches (Chapter 1, Section 1.7), Hardware Abstraction Layers (HALs).
*   "Microkernel" - Wikipedia: [https://en.wikipedia.org/wiki/Microkernel](https://en.wikipedia.org/wiki/Microkernel)
    *   Cited for: Kernel Architectural Approaches.
*   Mauerer, W. (2008). *Professional Linux Kernel Architecture*. Wrox.
    *   Cited for: Subsystem Interaction and Internal Kernel Interfaces.
*   The specific source code of an OS kernel (e.g., Linux, FreeBSD).
    *   Cited for: Subsystem Interaction and Internal Kernel Interfaces.

### Loadable Kernel Modules & Device Drivers:

*   Corbet, J., Rubini, A., & Kroah-Hartman, G. (2005). *Linux Device Drivers* (3rd ed.). O'Reilly Media.
    *   Cited for: Loadable Kernel Modules (Chapter 2), Device Driver Models & Interfaces (various chapters).
*   "Loadable kernel module" - Wikipedia: [https://en.wikipedia.org/wiki/Loadable_kernel_module](https://en.wikipedia.org/wiki/Loadable_kernel_module)
    *   Cited for: Loadable Kernel Modules.
*   Microsoft Docs. "WDF Driver Development Guide". [https://docs.microsoft.com/en-us/windows-hardware/drivers/wdf/](https://docs.microsoft.com/en-us/windows-hardware/drivers/wdf/)
    *   Cited for: Device Driver Models & Interfaces.

### Hardware Abstraction & Boot Process:

*   "Hardware abstraction layer" - Wikipedia: [https://en.wikipedia.org/wiki/Hardware_abstraction_layer](https://en.wikipedia.org/wiki/Hardware_abstraction_layer)
    *   Cited for: Hardware Abstraction Layers (HALs).
*   Bovet, D. P., & Cesati, M. (2005). *Understanding the Linux Kernel* (3rd ed.). O'Reilly Media.
    *   Cited for: Bootloader Sequences & Early Kernel Initialization (Chapter 3).
*   "Booting" - Wikipedia: [https://en.wikipedia.org/wiki/Booting](https://en.wikipedia.org/wiki/Booting)
    *   Cited for: Bootloader Sequences & Early Kernel Initialization.
*   Linux Kernel Documentation: `Documentation/filesystems/initramfs.txt` (or similar paths in kernel source).
    *   Cited for: Initramfs/Initrd and Early User Space.
*   "initramfs" - ArchWiki: [https://wiki.archlinux.org/title/Initramfs](https://wiki.archlinux.org/title/Initramfs)
    *   Cited for: Initramfs/Initrd and Early User Space.
