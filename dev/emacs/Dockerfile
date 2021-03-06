FROM debian:jessie

RUN apt-get update
RUN apt-get -y install aptitude build-essential git python openssh-server sudo supervisor locales
RUN echo 'en_US.UTF-8 UTF-8 > /etc/locale.gen'
RUN echo 'ja_JP.UTF-8 UTF-8 >> /etc/locale.gen'
RUN apt-get -y install libncurses-dev gcc g++ make wget xzip
 
WORKDIR /usr/src
RUN wget http://ftp.jaist.ac.jp/pub/GNU/emacs/emacs-25.1.tar.xz
RUN tar xJf emacs-25.1.tar.xz
WORKDIR /usr/src/emacs-25.1
RUN ./configure \
    --without-toolkit-scroll-bars \
    --without-xaw3d \
    --without-sound \
    --without-pop \
    --without-xpm \
    --without-tiff \
    --without-rsvg \
    --without-gconf \
    --without-gsettings \
    --without-selinux \
    --without-gpm \
    --without-makeinfo \
    --without-imagemagick \
    --without-x && make install

EXPOSE 22

RUN useradd -m -d /home/docker -u 1000 -s /bin/bash docker
RUN chown -R docker:docker /home/docker
RUN mkdir /var/run/sshd
RUN echo 'docker ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN echo 'docker:docker' | chpasswd

RUN git clone https://github.com/cask/cask.git /usr/local/cask
RUN echo 'export PATH=$PATH:/usr/local/cask/bin' > /etc/profile.d/cask.sh

ADD supervisord.conf /etc/supervisor/supervisord.conf
ADD sshd.conf /etc/supervisor/conf.d/sshd.conf

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
