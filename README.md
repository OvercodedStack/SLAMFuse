# SLAMFuse (AKA: SLAMBench 3.0):

This is a fork of the original SLAMFuse SLAM benchmark tool. This fork is designed to be a specialized tool that that can collect additional information that was not included in the original release. These enhancements are subtle but were collected to provide an additional level of validting the data collected by SLAMFuse. If you have complex technical issues, such as how does SLAMFuse create datasets, refer to the wiki: https://github.com/OvercodedStack/SLAMFuse/wiki

### Disclaimer 2.0: 

This version relies a lot more on the SLAMBench 3.0 capabilities of SLAMFuse rather than the Docker version of the algorithms. The reasoning for this choice is because some alterations were done to the dockerized versions of the SLAM algorithms provided by the original SLAMFuse developers, specifically the wrappers used to interface with SLAMFuse.

### Is this project on hold?

No, I will update this repository with the SLAMFuse-based code, which then will include the changes to the other SLAM algorithms as well.

### How to cite:

Until I publish my own publications on this tool, I will hold off on citing my own work on this tool. However, you can cite the original developers of SLAMBench and SLAMFuse:

```
@INPROCEEDINGS{Radulov2024,
  author={Radulov, Nikola and Zhang, Yuhao and Bujanca, Mihai and Ye, Ruiqi and Luján, Mikel},
  booktitle={2024 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)}, 
  title={A Framework for Reproducible Benchmarking and Performance Diagnosis of SLAM Systems}, 
  year={2024},
  volume={},
  number={},
  pages={14225-14232},
  keywords={Software maintenance;Simultaneous localization and mapping;Perturbation methods;Software algorithms;Benchmark testing;Fuzzing;Reproducibility of results;Reliability;Stress;Resilience},
  doi={10.1109/IROS58592.2024.10801690}}

@inproceedings{bujanca2019slambench,
  title={SLAMBench 3.0: Systematic automated reproducible evaluation of SLAM systems for robot vision challenges and scene understanding},
  author={Bujanca, Mihai and Gafton, Paul and Saeedi, Sajad and Nisbet, Andy and Bodin, Bruno and O'Boyle, Michael FP and Davison, Andrew J and Kelly, Paul HJ and Riley, Graham and Lennox, Barry and others},
  booktitle={2019 International Conference on Robotics and Automation (ICRA)},
  pages={6351--6358},
  year={2019},
  organization={IEEE}
}
```




# Quickstart Instructions

SLAMBench 3.0 relies on essentially 3 broad instructions. The steps are as follow:

- Clone the SLAMFuse/SLAMBench codebase.
- Install/compile the basic SLAMBench
- Install additional SLAM algorithms and/or datasets

These steps are detailed below:

## Clone this SLAMFuse repository

```
git clone https://github.com/OvercodedStack/SLAMFuse
```

## Install OS-based dependencies (Copied over from SLAMBench 3.0):

#### To install them ####

With Fedora 24: 
```dnf install -y gtk2-devel vtk-devel cmake make git mercurial wget unzip gcc gcc-c++ lapack blas lapack-devel blas-devel findutils  cvs  glut-devel glew-devel boost-devel glog-devel gflags-devel libXmu-devel```

With Ubuntu 16.10: 
``` apt-get -y install libvtk6.3 libvtk6-dev unzip libflann-dev wget mercurial git gcc cmake python-numpy freeglut3 freeglut3-dev libglew1.5 libglew1.5-dev libglu1-mesa libglu1-mesa-dev libgl1-mesa-glx libgl1-mesa-dev libxmu-dev libxi-dev  libboost-all-dev cvs libgoogle-glog-dev libatlas-base-dev gfortran  gtk2.0 libgtk2.0-dev  libyaml-dev build-essential bison flex libyaml-cpp-dev ```

With Ubuntu 16.04: 
``` apt-get -y install libvtk6.2 libvtk6-dev unzip libflann-dev wget mercurial git gcc cmake python-numpy freeglut3 freeglut3-dev libglew1.5 libglew1.5-dev libglu1-mesa libglu1-mesa-dev libgl1-mesa-glx libgl1-mesa-dev libxmu-dev libxi-dev  libboost-all-dev cvs libgoogle-glog-dev libatlas-base-dev gfortran  gtk2.0 libgtk2.0-dev libproj9 libproj-dev libyaml-0-2 libyaml-dev libyaml-cpp-dev libhdf5-dev libhdf5-dev```

