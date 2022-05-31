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
