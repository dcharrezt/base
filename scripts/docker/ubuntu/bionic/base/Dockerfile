from osrf/ros:melodic-desktop-full-bionic

# Jderobot base and assets
RUN sh -c 'echo "deb [arch=amd64] http://wiki.jderobot.org/apt `lsb_release -cs` main" > /etc/apt/sources.list.d/jderobot.list' && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv 24E521A4 && \
    apt update && apt install -y --no-install-recommends \
        jderobot \
        jderobot-assets \
        ros-melodic-jderobot-assets && \
    echo "source /opt/jderobot/share/jderobot/gazebo/gazebo-setup.sh" >> ~/.bashrc && \
    echo "source /opt/jderobot/share/jderobot/gazebo/assets-setup.sh" >> ~/.bashrc && \
    echo "source /opt/jderobot/setup.bash" >> ~/.bashrc
