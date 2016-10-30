FROM centos:6

RUN yum -y update
RUN yum -y install initscripts MAKEDEV git python openssh-server sudo supervisor

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
ADD sshd.conf /etc/supervisor/conf.d/sshd.conf

#CMD /usr/bin/supervisord -n
#CMD ["/usr/sbin/sshd", "-D"]
CMD /sbin/init
