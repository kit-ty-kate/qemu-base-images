The FreeBSD image is basically:
```
RUN qemu-img create -f qcow2 FreeBSD-13.2-RELEASE-amd64.qcow2 4G
RUN curl -LO https://download.freebsd.org/ftp/releases/ISO-IMAGES/13.2/FreeBSD-13.2-RELEASE-amd64-bootonly.iso.xz
RUN unxz FreeBSD-13.2-RELEASE-amd64-bootonly.iso.xz
RUN qemu-system-x86_64 -cdrom FreeBSD-13.2-RELEASE-amd64-bootonly.iso -drive file=FreeBSD-13.2-RELEASE-amd64.qcow2
INSTALL default
EXCEPT hostname=freebsd
EXCEPT distribution=[] (no kernel-dbg, no lib32)
EXCEPT filesystem=UFS
EXCEPT password=password
EXCEPT timezone=UTC
EXCEPT system-configuration=[sshd] (no dumpdev)
EXCEPT add-user-account=no
RUN pw usermod -n root -w none
RUN echo 'autoboot_delay="-1"' >> /boot/defaults/loader.conf
RUN echo 'PermitEmptyPasswords yes' >> /etc/ssh/sshd_config
RUN echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config
RUN echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config
RUN echo 'UsePAM no' >> /etc/ssh/sshd_config
```

The OpenBSD image is basically:
```
INSTALL_FROM https://cdn.openbsd.org/pub/OpenBSD/7.0/amd64/install70.iso
RUN echo "PermitEmptyPasswords yes" >> /etc/ssh/sshd_config
RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
RUN echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config
RUN sed -i '' -e "s/root:.*:/root::/" /etc/master.passwd
RUN pwd_mkdb /etc/master.passwd
```
