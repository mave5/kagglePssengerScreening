FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04

ARG userid=1000
ARG username=mra

# Install Ubuntu libraries
RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends build-essential cmake libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev unzip libgtkglext1 libgtkglext1-dev ffmpeg wget ca-certificates wkhtmltopdf sudo && \
    apt-get autoremove -y && \
    rm -rf /var/cache && \
    rm -rf /var/lib/apt/lists/*

# Setup user and permissions
WORKDIR /home/$username
RUN useradd -r -u $userid $username; chown $username:$username /home/$username

ENV BUILDDIR /home/$username

# Install Anaconda
RUN cd $BUILDDIR && \
    wget --no-check-certificate https://repo.continuum.io/archive/Anaconda2-4.3.0-Linux-x86_64.sh && \
    bash Anaconda2-4.3.0-Linux-x86_64.sh -b -p "$BUILDDIR/anaconda2" && \
    $BUILDDIR/anaconda2/bin/conda install keras=1.1.1 -y && \
    $BUILDDIR/anaconda2/bin/conda install pyqt=4 -y && \
    $BUILDDIR/anaconda2/bin/conda install libgcc && \
    $BUILDDIR/anaconda2/bin/conda install ipython -y && \
    $BUILDDIR//anaconda2/bin/pip install pdfkit && \
    $BUILDDIR/anaconda2/bin/conda clean --all -y && \
    cd $BUILDDIR && \
    rm $BUILDDIR/Anaconda2-4.3.0-Linux-x86_64.sh 

# Install SharedArray
RUN cd $BUILDDIR && \
    wget --no-check-certificate https://pypi.python.org/packages/7b/64/24f5876511af54eadea7622ea60f63cf05ccc61caa22e1f2b612a09c27f3/SharedArray-2.0.2.tar.gz && \
    tar -xvzf SharedArray-2.0.2.tar.gz && \
    cd SharedArray-2.0.2 && \
    python setup.py build && \
    python setup.py install && \
    ln -s /usr/local/lib/python2.7/dist-packages/SharedArray.so $BUILDDIR/anaconda2/lib/python2.7/site-packages/SharedArray.so && \
    cd $BUILDDIR && \
    rm $BUILDDIR/SharedArray-2.0.2.tar.gz && \
    rm -rf $BUILDDIR/SharedArray-2.0.2

# Install opencv
RUN cd $BUILDDIR && \
    wget --no-check-certificate https://github.com/opencv/opencv/archive/2.4.13.zip && \
    unzip 2.4.13.zip && \
    cd opencv-2.4.13 && \
    mkdir release && \
    cd release && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_CUDA=ON -D ENABLE_FAST_MATH=1 -D CUDA_FAST_MATH=1 -D WITH_CUBLAS=1 -D WITH_OPENGL=ON -D INSTALL_PYTHON_EXAMPLES=ON -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-2.4.13/modules -D BUILD_EXAMPLES=OFF .. && \
    make -j8 && \
    make install && \
    ldconfig && \
    ln -s /usr/local/lib/python2.7/dist-packages/cv2.so $BUILDDIR/anaconda2/lib/python2.7/site-packages/cv2.so && \
    cd $BUILDDIR && \
    rm $BUILDDIR/2.4.13.zip && \
    rm -rf $BUILDDIR/opencv-2.4.13

# install theano 0.9.0
RUN $BUILDDIR/anaconda2/bin/conda install theano=0.9.0 -y

# install tabulate
RUN $BUILDDIR/anaconda2/bin/conda install tabulate

# Start shell with the user
USER $username

# Post-installation configuration
RUN touch $BUILDDIR/.bashrc && \
    echo "# added by Anaconda2 4.3.0 installer" >>$BUILDDIR/.bashrc && \
    echo -n 'export PATH=' >>$BUILDDIR/.bashrc; echo -n $BUILDDIR >>$BUILDDIR/.bashrc; echo '/anaconda2/bin:$PATH' >>$BUILDDIR/.bashrc>>$BUILDDIR/.bashrc && \
    mkdir $BUILDDIR/.keras && \
    touch $BUILDDIR/.keras/keras.json && \
    echo '{\n    "image_dim_ordering": "th",\n    "epsilon": 1e-07,\n    "floatx": "float32",\n    "backend": "theano"\n}' > $BUILDDIR/.keras/keras.json && \
    echo '[global]\nfloatX = float32\ndevice = gpu\n[nvcc]\nfastmath = True\n[lib]\ncnmem = 0.2' >$BUILDDIR/.theanorc && \
    echo "$BUILDDIR/anaconda2/bin/ipython notebook --ip=0.0.0.0 --no-browser" >$BUILDDIR/runIpython.sh

