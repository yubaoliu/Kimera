FROM yubao/ubuntu18:cuda11.3.0
MAINTAINER yubao.liu.ra@tut.jp

# Install OpenCV for Ubuntu 18.04
RUN sudo apt-get update \
    && sudo apt-get install -y xvfb \
    unzip \
    libjpeg-dev libpng-dev libtiff-dev \
    libvtk6-dev \
    libgtk-3-dev \
    libatlas-base-dev gfortran

# RUN git clone https://github.com/opencv/opencv.git
# RUN git clone https://gitee.com/yubaoliu/opencv.git      
# RUN cd opencv && \
      # git checkout tags/3.3.1 && \
      # mkdir build

# RUN git clone https://github.com/opencv/opencv_contrib.git
# RUN git clone https://gitee.com/yubaoliu/opencv_contrib.git
# RUN cd opencv_contrib && \
#       git checkout tags/3.3.1

WORKDIR /home/ubuntu

COPY --chown=ubuntu:ubuntu Extern/opencv Extern/opencv
COPY --chown=ubuntu:ubuntu Extern/opencv_contrib Extern/opencv_contrib

RUN cd Extern/opencv \
    && sed -i '6i  INCLUDE_DIRECTORIES("/home/ubuntu/Extern/opencv_contrib/modules/xfeatures2d/include")' $HOME/Extern/opencv/modules/stitching/CMakeLists.txt \
    && mkdir build && cd build  \
    && cmake -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    -D BUILD_opencv_python=OFF \
    -D BUILD_opencv_python2=OFF \
    -D BUILD_opencv_python3=OFF \
    -D WITH_CUDA=OFF \
    -DOPENCV_EXTRA_MODULES_PATH=$HOME/Extern/opencv_contrib/modules .. \
    && make -j5 \
    && sudo make install

# Install GTSAM
RUN sudo apt-get update && sudo apt-get install -y libboost-all-dev libtbb-dev

COPY --chown=ubuntu:ubuntu Extern/gtsam Extern/gtsam

# RUN git clone https://github.com/borglab/gtsam.git
RUN cd Extern/gtsam \
    && mkdir build \
    && cd build \
    && cmake -DCMAKE_INSTALL_PREFIX=/usr/local \
    -DGTSAM_BUILD_TESTS=OFF \
    -DGTSAM_BUILD_EXAMPLES_ALWAYS=OFF \
    -DCMAKE_BUILD_TYPE=Release \
    -DGTSAM_BUILD_UNSTABLE=ON \
    -DGTSAM_POSE3_EXPMAP=ON \
    -DGTSAM_ROT3_EXPMAP=ON \
    -DGTSAM_TANGENT_PREINTEGRATION=OFF ..  \
    && sudo make -j3 install

COPY --chown=ubuntu:ubuntu Extern/opengv Extern/opengv

# Install Open_GV
# RUN git clone https://github.com/laurentkneip/opengv
RUN cd Extern/opengv  \
    && mkdir build \
    && cd build  \
    && cmake -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=/usr/local \
      -DEIGEN_INCLUDE_DIRS=$HOME/Extern/gtsam/gtsam/3rdparty/Eigen \
      -DEIGEN_INCLUDE_DIR=$HOME/Extern/gtsam/gtsam/3rdparty/Eigen .. \
    && sudo make -j3 install

COPY --chown=ubuntu:ubuntu Extern/DBoW2 Extern/DBoW2

# Install DBoW2
# RUN git clone https://github.com/dorian3d/DBoW2.git
RUN cd Extern/DBoW2  \
    && mkdir build \
    && cd build \
    && cmake .. \
    && sudo make -j3 install

# ADD https://api.github.com/repos/borglab/gtsam/git/refs/heads/master version.json

# Install RobustPGO
# ADD https://api.github.com/repos/MIT-SPARK/Kimera-RPGO/git/refs/heads/master version.json
# RUN git clone https://github.com/MIT-SPARK/Kimera-RPGO.git

COPY --chown=ubuntu:ubuntu Extern/Kimera-RPGO Extern/Kimera-RPGO
RUN cd Extern/Kimera-RPGO  \
# RUN cd Kimera-RPGO  \
    && mkdir build  \
    && cd build  \
    && cmake .. \
    && make -j \
    && sudo make install

# Install glog, gflags
RUN sudo apt-get update \
    && sudo apt-get install -y libgflags2.2 libgflags-dev libgoogle-glog0v5 libgoogle-glog-dev

# Install Pangolin
RUN sudo apt-get update \
    && sudo apt-get install -y libgl1-mesa-dev libglew-dev

# RUN git clone https://github.com/stevenlovegrove/Pangolin.git
COPY --chown=ubuntu:ubuntu Extern/Pangolin Extern/Pangolin

RUN cd Extern/Pangolin \
    && mkdir build \
    && cd build \
    && cmake .. \
    && make -j3 \
    && sudo make install

RUN sudo ldconfig

RUN sudo apt-get install -y --no-install-recommends apt-utils \
    ros-melodic-image-geometry \
    ros-melodic-pcl-ros \
    ros-melodic-cv-bridge \
    libboost-all-dev \
    libjpeg-dev libpng-dev libtiff-dev \
    libvtk6-dev libgtk-3-dev \
    libatlas-base-dev gfortran \
    libparmetis-dev \
    python-wstool python-catkin-tools \
    libtbb-dev

COPY --chown=ubuntu:ubuntu Extern/Kimera-VIO-ROS /home/ubuntu/catkin_ws/src/Kimera-VIO-ROS

# RUN cd ~/catkin_ws \
#     && catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release -DGTSAM_TANGENT_PREINTEGRATION=OFF \
#     && catkin config --merge-devel \
#     && cd ~/catkin_ws/src
    # && wstool init \
    # && wstool merge Kimera-VIO-ROS/install/kimera_vio_ros_https.rosinstall \
    # && wstool update


# setup entrypoint
COPY --chown=ubuntu:ubuntu Extern/entrypoint.sh /home/ubuntu/entrypoint.sh

RUN sudo chmod 755 $HOME/entrypoint.sh
ENTRYPOINT ["/home/ubuntu/entrypoint.sh"]


# # Get python3
# # RUN sudo apt-get update && \
# #     sudo add-apt-repository ppa:deadsnakes/ppa
#  
# RUN sudo apt-get update && \
#     sudo apt-get install -y python3.6 python3.6-dev python-pip python3-pip python-tk python3-tk
# RUN python3.6 -m pip install PyQt5==5.14
# 
# # Install evo-1 for evaluation
# # Hack to avoid Docker's cache when evo-1 master branch is updated.
# ADD https://api.github.com/repos/ToniRV/evo-1/git/refs/heads/master version.json
# RUN git clone https://github.com/ToniRV/evo-1.git
# RUN cd evo-1 && python3.6 $(which pip3) install .
# 
# # Install Kimera VIO Evaluation
# RUN python3.6 $(which pip3) install ipython prompt_toolkit
# # Hack to avoid Docker's cache when Kimera VIO Evaluation master branch is updated.
# ADD https://api.github.com/repos/MIT-SPARK/Kimera-VIO-Evaluation/git/refs/heads/master version.json
# RUN git clone https://github.com/MIT-SPARK/Kimera-VIO-Evaluation.git
# # We use `pip3 install -e .` so that Jinja2 has access to the webiste template...
# RUN cd Kimera-VIO-Evaluation && git fetch && git checkout master && python3.6 $(which pip3) install -e .
# 