With Ubuntu 14.04: 
``` apt-get -y install libvtk6-dev unzip libflann-dev wget mercurial git gcc cmake python-numpy freeglut3 freeglut3-dev libglew1.5 libglew1.5-dev libglu1-mesa libglu1-mesa-dev libgl1-mesa-glx libgl1-mesa-dev libxmu-dev libxi-dev  libboost-all-dev cvs libgoogle-glog-dev libatlas-base-dev gfortran  gtk2.0 libgtk2.0-dev ```

#### Special requirements for CUDA ####

Tu run the CUDA implementation of some of the algorithms, you will need extra dependencies.

With Ubuntu: 
``` apt-get -y install  nvidia-cuda-toolkit clinfo``` 

With Fedora: 
``` yum install cuda ``` 

Note: This is not guaranteed to work, occassionally this will not detect the graphics adapter. 

## Run the basic installation steps of SLAMBench:


Change into the directory where you have SLAMBench and run the basic installation steps from SLAMBench 3.0.
```
cd <your directory>/slamfuse
make deps
make slambench
```

## Install supported SLAMBench algorithms

This repository carries over the SLAM algorithms it previously supported. To check the full list, please look into the file called ``` benchmarks/benchmarks.repo ``` located in this repository.

The instructions remain the same for installing the SLAM algorithms; however:

```
make slambench APPS=orbslam2
make slambench APPS=lsdslam
```
NOTE: You HAVE to install each SLAM algorithm one line at a time. Doing ``` make slambench APPS= orbslam2,lsdslam``` could work but if at any time there was a compilation error or it fails to find the algorithm, SLAMBench will report successful compilation and a small error warning will appear regarding if the SLAM algorithm was installed or not. 

If you have reached this point and have resolved all dependency or compilation errors regarding each SLAM algorithm, congratulations! You are now ready to benchmark.

# How to benchmark?

SLAMBench 3.0 provides a significant instruction manual regarding the operation of SLAMBench 3.0. 


Once you have compile a benchmark, there are several ways to run it.
For each implementation of this benchmark, you will find a specific library. 
As an example, with KinectFusion, after running ```make benchmark APPS=kfusion```, you may found the following libraries in the ```build/lib``` directory :

```
> ls build/lib/libkfusion-*-library.so

build/lib/libkfusion-cpp-library.so   
build/lib/libkfusion-notoon-library.so      
build/lib/libkfusion-openmp-library.so
build/lib/libkfusion-cuda-library.so  
build/lib/libkfusion-opencl-library.so

```

We can see five different implementations (cpp, notoon, and openmp, cuda and opencl). The list of available binaries depends of the dependencies you installed beforehand. For example, you need CUDA to compile the kfusion-cuda. 


### Running a benchmark (e.g. KinectFusion) ###

To run one algorithm you will need to use a **loader**. 
There is currently two different loaders supported, **benchmark** and **pangolin**.
Both loader are used the same way, except that **benchmark** is a command line application dedicated to measurements, while **pangolin** is a graphical user interface less precise in term of measurement but which provide a good interface for demonstrations.


Each loader has a series of parameters to specify such as the dataset location, or the libraries to run. 
The list of those parameters is available by using the "--help" parameters.

```
> ./build/bin/benchmark_loader --help 
 == SLAMBench Configuration ==
  Available parameters :
   -fl            --frame-limit           : last frame to compute (Default=0)
   -o             --log-file              : Output log file (Default=)
   -i             --input                 : Specify the input file or mode. (Default=)
   -load          --load-library          : Load a specific SLAM library. (Default=)
   -dse           --dse                   : Output solution space of parameters. (Default=false)
   -h             --help                  : Print the help. (Default=false)
   -nf            --negative-focal-length : negative focal length (Default=false)
   -realtime      --realtime-mode         : realtime frame loading mode (Default=false)
   -realtime-mult --realtime-multiplier   : realtime frame loading mode (Default=1)
   -fo            --file-output           : File to write slamfile containing outputs (Default=)
    
```


Then if you run the loader again, while providing a dataset file ```-i dataset.slam```, you will see new parameters dedicated to the dataset : 


