# Custom ava dataset, Custom Spatio Temporally Action Video Dataset
Custom ava dataset, Multi-Person Video Dataset Annotation Method of Spatio-Temporally Actions <br>
自定义ava数据集，多人视频的时空动作数据集标注方法 <br>

ava dataset paper: [https://arxiv.org/pdf/1705.08421.pdf](https://arxiv.org/pdf/1705.08421.pdf) <br>
ava数据集论文：[https://arxiv.org/pdf/1705.08421.pdf](https://arxiv.org/pdf/1705.08421.pdf) <br>

下面是我在CSDN、知乎、B站的同步内容：<br>
CSDN：<br>
知乎：<br>
B站：<br>

# 1 Dataset‘s folder structure 数据集文件结构

![image](https://github.com/Whiffe/Custom-ava-dataset_Multi-Person-Video-Dataset-Annotation-Method-of-Spatio-Temporally-Actions/blob/95307633663fa3103a46de75220aabf1174013ca/images/DatasetFolderStructure.png)

# 2 AI platform and project download.  AI平台与项目下载
## AI platform. AI 平台
The AI platform I use is: [https://cloud.videojj.com/auth/register?inviter=18452&activityChannel=student_invite](https://cloud.videojj.com/auth/register?inviter=18452&activityChannel=student_invite) <br>
我使用的AI平台：[https://cloud.videojj.com/auth/register?inviter=18452&activityChannel=student_invite](https://cloud.videojj.com/auth/register?inviter=18452&activityChannel=student_invite)

The following operations are all done on this platform.<br>
以下的操作均在该平台的基础上完成。

Instance mirroring selection：Pytorch 1.8.0，python 3.8，CUDA 11.1.1 <br>
实例镜像选择：Pytorch 1.8.0，python 3.8，CUDA 11.1.1
![image](https://img-blog.csdnimg.cn/c6544a25f8a748c88ff4451cd1fceb39.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6K6h566X5py66KeG6KeJLeadqOW4hg==,size_20,color_FFFFFF,t_70,g_se,x_16)

## 2.2 project download. AI平台与项目下载
为了让项目可以快速下载，我将项目同步到了码云：[https://gitee.com/YFwinston/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset.git](https://gitee.com/YFwinston/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset.git)
```python
cd /home
git clone https://gitee.com/YFwinston/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset.git

```

# 3 数据集视频准备
The video is 1 randomly selected from the AVA dataset, and I will crop 3 10-second segments from this video:<br>
视频是从AVA数据集中随机选择了1个，我会从这个视频中裁剪出3个10秒的片段：
```python
https://s3.amazonaws.com/ava-dataset/trainval/2DUITARAsWQ.mp4
```
Execute the following code on the AI platform:<br>
在AI平台执行：
```python
cd /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset/videos
wget https://s3.amazonaws.com/ava-dataset/trainval/2DUITARAsWQ.mp4 -O ./1.mp4
```
![image](https://img-blog.csdnimg.cn/1f996811ec164f08b21f04e42220601a.png)
# 4 Video cropping and frame extraction 视频裁剪与抽帧
## 4.1 install ffmpeg 安装ffmpeg
We use ffmpeg for video cropping and frame extraction, so install ffmpeg first<br>
本文使用ffmpeg进行视频裁剪与抽帧，所以先安装ffmpeg
```python
conda install x264 ffmpeg -c conda-forge -y
```
## 4.2 video cropping 视频裁剪
Execute the code under /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset:<br>
在/home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset下执行：

```python
cd /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset
sh cut_video.sh
```
![image](https://img-blog.csdnimg.cn/8e9b191bb72e41ee96b508ad0230a4e5.png)
## 4.3 video frame 视频抽帧
Referring to the ava dataset, crop 30 frames per second <br>
参考ava数据集，每秒裁剪30帧<br>

Execute the code under /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset:<br>
在/home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset 下执行：

```python
cd /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset
bash cut_frames.sh 
```
![image](https://img-blog.csdnimg.cn/ff14789c0a3743e584ea11de15dfc517.png)
![image](https://img-blog.csdnimg.cn/334197c4599e4a12ab594dd8133730a4.png)

## 4.4 Consolidate and downscale frames 整合与缩减帧
The structure of the frames folder generated in Section 4.3 will be inconvenient in the subsequent yolov5 detection, so I put all the pictures in a folder (choose_frames_all) in the following way.<br>
4.3节中产生的frames文件夹的结构，在后续yolov5检测时会出现不方便，所以我采用下面的方式，将所有的图片放在了一个文件夹（choose_frames_all）中。<br>

It should be noted that not all images need to be detected and labeled. In the 10-second video, the detection labels are: x_000001.jpg, x_000031.jpg, x_000061.jpg, x_000091.jpg, x_0000121jpg, x_000151.jpg, x_000181. jpg, x_000211.jpg, x_000241.jpg, x_000271.jpg, x_000301.jpg.<br>
需要注意的是，并不是，所有图片都需要检测与标注，在10秒的视频中，检测标注：x_000001.jpg、x_000031.jpg、x_000061.jpg、x_000091.jpg、x_0000121jpg、x_000151.jpg、x_000181.jpg、x_000211.jpg、x_000241.jpg、x_000271.jpg、x_000301.jpg。<br>

Execute the code under /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset:<br>
在/home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset 下执行：

```python
cd /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset 
python choose_frames_all.py 10 0
```
In the above code, 10 represents the length of the video, and 0 represents the start from the 0th second.<br>
其中10代表视频长度，0代表从第0秒开始 <br>
![image](https://img-blog.csdnimg.cn/8fbb68efa7ac407db1317b1e5d202753.png)

## 4.5 Not consolidate and downscale frames 不整合的缩减帧
The consolidate and downscale frames in 4.4 is for the detection of yolov5, and not consolidate and downscale frames here is for the labeling of VIA.
4.4的整合与缩减是为了yolov5的检测，这里的不整合的缩减是为了VIA的标注。<br>
Execute the code under /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset:<br>
在/home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset 下执行：
```python
cd /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/Dataset 
python choose_frames.py 10 0
```
![image](https://img-blog.csdnimg.cn/f5501f08cd7941c692b702f0af25f985.png)
![image](https://img-blog.csdnimg.cn/9041d1ad34ca435ebd67c2f3e1bce3c8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ1Yt5p2o5biG,size_15,color_FFFFFF,t_70,g_se,x_16)

# 5 yolov5 and deep sort installation. yolov5与deep sort 安装

##  5.1 Install 安装
run the following code<br>
运行以下代码：<br>
```
cd /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/yolovDeepsort
pip install -r requirements.txt
pip install opencv-python-headless==4.1.2.30

wget https://github.com/ultralytics/yolov5/releases/download/v6.1/yolov5s.pt -O /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/yolovDeepsort/yolov5/yolov5s.pt 
mkdir -p /root/.config/Ultralytics/
wget  https://ultralytics.com/assets/Arial.ttf -O /root/.config/Ultralytics/Arial.ttf
```
The reason for using deep sort: In preparation for generating [train/val].csv, dense_proposals_[train/val/test].pkl will not use the detection results of deep sort.<br>
采用deep sort的原因：为生成[train/val].csv做准备，dense_proposals_[train/val/test].pkl不会用到deep sort的检测结果。<br>

## 5.2 Detect choose_frames_all 对choose_frames_all进行检测


```python
cd /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/yolovDeepsort
python ./yolov5/detect.py --source ../Dataset/choose_frames_all/ --save-txt --save-conf 
```
The result is stored in: /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/yolovDeepsort/yolov5/runs/detect/exp <br>
结果存储在：/home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/yolovDeepsort/yolov5/runs/detect/exp <br>

![image](https://img-blog.csdnimg.cn/f92b6d91cfb94eb0866ac37704f3d888.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ1Yt5p2o5biG,size_20,color_FFFFFF,t_70,g_se,x_16)

# 6 Generate dense_proposals_train.pkl 生成dense_proposals_train.pkl

Execute the code under /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/yolovDeepsort/mywork：
在 /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/yolovDeepsort/mywork下运行：

```python
cd /home/Custom-ava-dataset_Custom-Spatio-Temporally-Action-Video-Dataset/yolovDeepsort/mywork
python dense_proposals_train.py ../yolov5/runs/detect/exp/labels ./dense_proposals_train.pkl show
```
