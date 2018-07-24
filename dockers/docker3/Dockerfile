ARG cuda_version=9.0
ARG cudnn_version=7

FROM nvidia/cuda:${cuda_version}-cudnn${cudnn_version}-devel

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

ARG anaconda_major=3
ARG anaconda_mainor=5.0.1

# Install Anaconda
RUN cd $BUILDDIR && \
    wget --no-check-certificate https://repo.continuum.io/archive/Anaconda${anaconda_major}-${anaconda_mainor}-Linux-x86_64.sh && \
    bash Anaconda${anaconda_major}-${anaconda_mainor}-Linux-x86_64.sh -b -p "$BUILDDIR/anaconda${anaconda_major}" && \
    #$BUILDDIR/anaconda${anaconda_major}/bin/conda install keras -y && \
    $BUILDDIR/anaconda${anaconda_major}/bin/conda install pyqt -y && \
    $BUILDDIR/anaconda${anaconda_major}/bin/conda install libgcc && \
    $BUILDDIR/anaconda${anaconda_major}/bin/conda install ipython -y && \
    $BUILDDIR//anaconda${anaconda_major}/bin/pip install pdfkit && \
    $BUILDDIR/anaconda${anaconda_major}/bin/conda clean --all -y && \
    cd $BUILDDIR && \
    rm $BUILDDIR/Anaconda${anaconda_major}-${anaconda_mainor}-Linux-x86_64.sh


# install theano 0.9.0
RUN $BUILDDIR/anaconda${anaconda_major}/bin/conda install theano=0.9.0 -y

# install tabulate
#RUN $BUILDDIR/anaconda${anaconda_major}/bin/conda install tabulate

# install tqdm
RUN $BUILDDIR/anaconda${anaconda_major}/bin/conda install tqdm

# install theano 1.x 
#RUN $BUILDDIR/anaconda${anaconda_major}/bin/conda install -c conda-forge theano 

# install numpy
#RUN $BUILDDIR/anaconda${anaconda_major}/bin/conda install -c anaconda numpy 

# install opencv
#RUN $BUILDDIR/anaconda${anaconda_major}/bin/conda install -c conda-forge opencv



# Start shell with the user
USER $username

# Post-installation configuration
RUN touch $BUILDDIR/.bashrc && \
     echo "# added by Anaconda installer" >>$BUILDDIR/.bashrc && \
     echo -n 'export PATH=' >>$BUILDDIR/.bashrc; echo -n $BUILDDIR >>$BUILDDIR/.bashrc; echo '/anaconda3/bin:$PATH' >>$BUILDDIR/.bashrc>>$BUILDDIR/.bashrc 
#    mkdir $BUILDDIR/.keras && \
#    touch $BUILDDIR/.keras/keras.json && \
#    echo '{\n    "image_dim_ordering": "th",\n    "epsilon": 1e-07,\n    "floatx": "float32",\n    "backend": "theano"\n}' > $BUILDDIR/.keras/keras.json && \
#    echo '[global]\nfloatX = float32\ndevice = gpu\n[nvcc]\nfastmath = True\n[lib]\ncnmem = 0.2' >$BUILDDIR/.theanorc && \
RUN  echo "$BUILDDIR/anaconda${anaconda_major}/bin/ipython notebook --ip=0.0.0.0 --no-browser" >$BUILDDIR/runIpython.sh



