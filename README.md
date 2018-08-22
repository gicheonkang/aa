Server Setup Guideline [2018-04-20]
===================================

-	based on sushi server
-	os : ubuntu 16.04.4 LTS
-	gpu : TitanX(Pascal)

#### will use

-	CUDA 8.0
-	CUDNN 6
-	python 2.7x
-	tensorflow ==1.4.0

#### Task

-	Densecap use torch with CUDNN5 (may be it will be replaced)

#### Common solutions for installation errors

-	use aptitude for apt-get install dependecy problem
	-	(sometimes you shoud answer 'n' for aptitude solutions for check alternatives)
-	if you change repository of apt-get or pip, there may be some problem in your mirror repo.
	-	so please back up sources.list if you change repo.
-	googling
-	call me

Graphic driver installation
---------------------------

```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install -y nvidia-390
```

**nvidia-390** for Titan Xp, may be need different version for others.

-	if failed, please follow link [here](http://pythonkim.tistory.com/48)
-	check if you plugged the monitor cable to the graphic card correctly (not M/B)

---

Optional tasks
--------------

### [1. Change Ubuntu Repository](http://webdir.tistory.com/201)

-	recommanded, for faster apt-get install

	#### - there is a problem when you installing ros if you use daumkakao repo ! back up the original sources.list and use original one when installing ros.

### [2. Korean keyboard](http://www.kwangsiklee.com/ko/2016/12/%EC%9A%B0%EB%B6%84%ED%88%AC%EC%97%90%EC%84%9C-uim-%ED%95%9C%EA%B8%80%EC%9E%85%EB%A0%A5%EA%B8%B0-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0/)

-	recommanded, can type Korean language

### 3. Tmux

```
 sudo apt-get install -y tmux
```

-	[if you want the latest version](https://gist.github.com/P7h/91e14096374075f5316e)
-	if you get error "libevent not found": `sudo apt-get install libevent-dev`

### 4. Chrome

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt-get install libxss1 libgconf2-4 libappindicator1 libindicator7
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

### [5. Teamviewer](https://www.teamviewer.com/ko/download/linux/?pid=google.tv.kr_en_sn_desk_brand_teamviewerlinux_ex.s.kr&gclid=EAIaIQobChMI9ZWul8-C2gIVzQoqCh31oQW_EAAYASAAEgJfEPD_BwE)

```
wget https://download.teamviewer.com/download/linux/teamviewer_amd64.deb
sudo apt install gdebi-core
sudo gdebi teamviewer_amd64.deb
```

---

Some Dependecies
----------------

-	maybe you don't have to install explicitly
-	it really 'depends'

```
sudo apt-get install libhdf5-dev libblas-dev liblapack-dev gfortran libboost-all-dev
```

---

CUDA & CUDNN
------------

### 1. CUDA 8.0

-	https://developer.nvidia.com/cuda-80-ga2-download-archive

```
 sudo dpkg -i cuda-repo-<distro>_<version>_<architecture>.deb
 sudo apt-get update
 sudo apt-get install cuda
```

-	add these lines to your .bashrc

```
export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

-	check installation

```
nvcc --version
```

#### if error occured while apt-get install(general solution)

```
sudo apt-get install aptitude
sudo aptitude install cuda          # maybe first solution is not the answer, so try other things too.
```

### 2. CUDNN 6

-	https://developer.nvidia.com/rdp/cudnn-download

```
tar xzvf cudnn-8.0-linux-x64-v6.0.tgz
sudo cp cuda/lib64/* /usr/local/cuda-8.0/lib64/
sudo cp cuda/include/* /usr/local/cuda-8.0/include/
sudo chmod a+r /usr/local/cuda-8.0/lib64/libcudnn*
sudo chmod a+r /usr/local/cuda-8.0/include/cudnn.h
```

-	check installation

```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2  
```

---

Torch
-----

```
git clone https://github.com/torch/distro.git ~/torch --recursive
cd ~/torch; bash install-deps;
./install.sh
source ~/.bashrc
luarocks install nn
luarocks install image
luarocks install lua-cjson
luarocks install https://raw.githubusercontent.com/qassemoquab/stnbhwd/master/stnbhwd-scm-1.rockspec
luarocks install https://raw.githubusercontent.com/jcjohnson/torch-rnn/master/torch-rnn-scm-1.rockspec
luarocks install cutorch
luarocks install cunn
luarocks install md5
luarocks install --server=http://luarocks.org/dev torch-ros
```
-	if luarocks doesn't work: git config --global url.https://github.com/.insteadOf git://github.com/
-	it takes much time to install all the libraries, i recommend to use an sh script

---

ROS
---

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
sudo apt-get update
sudo apt-get install ros-kinetic-desktop-full
sudo rosdep init
rosdep update
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

---

Python(v2.7.12)
---------------

-	install base python tools

```
sudo apt-get install python python-dev python-pip
```

-	[if you wanna chage repo to daumkakao(faster)](http://greenfishblog.tistory.com/255)
	-	but sometimes, no connection in mirror server,
-	install libraries
	-	\(**it is recommanded to install one by one** because, if there is an error on your line, you should reinstall every library in your commandl)

```
sudo pip install numpy scipy matplotlib jupyter notebook pandas sympy nose Cython h5py

 # might be error on pyyaml package if you use kakao mirror
 # then, use original repo for this package
sudo pip install keras  

## pytorch
sudo pip install http://download.pytorch.org/whl/cu80/torch-0.3.1-cp27-cp27mu-linux_x86_64.whl
sudo pip install torchvision


## nltk
sudo pip install nltk
python
```

and in your python

```python
import nltk
nltk.download('punkt')
```

```
sudo pip install pattern google-cloud

-	sudo pip install pyasn1 --upgrade
```

https://github.com/thtrieu/darkflow

```
git clone https://github.com/thtrieu/darkflow.git
cd darkflow
python setup.py build_ext --inplace
sudo pip install -e .
sudo pip install .

##tensorflow(gpu)
sudo pip install tensorflow-gpu==1.4.0

##deepspeech(gpu)
sudo pip install deepspeech-gpu

##for ros
sudo pip install joblib

sudo pip install --upgrade google-auth-oauthlib

##for dialogflow
sudo pip install dialogflow

##for colorama
sudo pip install launchpad
sudo pip install colorama
```

---

Python & ROS
------------

```
sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential 
sudo apt-get install ros-kinetic-navigation 
sudo apt-get install ros-kinetic-gmapping
 
sudo apt-get install ros-kinetic-pepper-*
```

---

Catkin
------

##### if you don't have catkin_ws folder

```
mkdir -p catkin_ws/src && cd catkin_ws catkin_make
```

##### if you have saved 'src' folder,

```
(copy src folder to ~/catkin/src) cd ~/catkin_ws && catkin_make
```

---

NAOqi
-----

suppose you downloaded naoqi folder

add below to your .bashrc

```
export PYTHONPATH=${PYTHONPATH}:~/{your naoqi directory}/lib/python2.7/site-packages

## example)
## if naoqi/ in /home/sushi
export PYTHONPATH=${PYTHONPATH}:~/home/sushi/naoqi/lib/python2.7/site-packages
```

---
(optional) PyCharm IDE
------

```
sudo add-apt-repository ppa:mystic-mirage/pycharm

sudo apt update

sudo apt install pycharm-community
```
