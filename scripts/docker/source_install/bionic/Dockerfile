FROM ubuntu:bionic

ENV DEBIAN_FRONTEND=noninteractive
ARG RELEASE=melodic

RUN apt update && apt upgrade && apt install -y --no-install-recommends \
        build-essential \
        cmake \
        freeglut3 \
        freeglut3-dev \
        g++ \
        gcc \
        git \
        gnupg2 \
        libgl1-mesa-dev \
        libglu1-mesa \
        libtool \
	lsb-release \
        make \
	software-properties-common && \
    rm -rf /var/lib/apt/lists/*

# Adding jderobot, gazebo, ice repositories
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list' && \
	apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 && \
	sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list' && \
	apt-key adv --keyserver keyserver.ubuntu.com --recv-key 67170598AF249743 && \
	apt-key adv --keyserver keyserver.ubuntu.com --recv B6391CB2CFBA643D && \
	apt-add-repository "deb http://zeroc.com/download/Ice/3.7/ubuntu18.04 stable main" && \
	sh -c 'echo "deb [arch=amd64] http://wiki.jderobot.org/apt `lsb_release -cs` main" > /etc/apt/sources.list.d/jderobot.list' && \
    	apt-key adv --keyserver keyserver.ubuntu.com --recv 24E521A4

# Installing jderobot, jderobot-assets
RUN apt update && apt install -y --no-install-recommends \
        jderobot \
        jderobot-assets && \
    echo "source /opt/jderobot/share/jderobot/gazebo/gazebo-setup.sh" >> ~/.bashrc && \
    echo "source /opt/jderobot/share/jderobot/gazebo/assets-setup.sh" >> ~/.bashrc && \
    echo "source /opt/jderobot/setup.bash" >> ~/.bashrc && \
    rm -rf /var/lib/apt/lists/*

# Installing GTK2 libraries.
RUN apt update && apt install -y --no-install-recommends \
	libgtk2.0-0 libgtk2.0-bin libgtk2.0-cil libgtk2.0-common libgtk2.0-dev libgtkgl2.0-1 \
	libgtkgl2.0-dev libgtkglext1 libgtkglext1-dev libglademm-2.4-dev libgtkmm-2.4-dev \
	libgnomecanvas2-0 libgnomecanvas2-dev  libgtkglext1-doc libgnomecanvasmm-2.6-dev \
	libgnomecanvasmm-2.6-1v5 libgtkglextmm-x11-1.2-0v5 libgtkglextmm-x11-1.2-dev \
	&& rm -rf /var/lib/apt/lists/*

# Installing GTK3, GSL, LibXML, Eisen, Fireware, USB, CWIID, boost, Python and ROS libraries.
RUN apt update && apt install -y --no-install-recommends \
	libgoocanvasmm-2.0-6 libgoocanvasmm-2.0-dev libgsl23 gsl-bin libgsl-dev \
	libxml++2.6-2v5 libxml++2.6-dev libtinyxml-dev libeigen3-dev \
	libdc1394-22 libdc1394-22-dev libusb-1.0-0 libusb-1.0-0-dev \
	libcwiid-dev python-matplotlib python-pyqt5 python-pip python-numpy python-pyqt5.qtsvg \
	qtbase5-dev libqt5script5 libqt5svg5-dev libboost-system-dev libboost-filesystem-dev \
	ros-melodic-desktop-full ros-melodic-roscpp ros-melodic-std-msgs ros-melodic-cv-bridge ros-melodic-image-transport \
	ros-melodic-roscpp-core ros-melodic-rospy ros-melodic-nav-msgs ros-melodic-geometry-msgs \
	ros-melodic-mavros ros-melodic-gazebo-plugins ros-melodic-kobuki-msgs \
	&& rm -rf /var/lib/apt/lists/*

# Installing Qfi libraries, Qt 5.
RUN apt update && apt install -y --no-install-recommends \
	devscripts ubuntu-dev-tools debhelper dh-make diffutils patch gnupg fakeroot \
	lintian pbuilder cdbs qt5-qmake qt5-default \
	&& rm -rf /var/lib/apt/lists/*

# Building Qflight instruments from Jderobot ThirdParty repository.
RUN git clone https://github.com/JdeRobot/ThirdParty && \
    cd ThirdParty/qflightinstruments && \
    mkdir build && cd build && \
    qmake .. && make && make install

# Google glog, GStreamer, ICE, Configured ICE for python
RUN apt update && apt install -y --no-install-recommends \
	libgflags-dev liblmdb-dev \
	libgoogle-glog-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev \
	libdb5.3-dev libdb5.3++-dev libssl-dev libbz2-dev libmcpp-dev && \
	git clone -b 3.7 https://github.com/zeroc-ice/ice.git && \
	cd ice/cpp && make -j 4 CPP11=yes OPTIMIZE=yes && make install && \
	pip2 install --upgrade pip && \
	pip2 install zeroc-ice==3.7.2 \
        && rm -rf /var/lib/apt/lists/*

# OpenNI2, Point Cloud Library, OpenCV, NodeJS, IceStorm.
RUN apt update && apt install -y --no-install-recommends \
	libopenni2-dev libopenni-dev libpcl-dev libopencv-dev nodejs \
	libzeroc-ice3.7 libzeroc-icestorm3.7 zeroc-ice-slice libzeroc-ice-dev zeroc-icebox zeroc-ice-utils \
	&& rm -rf /var/lib/apt/lists/*

# Building SDK Parrot for ArDrone
RUN cd /ThirdParty/ && \
    mkdir ardronelib-build && cd ardronelib-build && \
    wget https://raw.githubusercontent.com/RoboticsURJC/JdeRobot-ThirdParty/master/ardronelib/CMakeLists.txt && \
    wget https://raw.githubusercontent.com/RoboticsURJC/JdeRobot-ThirdParty/master/ardronelib/ffmpeg-0.8.pc.in && \
    wget https://raw.githubusercontent.com/RoboticsURJC/JdeRobot-ThirdParty/master/ardronelib/ardronelib.pc.in && \
    cmake . && make -j 4 &&  make install

# Building Jderobot
RUN /bin/bash -c "source /opt/ros/melodic/setup.bash; \
    git clone https://github.com/JdeRobot/base; \
    cd base; \
    mkdir build && cd build; \ 
    cmake .. && make -j 4 && make install;"
