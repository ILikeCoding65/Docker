FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

ENV container docker
STOPSIGNAL SIGRTMIN+3

VOLUME ["/sys/fs/cgroup", "/tmp", "/run", "/run/lock"]

RUN apt-get update \
    && apt-get install ca-certificates curl gnupg systemd inetutils-syslogd \
    && apt-get upgrade

COPY syslogd.conf /etc/syslogd.conf

RUN apt-get install \
    iproute2

CMD ["/sbin/init", "--log-target=journal"]