Applying BHV Patches
====================

In order to apply the BHV patches, they must be applied to the correct upstream Linux kernel
version.  Each patch is named based on the following naming schema:

[KERNEL_PATCH_VERSION]-bhv.patch

For example, if the BHV patch is meant for the v5.10.79 kernel version, the patch name would
be `v5.10.79-bhv.patch`.

The Linux kernel source can be downloaded from:

https://www.kernel.org/

or if you prefer to work with git, the upstream repository can be found here:

git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git

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

Finally, using tracing features within the Linux kernel will cause BHV kernel integrity
violations.  To ensure this does not happen, these tracing features can be configured out
of the kernel by disabling `ENABLE_DEFAULT_TRACERS` and `FTRACE`.