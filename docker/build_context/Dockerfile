################################################################################
## ATTENTION: standalone container
##
################################################################################
FROM ubuntu:18.04 AS ubu1804
MAINTAINER Lothar Rubusch <l.rubusch@gmail.com>
ENV DEBIAN_FRONTEND=noninteractive

################################################################################
## the USER is needed to make it available as --build-arg
ARG USER="${USER}"
ARG UID="${UID}"
ARG GID="${GID}"
ARG DISPLAY="${DISPLAY}"
ENV MY_CONFIGS="/home/$USER/configs"

################################################################################
## set up apt fully functional
USER root
RUN dpkg --add-architecture i386

## NB: don't do "apt-get upgrade -y", for avoiding trouble
## incompatible through packaging solutions
RUN apt-get update && apt-get autoremove -y --purge

## container essentials
RUN apt-get install -y \
	locales \
	sudo \
	bc \
	libelf-dev \
	python3-all \
	python3-dialog \
	rsync \
	binutils \
	bzip2 \
	libc6-dev \
	fakeroot \
	udev \
	hdparm \
	xmlto \
	curl \
	bin86 \
	patch \
	aptitude \
	expect \
	device-tree-compiler \
	build-essential \
	flex \
	bison \
	autoconf \
	libtool \
	gcc \
	gcc-multilib \
	g++ \
	automake \
	make \
	wget \
	net-tools \
	iproute2 \
	socat \
	tftpd \
	iputils-ping \
	openssl \
	libssl-dev \
	openssh-server \
	libselinux1 \
	tar \
	unzip \
	gzip \
	xz-utils \
	cpio \
	tofrodos \
	debianutils \
	gawk \
	diffstat \
	chrpath \
	util-linux \
	sysvinit-utils \
	gnupg \
	libncurses-dev

## development
RUN apt-get install -y \
	screen \
	i2c-tools \
	cmake \
	apt-file \
	psmisc \
	u-boot-tools \
	tree \
	mc \
	colordiff \
	tig \
	dos2unix \
	x11-apps \
	vim \
	emacs \
	emacs-goodies-el \
	emacs-goodies-extra-el \
	exuberant-ctags \
	graphviz \
	doxygen \
	git-all \
	meld \
	file

RUN apt-get install -y \
	libffi-dev \
	libssl-dev \
	python3-dev \
	python3-pyqt5 \
	python3-pyqt5.qtwebengine \
	python3 \
	python3-pip \
	kas \
	zstd \
	lz4

################################################################################
## system settings

## xilinx - set shell to bash instead of dash non-interactively
## ref: https://superuser.com/questions/715722/how-to-do-dpkg-reconfigure-dash-as-bash-automatically
#RUN echo "dash dash/sh boolean false" | debconf-set-selections
#RUN DEBIAN_FRONTEND=noninteractive dpkg-reconfigure dash

## locale settings
USER root
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

################################################################################
## user settings
USER root
RUN groupadd -g "$GID" "$USER"
RUN useradd -m -u "$UID" -g "$GID" -s /bin/bash "$USER"

RUN echo "$USER  ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

RUN mkdir -p /usr/local
RUN chown -R "$USER":"$USER" /usr/local

RUN mkdir -p /opt
RUN chown -R "$USER":"$USER" /opt

RUN chown -R "$USER":"$USER" /home/$USER

RUN adduser "$USER" sudo
RUN adduser "$USER" dialout

##################################################################################
#### tftp server
##USER root
##RUN echo -e '\
##service tftp\n\
##    {\n\
##    protocol = udp\n\
##    port = 69\n\
##    socket_type = dgram\n\
##    wait = yes\n\
##    user = nobody\n\
##    server = /usr/sbin/in.tftpd\n\
##    server_args = /tftpboot\n\
##    disable = no\n\
##    }\n\
##' > /etc/xinetd.d/tftp
##
##RUN mkdir /tftpboot
##RUN chmod -R 777 /tftpboot
##RUN chown -R nobody /tftpboot
##RUN /etc/init.d/xinetd stop tftp
##RUN /etc/init.d/xinetd start tftp

################################################################################
## configs
USER $USER
WORKDIR /home/$USER

RUN ln -s /usr/local .local

## bash config
RUN rm -f /home/$USER/.bashrc && ln -s ${MY_CONFIGS}/.bashrc /home/$USER/

## vim config
RUN rm -f /home/$USER/.vimrc && ln -s ${MY_CONFIGS}/.vimrc /home/$USER/

## emacs config
RUN rm -f /home/$USER/.emacs && ln -s ${MY_CONFIGS}/.emacs /home/$USER/

## user
RUN echo "export USER=${USER}" >> /home/$USER/.profile

## ssh preparations
RUN mkdir -p /home/$USER/.ssh

################################################################################
## command mode
USER ${USER}
WORKDIR /home/$USER
COPY *.sh /usr/local/bin/
CMD ["/bin/bash", "/usr/local/bin/build.sh"]
