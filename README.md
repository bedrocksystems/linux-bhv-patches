Applying BHV Patches
====================

In order to apply the BHV patches, they must be applied to the correct upstream Linux kernel
version.  Each patch is named based on the following naming schema:

[KERNEL_PATCH_VERSION]-bhv.patch

For example, if the BHV patch is meant for the v5.10.79 kernel version, the patch name would
be `v5.10.79-bhv.patch`.

The Linux kernel source can be downloaded from:

<https://www.kernel.org/>

or if you prefer to work with git, the upstream repository can be found here:

<git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git>

In order to apply the patch, simply copy the patch file into the Linux kernel source folder
(of appropriate version), then apply the patch with:

`patch -p0 < v5.10.79-bhv.patch`

In the example above, we apply the patch for the 5.10.79 Linux kernel, you may have to
subsitute the kernel version number for your particular patch.

Configuring the Kernel
======================

In order to configure the kernel, you will need to enable BHV VAS support, this is enabled
by enabling the `BHV_VAS` configuration option in the patched Linux kernel.

Optionally, you can choose how the Linux kernel should respond on a failed hypercall.  With
the `BHV_PANIC_ON_FAIL` configuation option, you can determine whether the kernel will panic
or not.  For a production system it is generally recommnended that this option be set.

Here is a full list of additional BRASS kernel config options:

- `BHV_ALLOW_SELINUX_GUEST_ADMIN`: Allow the guest to perform SELinux administration if the host disabled guestpolicy support.  This should be set to `n` only if you intend to manage SELinux policy from the host.

- `BHV_CONST_CALL_USERMODEHELPER_KERNEL`: Make all paths in the kernel that are passed to `call_usermodehelper` constant such that they cannot be updated. This should be set to `y` on production systems if you do not intend to update default paths.

- `BHV_CONST_CALL_USERMODEHELPER_MODULES`: Make all paths in modules that are passed to `call_usermodehelper` constant such that they cannot be updated. This should be set to `y` on production systems if you do not intend to update default paths.

- `BHV_VAS_DEBUG`: Build BHV guest support with DEBUG information. This should be set to `n` on production systems.

- Ensure that the `bhv` LSM is enabled by adding it in `Security options -> Ordered list of enabled LSMs`.

- `MODULE_SIG`: Enable module signature verification. This should be set to `y` in production systems.

- `MODULE_SIG_FORCE`: Require modules to be validly signed. This should be set to `y` in production systems.

- `EXT4_FS` and `XFS_FS`: Support for Extended 4 (ext4) and XFS filesystems. These are set automatically to `y` by BHV_VAS.

- `MEMCG` and `MEM_NS`: These should be set to `y` if you intend to use strong isolation.

- `BHV_FREEZE_MEMORY_AFTER_BOOT`: Automatically issue memory freeze hypercalls after the system boots. Among other consequences, this prevents kernel modules from being loaded after the system is booted.

- `BHV_LOCKDOWN`: Automatically turns on our most secure settings. As of this writing, this:
    - enables `BHV_FREEZE_MEMORY_AFTER_BOOT`, `BHV_PANIC_ON_FAIL` and `BHV_CONST_MODPROBE_PATH`;
    - disables various kernel tracing options (`FTRACE`, `KPROBES`), `BPF_JIT`, `DEBUG_KERNEL`

- `BHV_FORCE_STRICT_FILEOPS`: Enable strict mode checking for supported file operations instances. This flags the usage of unsupported file operations instances violations. Note: This mode can be enabled as well by passing `bhv_strict_fileops` to the kernel command line as boot parameter.

- `BHV_VAULT_SPACES`: Enable the spaces-based vault. The spaces-based vault intends to guard any code-patching related in-guest activities, including alternative instructions, jump labels, static calls, static call keys, and tracepoints.

Finally, using `KPROBES` within the Linux kernel will cause BHV kernel integrity violations. To ensure this does not happen, these tracing features can be configured out of the kernel by disabling `KPROBES`.