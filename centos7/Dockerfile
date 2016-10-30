FROM centos:7

RUN yum -y update
RUN yum -y groupinstall "Development Tools"
RUN yum -y install initscripts MAKEDEV git python openssh-server sudo epel-release
RUN yum -y install --enablerepo=epel supervisor
RUN yum -y swap fakesystemd sysetmd
EXPOSE 22

RUN useradd -m -d /home/docker -u 1000 -s /bin/bash docker
RUN mkdir /home/docker/.ssh
RUN chmod 700 /home/docker/.ssh
ADD authorized_keys /home/docker/.ssh/
RUN chmod 600 /home/docker/.ssh/authorized_keys
RUN chown -R docker:docker /home/docker
RUN mv /home/docker/.ssh /home/docker/ssh && mv /home/docker/ssh /home/docker/.ssh
RUN mkdir /var/run/sshd
RUN echo 'docker ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN echo 'docker:docker' | chpasswd
RUN /usr/sbin/sshd-keygen
ADD sshd.conf /etc/supervisord.d/sshd.ini

ENTRYPOINT ["/usr/bin/supervisord", "-n"]
