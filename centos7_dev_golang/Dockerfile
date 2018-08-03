FROM centos:7

ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]

RUN yum update -y && yum clean all

# Install SSH
RUN yum install -y openssh-server && yum clean all
RUN mkdir /var/run/sshd
RUN echo 'root:root' | chpasswd
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

# Install Development tools
RUN yum groupinstall -y 'Development Tools' && yum clean all

# Install wget
RUN yum install -y wget && yum clean all
# Download golang
RUN cd /root
RUN wget https://dl.google.com/go/go1.10.3.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.10.3.linux-amd64.tar.gz
RUN export PATH=$PATH:/usr/local/go/bin

