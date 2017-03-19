九宫格任务：比赛规则中大神符系统，只有在九宫格前连续击中5次九个图案中不同的那一个，才能激活buff，获得有利条件。

下面是ubuntu 下的运行过程（windows下利用cmake的GUI生成vs的工程）

#### 编译：

```
mkdir build
cd build
cmake ..
make
```

### 运行：

在/bin目录下执行（不带参数调用摄像头，带参数调用测试视频）：

```
./main
./main test1.avi
```

#### 九宫格程序流程：

* 1.识别出九宫格（九个矩形轮廓）。对视频预处理后，拟合轮廓。在识别出矩形轮廓的前提下，筛选出符合条件的矩形，在筛选的过程中修正可以参考前一帧的值修正当前帧的错误值；当检测出九个符合条件的矩形后，对九个矩形轮廓按冒泡法进行排序，至此识别九宫格（九个矩形轮廓）的部分完成。
* 2.识别出击打的目标。对九个已经排序好的九宫格进行击打目标的识别，采用图案包含像素点的多少作为判别的依据。找出像素点数量不同于其他图案的格子。当设定次数内，该图案的像素点数量都不同于其他的图案时，确定该图案就是要找的目标。
* 3.返回击打目标的位置。在第2步中，当连续在设定的次数内检测结果都为同一值时，向串口发送信号。

#### 有待改善：
目前仅向下位机发送序号，具体哪个序号打击那个点需要在比赛时临时标定，可能只需要一两秒，但这一两秒也许是制胜关键，可以改成下位机只点击一个键就能根据上位机发送的数据自动完成射击