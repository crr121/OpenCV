## 读取视频

```c++

//---------------------------------【头文件、命名空间包含部分】----------------------------
//		描述：包含程序所使用的头文件和命名空间
//-------------------------------------------------------------------------------------------------
#include <opencv2\opencv.hpp>  
using namespace cv;

//-----------------------------------【main( )函数】--------------------------------------------
//		描述：控制台应用程序的入口函数，我们的程序从这里开始
//-------------------------------------------------------------------------------------------------
int main()
{
	//【1】读入视频
	VideoCapture capture("1.avi");//在实例化VideoCapture的同时进行初始化
	//另外一种写法
	/*videocapture capture;
	capture.open("1.avi");*/
	//【2】循环显示每一帧
	while (1)
	{
		Mat frame;//定义一个Mat变量，用于存储每一帧的图像
		capture >> frame;  //读取当前帧
		if (frame.empty())
		{
			break;
		}
		imshow("读取视频", frame);  //显示当前帧
		waitKey(30);  //延时30ms
	}
	return 0;
}

```

读取当前帧：

```c++
  /** @brief Stream operator to read the next video frame.
    @sa read()
    */
    virtual VideoCapture& operator >> (CV_OUT Mat& image);

    /** @brief Grabs, decodes and returns the next video frame.

    @param [out] image the video frame is returned here. If no frames has been grabbed the image will be empty.
    @return `false` if no frames has been grabbed

    @note In @ref videoio_c "C API", functions cvRetrieveFrame() and cv.RetrieveFrame() return image stored inside the video capturing structure. It is not allowed to modify or release the image! You can copy the frame using cvCloneImage and then do whatever you want with the copy.
     */
    CV_WRAP virtual bool read(OutputArray image);
```

## 调用摄像头

```c++
//---------------------------------【头文件、命名空间包含部分】----------------------------
//		描述：包含程序所使用的头文件和命名空间
//-------------------------------------------------------------------------------------------------
#include <opencv2\opencv.hpp>  
using namespace cv;

//-----------------------------------【main( )函数】--------------------------------------------
//		描述：控制台应用程序的入口函数，我们的程序从这里开始
//-------------------------------------------------------------------------------------------------
int main()
{
	//【1】从摄像头读入视频
	VideoCapture capture(0);

	//【2】循环显示每一帧
	while (1)
	{
		Mat frame;  //定义一个Mat变量，用于存储每一帧的图像
		capture >> frame;  //读取当前帧
		imshow("读取视频", frame);  //显示当前帧
		waitKey(30);  //延时30ms
	}
	return 0;
}

```

这里调用摄像头只是简单的修改了capture的初始化参数为0就可以了

## 对采集的视频进行canny边缘检测

```c++
//---------------------------------【头文件、命名空间包含部分】----------------------------
//		描述：包含程序所使用的头文件和命名空间
//-------------------------------------------------------------------------------------------------
#include <opencv2\opencv.hpp>  
using namespace cv;

//-----------------------------------【main( )函数】--------------------------------------------
//		描述：控制台应用程序的入口函数，我们的程序从这里开始
//-------------------------------------------------------------------------------------------------
int main()
{
	//【1】从摄像头读入视频
	VideoCapture capture(0);
	Mat edges;

	//【2】循环显示每一帧
	while (1)
	{
		Mat frame;  //定义一个Mat变量，用于存储每一帧的图像
		capture >> frame;  //读取当前帧

		//将原图像转为灰度图像
		cvtColor(frame,edges,COLOR_BGR2GRAY);
		//使用3X3内核进行降噪（2X3+1 = 7）
		blur(edges, edges, Size(7, 7));
		//进行canny边缘检测并显示
		Canny(edges, edges, 0, 30, 3);
		imshow("被Canny后的视频", frame);  //显示当前帧
		if (waitKey(30) >= 0) break;
		//waitKey(30);  //延时30ms
	}
	return 0;
}

```



参考

[【浅墨的游戏编程Blog】毛星云（浅墨）的专栏](https://blog.csdn.net/poem_qianmo)](https://blog.csdn.net/poem_qianmo/article/category/1923021)

openCV3编程入门



