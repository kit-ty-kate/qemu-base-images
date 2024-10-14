The FreeBSD image is basically:
```
RUN qemu-img create -f qcow2 FreeBSD-14.1-RELEASE-amd64.qcow2 4G
RUN curl -LO https://download.freebsd.org/ftp/releases/ISO-IMAGES/14.1/FreeBSD-14.1-RELEASE-amd64-bootonly.iso.xz
RUN unxz FreeBSD-14.1-RELEASE-amd64-bootonly.iso.xz
RUN qemu-system-x86_64 -cdrom FreeBSD-14.1-RELEASE-amd64-bootonly.iso -drive file=FreeBSD-14.1-RELEASE-amd64.qcow2 -smp 2 -m 2G -machine q35
INSTALL default
EXCEPT hostname=freebsd
EXCEPT distribution=[] (no kernel-dbg, no lib32)
EXCEPT partinioning=Auto (UFS)
EXCEPT password=password
EXCEPT timezone=UTC
EXCEPT system-configuration=[sshd] (no dumpdev)
EXCEPT add-user-account=no
POWEROFF
RUN qemu-system-x86_64 -drive file=FreeBSD-14.1-RELEASE-amd64.qcow2 -smp 2 -m 2G -machine q35
RUN echo 'autoboot_delay="-1"' >> /boot/defaults/loader.conf
RUN echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config
RUN echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config
RUN echo 'UsePAM no' >> /etc/ssh/sshd_config
```

The OpenBSD image is basically:
```
RUN qemu-img create -f qcow2 OpenBSD-7.6-amd64.qcow2 4G
RUN curl -LO https://cdn.openbsd.org/pub/OpenBSD/7.6/amd64/install76.iso
RUN qemu-system-x86_64 -cdrom install76.iso -drive file=OpenBSD-7.6-amd64.qcow2 -smp 2 -m 2G -machine q35
INSTALL default
EXCEPT hostname=openbsd
EXCEPT ipv6=autoconf
EXCEPT password=password
EXCEPT x-window-system=no
EXCEPT allow-root-ssh-login=yes
EXCEPT timezone=UTC
EXCEPT edit-auto-layout=[d b, d d, d e, c a, 8388544, q]
EXCEPT distribution=-game* -x*
EXCEPT no-sha256-verif=yes
POWEROFF
RUN qemu-system-x86_64 -drive file=OpenBSD-7.6-amd64.qcow2 -smp 2 -m 2G -machine q35
```

The NetBSD image is basically:
```
RUN qemu-img create -f qcow2 NetBSD-10.0-amd64.qcow2 4G
RUN curl -LO https://cdn.netbsd.org/pub/NetBSD/NetBSD-10.0/images/NetBSD-10.0-amd64.iso
RUN qemu-system-x86_64 -cdrom NetBSD-10.0-amd64.iso -drive file=NetBSD-10.0-amd64.qcow2 -smp 2 -m 2G -machine q35
INSTALL default
EXCEPT shall-we-continue=yes
EXCEPT partitioning=use-default-partition-sizes
EXCEPT partition=[delete swap, set size of / to -1 (max)]
EXCEPT shall-we-continue=yes
EXCEPT distribution=custom-installation
EXCEPT distribution-set=[a c d e f i]
EXCEPT password=password
EXCEPT entropy-generation=<random characters>
EXCEPT configure-network=[hostname: netbsd, dns: <empty>]
EXCEPT enable-installation-binary-packages=install
EXCEPT enable-sshd=YES
EXCEPT enable-raidframe=NO
POWEROFF
RUN qemu-system-x86_64 -drive file=NetBSD-10.0-amd64.qcow2 -smp 2 -m 2G -machine q35
RUN echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config
```
