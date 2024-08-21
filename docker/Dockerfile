FROM bharadwajakshay/nvidiaros:noetic

# Install PCL & Eigen & essential libraries
RUN apt-get update && apt-get install -y cmake libeigen3-dev libpcl-dev \
    ros-noetic-rviz ros-noetic-pcl-ros ros-noetic-eigen-conversions \
    libatlas-base-dev libgoogle-glog-dev libsuitesparse-dev libglew-dev wget \
    pcl-tools


# Install matplotlib
RUN apt-get update && apt-get install -y python3-pip python3-tk && pip3 install matplotlib

# Install ceres-solver
WORKDIR /home/
RUN wget https://github.com/ceres-solver/ceres-solver/archive/refs/tags/2.0.0.tar.gz
RUN tar zxf 2.0.0.tar.gz
RUN cd ceres-solver-2.0.0
RUN mkdir build && cd build
RUN ls
RUN cmake -DCMAKE_BUILD_TYPE=Release ./ceres-solver-2.0.0 && make -j2 && make install

# Install colmap
RUN apt-get install -y git cmake ninja-build build-essential libboost-program-options-dev \
libboost-filesystem-dev libboost-graph-dev libboost-system-dev libeigen3-dev libflann-dev \
libfreeimage-dev libmetis-dev libgoogle-glog-dev libgtest-dev libsqlite3-dev libglew-dev \
qtbase5-dev libqt5opengl5-dev libcgal-dev libceres-dev

RUN git clone https://github.com/colmap/colmap.git && cd colmap && mkdir build && cd build \
&& cmake -DCMAKE_CUDA_ARCHITECTURES="native" .. -GNinja && ninja && ninja install



WORKDIR /home/akshay/catkin_ws/src
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash; catkin_init_workspace; cd .. && catkin_make'
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash; git clone git@github.com:bharadwajakshay/lidar-cam-calib.git; cd .. && catkin_make'

WORKDIR /home/akshay/catkin_ws/

# Load ROS environment at each run
COPY ./ros_entrypoint.sh /
RUN chmod 755 /ros_entrypoint.sh
ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
