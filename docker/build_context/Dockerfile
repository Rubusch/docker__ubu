################################################################################
## ATTENTION: standalone container
##
################################################################################
FROM ubuntu:22.04 AS ubu2204
MAINTAINER Lothar Rubusch <l.rubusch@gmail.com>
ENV DEBIAN_FRONTEND=noninteractive


################################################################################
## the USER is needed to make it available as --build-arg
ARG USER="${USER}"
ARG UID="${UID}"
ARG GID="${GID}"
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
	mercurial \
	libc6-i386 \
	aptitude \
	bsdmainutils \
	i2c-tools \
	expect \
	device-tree-compiler

## xilinx needed packages
RUN apt-get install -y \
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
	google-perftools \
	libegl1-mesa \
	libsdl1.2-dev \
	pylint \
	python3 \
	python3-pip \
	python3-git \
	python3-jinja2 \
	python3-pexpect \
	perl \
	liberror-perl \
	gnupg \
	haveged \
	lib32stdc++6 \
	libgtk2.0-0:i386 \
	libfontconfig1:i386 \
	libx11-6:i386 \
	libxext6:i386 \
	libxrender1:i386 \
	libsm6:i386 \
	xinetd \
	xterm \
	texinfo \
	zlib1g-dev \
	zlib1g:i386 \
	screen \
	putty \
	pax \
	mtd-utils \
	xtrans-dev \
	libxcb-randr0-dev \
	libxcb-xtest0-dev \
	libxcb-xinerama0-dev \
	libxcb-shape0-dev \
	libxcb-xkb-dev \
	ncurses-dev \
	libncurses5 \
	libncurses5-dev \
	libncursesw5-dev \
	libncurses5:i386 \
	libtinfo5 \
	libstdc++6:i386 \
	dpkg-dev:i386 \
	ocl-icd-libopencl1 \
	opencl-headers \
	ocl-icd-opencl-dev

## GUI IDEs may require the following...
RUN apt-get install -y \
	'^libxcb.*-dev' \
	libx11-xcb-dev \
	libglu1-mesa-dev \
	libxrender-dev \
	libxi-dev \
	libxkbcommon-dev \
	libxkbcommon-x11-dev

## development
RUN apt-get install -y \
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
        git-all


################################################################################
## system settings

## set shell to bash instead of dash non-interactively
## ref: https://superuser.com/questions/715722/how-to-do-dpkg-reconfigure-dash-as-bash-automatically
RUN echo "dash dash/sh boolean false" | debconf-set-selections
RUN DEBIAN_FRONTEND=noninteractive dpkg-reconfigure dash

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
RUN usermod -a -G sudo "$USER"
RUN sed -i '/%sudo/s/.*/%sudo   ALL=NOPASSWD: ALL/' /etc/sudoers
RUN mkdir -p /usr/local
RUN chown -R "${USER}":"${USER}" /usr/local
RUN mkdir -p /opt
RUN chown -R "${USER}":"${USER}" /opt
RUN adduser "$USER" dialout


################################################################################
## tftp server
USER root
RUN echo -e '\
service tftp\n\
    {\n\
    protocol = udp\n\
    port = 69\n\
    socket_type = dgram\n\
    wait = yes\n\
    user = nobody\n\
    server = /usr/sbin/in.tftpd\n\
    server_args = /tftpboot\n\
    disable = no\n\
    }\n\
' > /etc/xinetd.d/tftp

RUN mkdir /tftpboot
RUN chmod -R 777 /tftpboot
RUN chown -R nobody /tftpboot
RUN /etc/init.d/xinetd stop tftp
RUN /etc/init.d/xinetd start tftp


################################################################################
## configs
USER ${USER}
WORKDIR /home/${USER}
RUN rm -rf .local
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
CMD ["/bin/cat", "/etc/issue"]