```
> ./build/bin/benchmark_loader -i datasets/ICL_NUIM/living_room_traj2_loop.slam --help
 == SLAMBench Configuration ==
  Available parameters :
   ....
   -Camera-intrisics --Camera-intrisics       : (Default=nullptr  Current=0.751875,1,0.4992185,0.4989583)
   -Depth-intrisics  --Depth-intrisics        : (Default=nullptr  Current=0.751875,1,0.4992185,0.4989583)
   -Depth-dip        --Depth-disparity-params : (Default=nullptr  Current=0.001,0)
   -Camera-intrisics --Camera-intrisics       : (Default=nullptr  Current=0.751875,1,0.4992185,0.4989583)


```

Finally is you add a library name ```-load libname```, more parameter can be seen : 

```
> ./build/bin/benchmark_loader -i datasets/ICL_NUIM/living_room_traj2_loop.slam -load ./build/lib/libkfusion-cpp-library.so  --help
 == SLAMBench Configuration ==
  Available parameters :

    ....

   -c                --compute-size-ratio     : Compute ratio (Default=1)
   -r                --integration-rate       : integration-rate  (Default=2)
   -t                --tracking-rate          : tracking-rate     (Default=1)
   -z                --rendering-rate         : rendering-rate    (Default=4)
   -l                --icp-threshold          : icp-threshold     (Default=1e-05)
   -m                --mu                     : mu                (Default=0.1)
   -s                --volume-size            : volume-size       (Default=8,8,8)
   -d                --volume-direction       : volume-direction  (Default=4,4,4)
   -v                --volume-resolution      : volume-resolution (Default=256,256,256)
   -y1               --pyramid-level1         : pyramid-level1    (Default=10)
   -y2               --pyramid-level2         : pyramid-level2    (Default=5)
   -y3               --pyramid-level3         : pyramid-level3    (Default=4)

```


You can run a loader with **only one dataset** at a time and **it must be specified first**.


### Evaluating a benchmark (eg. KinectFusion) ###

SLAMBench works with Metrics and Outputs elements. 
When you run the ```benchmark_loader``` or the ```pangolin_loader``` these are those elements that you can visualize.
Metrics are components generated by SLAMbench framework really, while Outputs are generated by the algorithm or may be elements post-processed by SLAMbench (such as the aligned trajectory with the ground truth).

Les us run the benchmark loader. Its output is composed of two main parts, the ```Properties``` section, and the ```Statistics``` section. 
the properties section details all the parameters used for the experiment (could been changed or not via the command line). 
the statistics section repart all the outputs and metrics selection for output in the benchmark loader.

```
> ./build/bin/benchmark_loader -i datasets/ICL_NUIM/living_room_traj2_loop.slam -load ./build/lib/libkfusion-cpp-library.so 

SLAMBench Report run started:	2018-02-02 04:41:31

Properties:
=================

frame-limit: 0
log-file: 
input: datasets/ICL_NUIM/living_room_traj2_loop.slam
load-library: ./build/lib/libkfusion-cpp-library.so
dse: false
help: false
negative-focal-length: false
realtime-mode: false
realtime-multiplier: 1
file-output: 
Camera-intrisics: 0.751875,1,0.4992185,0.4989583
Depth-intrisics: 0.751875,1,0.4992185,0.4989583
Depth-disparity-params: 0.001,0
Camera-intrisics: 0.751875,1,0.4992185,0.4989583
compute-size-ratio: 1
integration-rate: 2
tracking-rate: 1
rendering-rate: 4
icp-threshold: 1e-05
mu: 0.1
volume-size: 8,8,8
volume-direction: 4,4,4
volume-resolution: 256,256,256
pyramid-level1: 10
pyramid-level2: 5
pyramid-level3: 4
Statistics:
=================

Frame Number	Timestamp	Duration_Frame	GPU_Memory	CPU_Memory		Duration_Preprocessing	Duration_Tracking	Duration_Integration	Duration_Raycasting	Duration_Render	X	Y	ZATE_Frame
1	0.0000000000	0.7679200000	0	623801799		0.1254800000	0.0195420000	0.0561620000	0.0000030000	0.5667170000	4.0000000000	4.0000000000	4.0000000000	0.0000002980
2	1.0000000000	0.2003970000	0	623801799		0.1242030000	0.0156470000	0.0581670000	0.0000000000	0.0023710000	4.0000000000	4.0000000000	4.0000000000	0.0010031639
3	2.0000000000	0.1989980000	0	623801799		0.1233680000	0.0152360000	0.0580180000	0.0000000000	0.0023690000	4.0000000000	4.0000000000	4.0000000000	0.0055015362
4	3.0000000000	0.7518580000	0	623801799		0.1220660000	0.0152080000	0.0563070000	0.5559520000	0.0023170000	4.0000000000	4.0000000000	4.0000000000	0.0036504765
5	4.0000000000	1.3683420000	0	623801799		0.1240890000	0.0767240000	0.0581630000	0.5504240000	0.5589330000	3.9957129955	4.0020360947	4.0009112358	0.0021276891
...
```


