FROM debian:jessie

RUN perl -pi -e 's/deb.debian.org/ftp.jp.debian.org/g' /etc/apt/sources.list
RUN apt-get update
RUN apt-get -y install aptitude locales emacs24-nox python curl git byobu autoconf file g++ gcc libc-dev make pkg-config re2c ca-certificates libedit2 libsqlite3-0 libxml2 xz-utils libicu52 unzip libcurl4-openssl-dev libedit-dev libsqlite3-dev libssl-dev libxml2-dev libicu-dev bzip2 supervisor sudo openjdk-7-jdk bison libjpeg-dev libpng-dev libxpm-dev libgif-dev libmcrypt-dev libreadline-dev libtidy-dev libxslt-dev libbz2-dev

EXPOSE 22

RUN useradd -m -d /home/docker -u 1000 -s /bin/bash docker
RUN cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
RUN echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen
RUN echo 'ja_JP.UTF-8 UTF-8' >> /etc/locale.gen
RUN locale-gen

RUN mkdir /home/docker/.emacs.d
ADD init.el /home/docker/.emacs.d/init.el
ADD Cask /home/docker/.emacs.d/Cask

RUN curl -fsSL https://raw.githubusercontent.com/cask/cask/master/go | python
RUN mv /root/.cask /home/docker

RUN git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
RUN git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
RUN mv /root/.rbenv /usr/local/rbenv

RUN git clone https://github.com/yyuu/pyenv.git ~/.pyenv
RUN mv /root/.pyenv /usr/local/pyenv

RUN git clone https://github.com/CHH/phpenv.git /tmp/phpenv
RUN /tmp/phpenv/bin/phpenv-install.sh
RUN git clone https://github.com/CHH/php-build.git /root/.phpenv/plugins/php-build
RUN mv /root/.phpenv /usr/local/phpenv
RUN perl -pi -e 's|/root/|/home/docker/|g' /usr/local/phpenv/bin/phpenv
RUN perl -pi -e 's|--with-mysql=mysqlnd\n||g' /usr/local/phpenv/plugins/php-build/share/php-build/default_configure_options

RUN echo 'export RBENV_ROOT=/usr/local/rbenv' >> /etc/profile.d/rbenv.sh
RUN echo 'export PATH=$RBENV_ROOT/bin:$PATH' >> /etc/profile.d/rbenv.sh
RUN echo 'eval "$(rbenv init -)"' /etc/profile.d/rbenv.sh

RUN echo 'export PYENV_ROOT=/usr/local/pyenv' >> /etc/profile.d/pyenv.sh
RUN echo 'export PATH=$PYENV_ROOT/bin:$PATH' >> /etc/profile.d/pyenv.sh
RUN echo 'eval "$(pyenv init -)"' /etc/profile.d/pyenv.sh

RUN echo 'export PHPENV_ROOT=/usr/local/phpenv' >> /etc/profile.d/phpenv.sh
RUN echo 'export PATH=$PHPENV_ROOT/bin:$PATH' >> /etc/profile.d/phpenv.sh
RUN echo 'eval "$(phpenv init -)"' /etc/profile.d/phpenv.sh

RUN echo 'export TERM=xterm-256color' >> /home/docker/.bashrc
RUN echo 'export PATH=$PATH:$HOME/.cask/bin' >> /home/docker/.bashrc

RUN echo 'cd $HOME'

RUN chown -R docker:docker /home/docker
RUN echo 'docker ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN echo 'docker:docker' | chpasswd
RUN su docker -c 'cd ~/.emacs.d ; ~/.cask/bin/cask'

RUN . /etc/profile ; /usr/local/rbenv/bin/rbenv install 2.4.0
RUN . /etc/profile ; /usr/local/rbenv/bin/rbenv install jruby-9.1.7.0
RUN . /etc/profile ; /usr/local/rbenv/bin/rbenv global 2.4.0
RUN . /etc/profile ; /usr/local/pyenv/bin/pyenv install 3.6.0
RUN . /etc/profile ; /usr/local/pyenv/bin/pyenv global 3.6.0
RUN . /etc/profile ; /usr/local/phpenv/bin/phpenv install 7.1.2 ; echo 0
RUN . /etc/profile ; /usr/local/phpenv/bin/phpenv global 7.1.2

RUN chown -R docker:docker /usr/local/rbenv
RUN chown -R docker:docker /usr/local/pyenv
RUN chown -R docker:docker /usr/local/phpenv

CMD ["/usr/bin/supervisord"]