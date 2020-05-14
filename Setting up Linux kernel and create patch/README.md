# Setting up Linux Kernel and create a patch to submit on mailing list. 

This is a step by step guide for building the kernel and create a patch for it and send it to kernel mailing-list. 

This is created because i didn't find any thing simple about this on web.


# Cloning the stable kernel git

```sh
$ git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git linux_stable
$ cd linux_stable
$ git branch -a | grep linux-5
    remotes/origin/linux-5.0.y
    remotes/origin/linux-5.1.y
    remotes/origin/linux-5.2.y
​$ git checkout linux-5.2.y
```


# Copying the Configuration for Current Kernel from /boot

> ***Note: copying the configuration for your current kernel from /proc/config.gz or /boot***

```sh
$ ls /boot
config-4.19.0-8-amd64       initrd.img-4.19.0-8-amd64       System.map-5.4.0-0.bpo.3-amd64
config-5.2.21               initrd.img-5.2.21               vmlinuz-4.19.0-8-amd64
config-5.4.0-0.bpo.3-amd64  initrd.img-5.4.0-0.bpo.3-amd64  vmlinuz-5.2.21
efi                         System.map-4.19.0-8-amd64       vmlinuz-5.4.0-0.bpo.3-amd64
grub                        System.map-5.2.21

$ cp /boot/<config-5.4.0-0.bpo.3-amd64> .config

```

# Compiling the Kernel

Run the following command to generate a kernel configuration file based on the current configuration. This step is important to configure the kernel, which has a good chance to work correctly on your system. You will be prompted to tune the configuration to enable new features and drivers that have been added since Ubuntu snapshot the kernel from the mainline. make all will invoke make oldconfig in any case. I am showing these two steps separately just to call out the configuration file generation step.

```sh
$ make oldconfig
```

Another way to trim down the kernel and tailor it to your system is by using make localmodconfig. This option creates a configuration file based on the list of modules currently loaded on your system.

```sh
$ lsmod > /tmp/my-lsmod
$ make LSMOD=/tmp/my-lsmod localmodconfig
```

Once this step is complete, it is time to compile the kernel. Using the -j option helps the compiles go faster. The -j option specifies the number of jobs (make commands) to run simultaneously:

```sh
$ make -j3 all
```
or, rather 
```sh
$ make -j$(nproc) all
```

when the kernel is compiled, let's install it...

```sh
$ su -c "make modules_install install"
```

above command will install the new kernel.

Now, run `update-grub` to add the entry of the new kernel to the GRUB menu.

# Save some logs for debugging

Before booting to the new kernel create log files from the current kernel to compare with the new one for errors and warnings, if any.

```sh
$ dmesg -t > dmesg_current
$ dmesg -t -k > dmesg_kernel
$ dmesg -t -l emerg > dmesg_current_emerg
$ dmesg -t -l alert > dmesg_current_alert
$ dmesg -t -l crit > dmesg_current_alert
$ dmesg -t -l err > dmesg_current_err
$ dmesg -t -l warn > dmesg_current_warn

```
here `-t` flag helps to generate dmesg logs without timestamps.

if `dmesg` is clean i.e without emerg, alert, crit, and err level messages then we are good to go. If these ocuurs then it's probably due to some hardwre and/or kernel problem.

# Boot the new kernel

Last steps before booting new kernel:

1) Increse the `GRUB_TIMEOUT` and set it to 10.
2) comment out `GRUB_TIMEOUT_STYLE=hidden`

make your configuration like this:

```
GRUB_DEFAULT=0
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet"
GRUB_CMDLINE_LINUX=""
```
after saving it

do

```sh$ sudo update-grub
```

> ***Note: If the newly installed kernel fails to boot then go back to the already installed kernel and find issues why it's failed.***

After booting into the new kernel again save the logs using `dmesg` and compare it with the old kernel.



(sending of patches will be coveres in next commit...)


Author: Anmol
