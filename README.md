# UnrealEngine4.27_AnselCapture
UnrealEngine4.27_AnselCapture Final Edition
# Ansel_Capture
## 修正内容
 1.修改build.cs文件，并且修复部分代码。
 2.实现插件放置位置问题，可放置在任意虚幻插件能识别到的位置，就能被启用。
 3.修改插件图标，方便识别。
## 使用说明
一个基于 Unreal Engine 4.27 修改的Nvidia_Ansel plugins 可以用于生成360 或者立体360 影片的工具 
这是基于Unreal Engine 4.27 的Ansel插件修改而来的版本。
在开始你的工作之前你需要做好一下准备：
#### 1.确认您的显卡已经安装好显卡驱动：
目前已经测试了：456.71~551.79版本的Nvidia驱动。
#### 2.假如你需要更改单帧的保存位置：请安装Nvidia的GeForceExperience组件。
**没有修改：将默认保存在系统指定的”视频文件夹下方“

（1）点击“设置”

![Pasted image 20220324120858](https://user-images.githubusercontent.com/39860733/159846088-18804c78-c19a-47ca-8edc-ea44e3d7a3af.png)
（2）关闭“游戏内覆盖”，这一步是为了确保“Ansel”能正常启动，这里为啥我也不大清楚。反正必须关掉，虚幻内部的Ansel才能启动。

![Pasted image 20220324121037](https://user-images.githubusercontent.com/39860733/159846137-8b6e1ee7-57e3-4cb8-b1bb-c78f52e559b2.png)
（3）点击“游戏内覆”盖设置

![Pasted image 20220324120645](https://user-images.githubusercontent.com/39860733/159846194-877e800a-cc0b-48e6-9712-8b22d08c5ee8.png)
（4）点击“设置”选项

![Pasted image 20220324121245](https://user-images.githubusercontent.com/39860733/159846343-153054d9-3cf0-4304-b42d-8202dafcbe8e.png)
（5）设置保存路径

![Pasted image 20220324122738](https://user-images.githubusercontent.com/39860733/159846366-b01d8273-55bc-4cd4-9ff7-26e082e122e4.png)
![Pasted image 20220324122852](https://user-images.githubusercontent.com/39860733/159846373-489b35b0-f155-4791-80d8-2cbacb7be82b.png)
#### 3.下方就是在虚幻4中普通使用的一个情景
1.先关闭场景的”自动曝光“，这里的值可以根据自己的预设值设定。

![Pasted image 20220324125255](https://user-images.githubusercontent.com/39860733/159846410-f6ff752d-283a-41c4-9fdd-45e8394a5bd6.png)  
2.调用一个简单的蓝图逻辑    
（1）打开插件资源库 ，勾选"Show Plugins Content"
![image](https://user-images.githubusercontent.com/39860733/167578994-da3e385e-24af-4a41-b2c3-2f371b59da25.png)
（2）将编写好的蓝图拖入至关卡中   
![image](https://user-images.githubusercontent.com/39860733/167579565-885e806b-b46e-4439-929d-67b6439271fa.png)
（3）打开你的sequnce关卡序列，设置合适的帧率    
![image](https://user-images.githubusercontent.com/39860733/167580316-bf242321-ada4-47d5-bd7b-ca8e483b29a5.png)
（4）打开"项目设置"，在搜索栏输入"frame".设置固定帧率（指定为你输出影片的帧率）  
![image](https://user-images.githubusercontent.com/39860733/167580779-2a51135e-1207-4cdd-8079-b3058104be27.png)
#### 蓝图概览
![image](https://user-images.githubusercontent.com/39860733/167580884-14419853-b443-486b-a134-399938952566.png)

这个蓝图的大致思路就是  
（1）在启动那一刻先让游戏的帧率稳定在固定的值，然后将 sequence播放结束的代理挂上去.触发”stop session“事件，自动结束采集。  
（2）当按下”按键1“时播放”指定的sequnce序列“，然后激活”start session“事件。  
（3）一切就绪后,点击”拍摄“，将自动开启截图。等待sequence跑完将自动关闭”Ansel“的对话框。  

![Pasted image 20220324124752](https://user-images.githubusercontent.com/39860733/159846452-6d2341ad-d0a1-4451-bc25-03b85ecd0430.png)

### 如果需要自定义蓝图，可拷贝编写好的蓝图，然后自定义自己的调用逻辑。

4.找一个批量重名名工具对单帧进行重命名：
这里我用的是：https://u.tools/
![image](https://user-images.githubusercontent.com/39860733/160042397-c18ffabf-e4c0-4127-834f-8d30dcbeca01.png)
![image](https://user-images.githubusercontent.com/39860733/160042857-4406b9f8-6caf-48d1-9bbe-a35994d76730.png)

5.最后是输出成影片：
这里我用的是：FFMEPG的方式：
这是我的配置方式： 路径需要根据自己的需求自行更改  
ffmpeg.exe -framerate 30 -i F:/Grab/UnrealEngineDemo/Frame_%4d.jpg -c:v libx264 -profile:v high -level 4.2 -r 30 -pix_fmt yuv420p -crf 18 -preset slower F:/Grab/MyMovie.mp4  
6.输出视频实例  
bilibili：https://www.bilibili.com/video/BV1J34y1b7Cr?spm_id_from=333.788.top_right_bar_window_history.content.click  
Youtube: https://youtu.be/M65mXhrFh8U   
7.加速渲染速度可以适当调节控制台变量：r.Photography.SettleFrames 的值 （范围1~10），可以设置成1（最快），  最慢是10.

### 目前已经测试各个帧率下输出情况：包含24fps,25fps,30fps,48fps,50fps,60fps
![image](https://user-images.githubusercontent.com/39860733/167581780-efde6191-2e62-440c-95e0-250ccacdfa3b.png)

