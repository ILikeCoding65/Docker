FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

ENV container docker
STOPSIGNAL SIGRTMIN+3
VOLUME ["/sys/fs/cgroup", "/tmp", "/run", "/run/lock"]

RUN apt-get update \
    && apt-get dist-upgrade \
    && apt-get install ca-certificates curl gnupg systemd inetutils-syslogd apt-utils unattended-upgrades tzdata

RUN apt-get install \
    iproute2 iputils-ping avahi-daemon libnss-mdns

RUN sed -i 's?//Unattended-Upgrade::Automatic-Reboot "false"?Unattended-Upgrade::Automatic-Reboot "true"?g' /etc/apt/apt.conf.d/50unattended-upgrades && \
    sed -i 's?//Unattended-Upgrade::Automatic-Reboot-Time "02:00"?Unattended-Upgrade::Automatic-Reboot-Time "02:00"?g' /etc/apt/apt.conf.d/50unattended-upgrades && \
    sed -i 's?//        "${distro_id}:${distro_codename}-updates"?      "${distro_id}:${distro_codename}-updates"?g' /etc/apt/apt.conf.d/50unattended-upgrades

RUN sed -i '/Unattended-Upgrade::Verbose/s?^//??' /etc/apt/apt.conf.d/50unattended-upgrades && \
    sed -i '/Unattended-Upgrade::Verbose/s?false?true?' /etc/apt/apt.conf.d/50unattended-upgrades

RUN systemctl mask dev-hugepages.mount
RUN systemctl mask ntp.service
RUN systemctl mask systemd-sysusers
RUN systemctl mask sys-fs-fuse-connections.mount
RUN systemctl mask org.freedesktop.timesync1

RUN touch /.dockerenv

CMD ["/sbin/init", "--log-target=journal"]
