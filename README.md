# NeAudioVedioDecodeTheory 音视频编解码原理

## 一、音频
### 1.1 概念
音频: 指人耳可以听到的声音频率在20HZ-20KHZ之间的声波，称为音频。
#### 1.1.1 奈奎斯特采样定理
fs >= 2fH

#### 1.1.2 采样频率
即取样频率，指每秒钟取得声音样本的次数。采样频率越高，声音的质量也就越好，声音的还原也就越真实，但同时它占的资源比较多。由于人耳的分辨率很有限，太高的频率并不能分辨出来。  
22050的采样频率是常用的，44100已是CD音质，超过48000或96000的采样频率对人耳已经没有意义。这和电影的每秒24帧图片的道理差不多。  
如果是双声道（stereo），采样就是双份的，文件也差不多要大一倍。  

#### 1.1.3 采样位数
即采样值或取样值（也就是将采样样本幅度量化），它是用来衡量声音波动变化的一个参数，也可以说是声卡的分辨率。它的数值越大，分辨率也就越高，所发出声音的能力也就越强。  
每个采样数据记录的是振幅，采样精度取决于采样位数的大小：  
> 1字节（也就是8bit）只能记录256个数，也就是只能将振幅划分为256个等级；  
> 2字节（也就是16bit）可以细分到65536个数，这已是CD标准了；  
> 4字节（也就是32bit）能把振幅细分到4294967296个等级，实在是没有必要了。  

#### 1.1.4 通道数
即声音的通道的数目，常有单声道和立体声之分，单声道的声音只能使用一个喇叭发声（有的也处理成两个喇叭输出同一个声道的声音），立体声可以使两个喇叭都发声（一般左右声道有分工），
更能感受到空间的效果。当然，还有更多的通道数。

#### 1.1.5 帧
帧记录了一个声音单元，其长度为样本长度（采样位数）和通道数的乘积。

#### 1.1.6 周期
音频设备一次处理所需要的帧数，对于音频设备的数据访问以及音频数据的存储，都是以此为单位的。

#### 1.1.7 交错模式
数字音频信号存储的方式。数据以连续帧的方式存放，即首先记录帧1的左声道样本和右声道样本，再开始帧2的记录。

#### 1.1.8 非交错模式
首先记录的是一个周期内所有帧的左声道样本，再记录所有右声道样本。

#### 1.1.9 比特率
每秒的传输速率（位速，也叫比特率）。如705.6kbps或705600bps，其中的b是bit，ps是每秒的意思，表示每秒传输705600bit的数据量。

### 1.2 音频编码过程
#### 1.2.1 音频信号数字化
信号的数字化就是将连续的模拟信号转换成离散的数字信号，一般要完成采样、量化和编码三个步骤，如下图所示。采样是指用每隔一定时间间隔的信号样本值序列来代替原来在时间上连续的信号。
量化是用有限个幅度近似表示原来在时间上连续变化的幅度值，把模拟信号的连续幅度变为有限数量、有一定时间间隔的离散值。编码则是按照一定的规律，把量化后的离散值用二进制数码表示。
上述数字化的过程又称为脉冲编码调制（Plus Code Modulation），通常由A/D转换器来实现。  

![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/audio1.png)  

数字音频信号经过处理，记录或传输后，当需要重现声音时，还必须还原为连续变化的模拟信号。将数字信号转换为模拟信号为D/A转换。  
数字音频的质量取决于采样频率和量化位数。采样频率越高，量化位数越多，数字化后的音频质量越高。  

## 二、视频
### 2.1 概念
#### 2.1.1 什么是图像？什么是视频？
图像：是人对视觉感知的物质再现。三维自然场景的对象包括：深度、纹理和亮度信息。二维图像：纹理和亮度信息。  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video1.png)  
视频：连续的图像。视频由多幅图像构成，包含对象的运动信息，又称为运动图像。  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video2.png)  

#### 2.1.2 何为数字视频？
数字视频可以理解为自然场景空间和时间的数字采样表示。  
空间采样的主要技术指标为：  
解析度（Resolution）  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video3.png)  
时间采样的技术指标为：  
帧率（帧/秒）  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video4.png)  

#### 2.1.3 数字视频系统的构成和运行原理
* 采样：照相机  
* 处理：编解码器，传输设备  
* 显示：显示器  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video5.png) 

#### 2.1.4 人类视觉系统HVS
HVS的构成：  
* 眼睛  
* 神经  
* 大脑  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video6.png)  
HVS特点：  
* 对高频信息不敏感  //24-40HZ 刷新频率（低频 < 60HZ < 高频）
* 对高对比度更敏感  
* 对亮度信息比色度信息更敏感  
* 对运动的信息更敏感  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video7.png)  

#### 2.1.5 针对HVS的特点，数字视频系统的设计应该考虑哪些因素？ 
* 丢弃高频信息，只编码低频信息  
* 提高边缘信息的主观质量    
* 降低色度的解析度  
* 对感兴趣区域（Region of Interesting, ROI）进行特殊处理  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video8.png)  

#### 2.1.6 RGB颜色空间
三原色分别是红（R），绿（G），蓝（B）。任何颜色都可以通过按一定比例混合三原色产生。  
RGB色度空间：  
* 有RGB三原色组成  
* 广泛用于BMP，TIFF，PPM等  
* 每个色度成分通常用8bit表示[0, 255]  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video9.png)  

#### 2.1.7 YUV颜色空间
YUV色彩空间是指，Y：亮度分量，UV：两个色度分量。YUV能更好地反映HVS特点。  

#### 2.1.8 RGB转换到YUV空间
亮度分量Y与三原色有如下关系：  
Y = k<sup>r</sup>R + k<sup>g</sup>G + k<sup>b</sup>B  
经过大量实验ITU-R 给出了，k<sup>r</sup> = 0.299 k<sup>g</sup> = 0.587，k<sup>b</sup> = 0.114
主流的编解码标准的压缩对象都是YUV图像。  
**YUV格式**  
YUV中的Y代表的是亮度信号，亮度信号又可以称之为灰度信息。经历过黑白电视机时代的人应该知道黑白电视只能显示不带颜色的图像即灰度图像，这是因为黑白电视只能接收Y信号。
可以类比水墨画，可以认为水墨画中只包含Y信号。  
UV合起来表示色彩信号。有一种UV表示色彩信号的方式称为CbCr。U=Cb表示当前颜色对蓝色的偏移；V=Cr表示当前颜色对红色的偏移，如下图所示：  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video10.png)  
YUV其实就是用Y,U,V三个分量来保存一个像素的显示（亮度和色彩）信息。  
由于YUV格式的是将亮度和色彩信息分离存储，在黑白电视和彩色电视共存的年代，可以用YUV同时兼容两种电视机。下图为YUV图像分别只显示Y，U，V信息时的效果  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video11.png)  

#### 2.1.9 YUV图像分量采样 
YUV图像可以根据HVS的特点，对色度进行分量采样，可以降低视频数据量。  
根据亮度和色度分量的采样比率，YUV图像通常有以下几种分量方式：  
用三个图来直观地表示采集的方式，以黑点表示采样该像素点的Y分量，以空心圆圈表示采用该像素的UV分量。  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video12.png)  

三种分量对应的格式：  
![image](https://github.com/tianyalu/NeAudioVedioDecodeTheory/blob/master/show/video13.png)  








