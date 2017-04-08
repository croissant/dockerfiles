FROM php:fpm

RUN apt-get update
RUN apt-get -y install aptitude \
                       build-essential \
                       git \
                       python \
                       openssh-server \
		       sudo \
		       supervisor \
		       nginx \
		       libicu-dev \
		       libxml2-dev \
		       libxslt-dev \
		       libpng-dev \
		       libpcre3-dev \
		       libtidy-dev \
		       libreadline-dev \
		       libedit-dev
RUN docker-php-ext-install pdo_mysql \
    			   mysqli \
			   mbstring \
			   intl \
			   soap \
			   exif \
			   gd \
			   zip \
			   sockets \
			   xsl \
			   tidy \
			   xmlrpc \
			   sysvsem \
			   sysvshm \
			   shmop \
			   pcntl \
			   readline \
			   bcmath

EXPOSE 22

RUN useradd -m -d /home/docker -u 1000 -s /bin/bash docker
RUN mkdir /var/run/sshd
RUN echo 'docker ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN echo 'docker:docker' | chpasswd

RUN curl https://getcomposer.org/composer.phar -o /usr/local/bin/composer ; chmod 755 /usr/local/bin/composer

ADD supervisord.conf /etc/supervisor/supervisord.conf
ADD sshd.conf /etc/supervisor/conf.d/sshd.conf

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]

