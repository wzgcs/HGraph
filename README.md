**News!!!**   
**[12/03/2023]** Manuscript `LR-CNN: Lightweight Row-Centric Convolutional Neural Network Training for Memory Reduction` is submitted to ICDE 2024 !!!  
**[08/10/2023]** Code cleanup.   
**[07/13/2023]** The existing ieda of Checkpointing is integrated.   
**[05/28/2023]** The existing idea of abandoning cheap-to-be-reomputed data is integrated.  
**[05/11/2017]** Another example RestNet-50 is tested.  
**[04/29/2023]** Two row partitioning policies are implemented.   
**[03/11/2023]** A prototype row-centric implementation on PyTorch using VGG-16 is tested.  
**[11/17/2022]** This project starts.  

# LR-CNN
LR-CNN is a new Convolutional Neural Network (CNN) implementation on PyTorch, with proimnent memory scalability via a novel Lightweight Row-centric computation design.

## 1. Introduction
The multi-layer CNN is one of the most successful deep learning algorithms. It has been widely used in various domains, especially for image understanding. To address complex datasets and tasks, it is becoming deeper and larger, with hundreds of layers and kernel parameters. Thus, training its network yields strong and ever-growing demands on memory resources. Till now, many works have been presented to break the memory wall, but all of them follow the conventional layer-by-layer computation dataflow. `LR-CNN` explores another path to achieve the memory reduction goal. Its novel row-centric dataflow is very orthogonal to existing works and is compatible with them. 

The LR-CNN project started at Ocean University of China in Nov. 2022. The backbone memobers are **Hangyu Yang** (master student) and **Zhigang Wang** (associate professor). Currently it is implemented on top of the widely used deep learning platform PyTorch. Features of LR-CNN are listed as below.   

* ___Weak Dependency:___ We deeply analyze convolution behaviours in CNN and find the weak dependency feature for intra- and inter-layer computations, instead of traditionally assumed the many-to-many strong dependency.  
* ___Row-centric Dataflow:___ Inspired by the weak dependency, we re-organize convolutions into rows through all convolutional layers in both forward- and backward-propagations. Then a lot of feature maps can be removed to reduce the peak memory usage.    
* ___Row Partitioning:___ We give two partitioning policies with different implementations to cope with the weak dependency between consecutive rows. They feature contributions respectively for low- and high-configured devices.
* ___Lightweight Design:___ Our proposals can work only on the originally provided accelerator, like GPU, without additonal hardwares, like CPU RAM and more physical devices. More imortantly, by integrating it into existing works, we can further enhance the benefit of memory reduction, to reduce the economic investments when processing larger CNNs. We have already made attempts for existing Prioritized Recomputation and Checkpointing techniques.        

## 2. Quick Start
LR-CNN is developed on top of PyTorch. Before running it, some softwares must be installed, which is beyond the scope of this document. 

### 2.1 Requirements
* PyTorch x.x.x  
* Python x.x.x or higher version   

### 2.2 Testing LR-CNN  
We should first prepare the input training samples.     

Second, we can submit a training job using the following commands.  
* __For VGG-16__  
`termite jar $HGraph_HOME/termite-examples-0.1.jar sssp.pull input output 2 50 100000 5 10000 2`  
About arguments:  
[1] input directory on HDFS  
[2] output directory on HDFS  
[3] the number of child processes (tasks)  
[4] the maximum number of supersteps  
[5] the total number of vertices  
[6] the number of VBlocks per task  
[7] the sending threshold  
[8] the source vertex id  
* __For ResNet-50:__  
`termite jar $HGraph_HOME/termite-examples-0.1.jar sssp.hybrid input output 2 50 100000 5 10000 10000 10000 2 2`  
About arguments:  
[1] input directory on HDFS  
[2] output directory on HDFS  
[3] the number of child processes (tasks)  
[4] the maximum number of supersteps  
[5] the total number of vertices  
[6] the number of VBlocks per task  
[7] the sending threshold used by b-pull  
[8] the sending threshold used by push  
[9] the receiving buffer size per task used by push  
[10] starting style: 1--push, 2--b-pull  
[11] the source vertex id    


## 3. Contact  
If you encounter any problem with HGraph, please feel free to contact yanghangyu3961@stu.ouc.edu.cn and wangzhigang@ouc.edu.cn.

