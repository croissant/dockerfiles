FROM debian:jessie

RUN apt-get update
RUN apt-get -y install aptitude build-essential git python openssh-server sudo supervisor locales
RUN echo 'en_US.UTF-8 UTF-8 > /etc/locale.gen'
RUN echo 'ja_JP.UTF-8 UTF-8 >> /etc/locale.gen'
RUN apt-get -y install libgtk-3-dev libncurses-dev libwebkitgtk-3.0-dev gcc g++ make

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

ADD supervisord.conf /etc/supervisor/supervisord.conf
ADD sshd.conf /etc/supervisor/conf.d/sshd.conf

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]