# Frequently asked questions and answers

## Which set of instructions shoud I use to run this? SLAMFuse or SLAMBench 3.0? 

Use SLAMBench 3.0 as a basis to run this repository.

## What OS or setup is reccomended to run this repository?

SLAMBench 3.0 reccomends using Ubuntu 20.04, of which this repository is based off for development. Using a different OS leads to issues with dependencies, which means you will have to solve those issues on your own if you run into them. 

## Do you reccomend running this on a virtual machine?

I run into a lot of complications when running virtual machines on my computers so I avoid using them. As such I generally reccomend a spare high-powered computer to run the repository natively on a metal installation. If you can manage running this repository on a virtual machine, feel free to post the instructions on the wiki.

## I don't know what X does? 

Half the time I don't know either. Feel free to open an issue and I will attempt to answer it through code snippets or through the original SLAMFuse developers. 




<!-- ================================================== Disclaimer ============================================================ -->
<!--
# Disclaimer
SLAMFuse is still under development and not all features presented in the paper are currently available on  main 'SLAMFuse' branch. We are actively working on improving 
the tool so have a look [here](https://github.com/nikolaradulov/SLAMFuse/issues/30) if you can't find a feature you were looking for.   -->
<!-- ==================================================Prerequisites============================================================ -->

<!--
# 1. ___Prerequisites___
## 1.1 Recommended System:
* Linux distribution.
* Windows WSL2

## 1.2 Docker
Install [Docker Engine](https://docs.docker.com/engine/install/). <br>
If you are using Windows10 WSL2, you can EITHER install [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/) OR run following command:
```
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
$ sudo service docker start
$ sudo chmod 777 /var/run/docker.sock
```
-->
<!-- ==============================================Start with Docker========================================================= -->

<!--
# 2. ___Start with Docker___
## 2.1 Build SLAMFuse
```
$ sudo apt-get install git
$ git clone https://github.com/nikolaradulov/SLAMFuse.git slamfuse
$ cd ~/slamfuse/
$ docker build . -t slamfuse/main
```
## 2.2 Build Algorithm
```
$ cd ~/slamfuse/
$ ./scripts/algorithm-vol.sh [algorithm name]
$ ./scripts/algorithm-vol.sh floam
```
Replace [algorithm name] by `kfusion`, `orbslam3`, `floam`, etc.
Now you have: 
* **Docker IMAGE:** [algorithm name]-img
* **Docker CONTAINER:** [algorithm name]
* **Docker VOLUME:** [algorithm name]-vol

The `[algorithm name]-vol` contains all the necessary components for  evaluating algorithm on SLAMFuse. <br>
**Hints**: If Docker images takes too much memory and you already have `[algorithm name]-vol`, you can use `docker rm [algorithm name]`, `docker rmi [algorithm name]-img` and `docker system prune` to free memory.

## 2.3 Build dataset
```
$ python3 starter.py dataset -t make -d ./datasets/KITTI/2011_09_30_drive_0027/2011_09_30_drive_0027_sync.slam -v KITTI07
```
check `~/slamfuse/datasets/command.txt` for more details.

## 2.4 Run Algorithm
```
$ python3 starter.py run -t gui -dv KITTI07 -d 2011_09_30_drive_0027_sync.slam -a floam/libfloam-original-library.so
```
**If you want to have more control of the filesystem**, then you will need `-t interactive-gui`, it allows you to modify the configuration file, visualize results, and so on.
```
$ python3 starter.py run -t interactive-gui -dv KITTI07 -d 2011_09_30_drive_0027_sync.slam -a floam/libfloam-original-library.so
```
 -->
