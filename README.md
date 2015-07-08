## Using Google's [deepdream](https://github.com/google/deepdream)

I used Ubuntu 15.04 to make this experiment.

I installed all the packages using the following instructions (found [here](https://www.reddit.com/r/deepdream/comments/3cd1yf/howto_install_on_ubuntulinux_mint_including_cuda/))

```bash
sudo apt-get install build-essential git
cd ~
git clone https://github.com/BVLC/caffe.git
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev python python-dev python-scipy python-setuptools python-numpy python-pip libgflags-dev libgoogle-glog-dev liblmdb-dev protobuf-compiler libatlas-dev libatlas-base-dev libatlas3-base libatlas-test
sudo apt-get install --no-install-recommends libboost-all-dev libhdf5-dev
sudo pip install --upgrade pip
sudo pip install --upgrade numpy
cd ~/caffe
cp Makefile.config.example Makefile.config
```

My PC doesn't have a GPU, so I edited the Makefile.config to use CPU only:

```bash
CPU_ONLY := 1
```

I also needed to add hdf5 path to Makefile.config:

```bash
INCLUDE_DIRS := $(PYTHON_INCLUDE) usr/local/include /usr/include/hdf5/serial/
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial

```


Added this to ~/.bashrc:

```bash
export PATH=$PATH:/usr/local/cuda-7.0/bin
export LD_LIBRARY_PATH=:/usr/local/cuda-7.0/lib64
export PYTHONPATH=/home/heitor/caffe/python
export PATH=/usr/local/cuda/bin:${PATH}
PATH=${CUDA_HOME}/bin:${PATH} 
export PATH
export PATH=$PATH:/usr/local/cuda/bin
export LD_LIBRARY_PATH=:/usr/local/cuda/lib64
```

Then this:

```bash
source ~/.bashrc
make all -j2
make test -j2
make runtest -j2
make pycaffe -j2
```

Now we're ready to use deepdream:

```bash
cd ~
git clone https://github.com/google/deepdream.git
wget ~/caffe/models/bvlc_googlenet http://dl.caffe.berkeleyvision.org/bvlc_googlenet.caffemodel
cd ~/deepdream
ipython notebook ./dream.ipynb
```
If anything goes wrong, check your python path:

```python
import sys
print sys.path
```

You can click on "Run all" button, after editing the file's name to be iterated.
The software uses 39 iteration per cycle. The last block of code makes infinite (I believe so) cycles, so you can leave it processing all you want. I had to halt it because it eats too much of CPU

Source image:
![source image](sky1024px.jpg)

Results:

![result 1](0000.jpg)
![result 2](0001.jpg)
![result 3](0002.jpg)
![result 4](0003.jpg)
![result 5](0004.jpg)
![result 6](0005.jpg)