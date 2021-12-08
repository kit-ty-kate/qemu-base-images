The FreeBSD image is basically:
```
INSTALL_FROM https://download.freebsd.org/ftp/releases/ISO-IMAGES/13.0/FreeBSD-13.0-RELEASE-amd64-bootonly.iso.xz
RUN pw usermod -n root -w none
RUN echo 'autoboot_delay="-1"' >> /boot/defaults/loader.conf
RUN echo "PermitEmptyPasswords yes" >> /etc/ssh/sshd_config
RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
RUN echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config
RUN sed -i '' -e 's/^auth        required    pam_unix.so     no_warn try_first_pass$/auth        required    pam_unix.so     no_warn try_first_pass nullok/' /etc/pam.d/sshd
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
