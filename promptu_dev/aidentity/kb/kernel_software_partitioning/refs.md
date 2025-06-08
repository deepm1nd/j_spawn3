# References for Kernel Software Partitioning Strategies - L1

**Topic:** Best practices for software partitioning within OS kernels (e.g., monolithic vs. microkernel vs. hybrid approaches, loadable kernel modules, driver models). Include common patterns in boot sequences and initialization processes that support modular kernel design.
**Level:** L1
**Date:** 2024-05-24

## Kernel Architecture and General Partitioning

*   **Wikipedia contributors. (2024, June 2). *Microkernel*. In Wikipedia, The Free Encyclopedia.** (Previously reviewed in "Interface-Based Design Best Practices" & "OS Software Partitioning" research) Retrieved 06:12, June 2, 2024, from [https://en.wikipedia.org/w/index.php?title=Microkernel&oldid=1293528771](https://en.wikipedia.org/w/index.php?title=Microkernel&oldid=1293528771)
    *   Key Takeaways: Discusses microkernel, monolithic, and hybrid kernel architectures, which are fundamental approaches to kernel software partitioning. Highlights how services are separated (or not) from the core kernel.
*   **Wikipedia contributors. (2024, May 24). *Modular programming*. In Wikipedia, The Free Encyclopedia.** (Previously reviewed in "Interface-Based Design Best Practices" research) Retrieved 17:16, May 24, 2024, from [https://en.wikipedia.org/w/index.php?title=Modular_programming&oldid=1292002975](https://en.wikipedia.org/w/index.php?title=Modular_programming&oldid=1292002975)
    *   Key Takeaways: General principles of modularity directly applicable to kernel design, such as separation of concerns and well-defined interfaces between kernel subsystems.

## Driver Models and Loadable Kernel Modules

*   **The Linux Kernel Documentation. *The Linux Kernel Device Model Overview*.** Retrieved from [https://www.kernel.org/doc/html/latest/driver-api/driver-model/overview.html](https://www.kernel.org/doc/html/latest/driver-api/driver-model/overview.html)
    *   Key Takeaways: Explains the unified device model in Linux, the role of `struct device`, how bus layers and device-specific drivers interact, and the abstraction provided for modular driver development. Mentions `sysfs` for exporting the model.
*   **Wikipedia contributors. (2025, February 1). *Loadable kernel module*. In Wikipedia, The Free Encyclopedia.** Retrieved 01:36, February 1, 2025, from [https://en.wikipedia.org/w/index.php?title=Loadable_kernel_module&oldid=1273158506](https://en.wikipedia.org/w/index.php?title=Loadable_kernel_module&oldid=1273158506)
    *   Key Takeaways: Defines LKMs, their advantages (flexibility, reduced base kernel size), how they extend kernel functionality (drivers, filesystems), and implementations across various OSes (Linux, FreeBSD, macOS, Windows). Also touches on binary compatibility and security aspects.

## Boot Sequences and Initialization

*   **Wikipedia contributors. (2025, April 6). *Booting process of Linux*. In Wikipedia, The Free Encyclopedia.** Retrieved 02:32, April 6, 2025, from [https://en.wikipedia.org/w/index.php?title=Booting_process_of_Linux&oldid=1284183053](https://en.wikipedia.org/w/index.php?title=Booting_process_of_Linux&oldid=1284183053)
    *   Key Takeaways: Describes the multi-stage Linux boot process, including firmware (BIOS/UEFI), bootloader (GRUB), kernel loading, and importantly, the role of the **initial RAM disk (initrd/initramfs)** in loading kernel modules early in the boot process to support access to the root filesystem and other hardware. Explains how this allows for a more modular kernel. Also covers the transition to the user-space init process.

## Related Concepts (from previous OS Partitioning research)

*   **User space vs. Kernel space separation:** Fundamental to all kernel designs, defining the most basic partition. (Covered in `promptu_dev/kb/os_software_partitioning/expert_L1.md`)
*   **System Call Interface:** The primary interface between user space and the kernel, regardless of internal kernel partitioning. (Covered in `promptu_dev/kb/os_software_partitioning/expert_L1.md`)
