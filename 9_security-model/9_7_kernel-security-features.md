## 9.7\. Kernel Security Features

The Android Sandbox includes features that use the Security-Enhanced Linux
(SELinux) mandatory access control (MAC) system, seccomp sandboxing, and other
security features in the Linux kernel. SELinux or any other security features
implemented below the Android framework:

*   MUST maintain compatibility with existing applications.
*   MUST NOT have a visible user interface when a security violation is
detected and successfully blocked, but MAY have a visible user interface when
an unblocked security violation occurs resulting in a successful exploit.
*   MUST NOT be user or app developer configurable.

If any API for configuration of policy is exposed to an application that can
affect another application (such as a Device Administration API), the API MUST
NOT allow configurations that break compatibility.

Devices MUST implement SELinux or, if using a kernel other than Linux, an
equivalent mandatory access control system. Devices MUST also meet the
following requirements, which are satisfied by the reference implementation in
the upstream Android Open Source Project.

Device implementations:

*   MUST set SELinux to global enforcing mode.
*   MUST configure all domains in enforcing mode. No permissive mode domains
are allowed, including domains specific to a device/vendor.
*   MUST NOT modify, omit, or replace the neverallow rules present within the
system/sepolicy folder provided in the upstream Android Open Source Project
(AOSP) and the policy MUST compile with all neverallow rules present, for both
AOSP SELinux domains as well as device/vendor specific domains.
*   MUST split the media framework into multiple processes so that it
is possible to more narrowly grant access for each process as
[described](https://source.android.com/devices/media/framework-hardening.html#arch_changes)
in the Android Open Source Project site.

Device implementations SHOULD retain the default SELinux policy provided in the
system/sepolicy folder of the upstream Android Open Source Project and only
further add to this policy for their own device-specific configuration. Device
implementations MUST be compatible with the upstream Android Open Source
Project.

Devices MUST implement a kernel application sandboxing mechanism which allows
filtering of system calls using a configurable policy from multithreaded
programs. The upstream Android Open Source Project meets this requirement
through enabling the seccomp-BPF with threadgroup synchronization (TSYNC) as
described [in the Kernel Configuration section of source.android.com](http://source.android.com/devices/tech/config/kernel.html#Seccomp-BPF-TSYNC).

Kernel integrity and self-protection features are integral to Android
security. Device implementations:

*   MUST implement kernel stack buffer overflow protections
(e.g. `CONFIG_CC_STACKPROTECTOR_STRONG`).
*   MUST implement strict kernel memory protections where executable code
is read-only, read-only data is non-executable and non-writable, and writable
data is non-executable (e.g. `CONFIG_DEBUG_RODATA` or `CONFIG_STRICT_KERNEL_RWX`).
*   are STRONGLY RECOMMENDED to keep kernel data which is written only during
initialization marked read-only after initialization (e.g. `__ro_after_init`).
*   are STRONGLY RECOMMENDED to implement static and dynamic object size bounds
checking of copies between user-space and kernel-space
(e.g. `CONFIG_HARDENED_USERCOPY`).
*   are STRONGLY RECOMMENDED to never execute user-space memory when
running in the kernel (e.g. hardware PXN, or emulated via `CONFIG_CPU_SW_DOMAIN_PAN` or
`CONFIG_ARM64_SW_TTBR0_PAN`).
*   are STRONGLY RECOMMENDED to never read or write user-space memory
in the kernel outside of normal usercopy access APIs
(e.g. hardware PAN, or emulated via `CONFIG_CPU_SW_DOMAIN_PAN` or `CONFIG_ARM64_SW_TTBR0_PAN`).
*   are STRONGLY RECOMMENDED to randomize the layout of the kernel code
and memory, and to avoid exposures that would compromise the randomization
(e.g. `CONFIG_RANDOMIZE_BASE` with bootloader entropy via the [`/chosen/kaslr-seed Device Tree node`](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/devicetree/bindings/chosen.txt)
or [`EFI_RNG_PROTOCOL`](https://docs.microsoft.com/en-us/windows-hardware/drivers/bringup/efi-rng-protocol)).