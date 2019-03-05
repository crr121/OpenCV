## 第一个程序图像显示

```c++
#include <opencv2/opencv.hpp> //头文件
using namespace cv; //包含cv命名空间

int main()
{
	// 【1】读入一张图片
	Mat img = imread("1.jpg");
	// 【2】在窗口中显示载入的图片
	imshow("【载入的图片】", img);
	// 【3】等待6000 ms后窗口自动关闭
	waitKey(6000);
}
```

## 第二个程序图像腐蚀

```c++

//-----------------------------------【头文件包含部分】---------------------------------------
//		描述：包含程序所依赖的头文件
//---------------------------------------------------------------------------------------------- 
#include <opencv2/highgui/highgui.hpp>//OpenCV highgui模块头文件
#include <opencv2/imgproc/imgproc.hpp>//OpenCV图像处理头文件

//-----------------------------------【命名空间声明部分】---------------------------------------
//		描述：包含程序所使用的命名空间
//-----------------------------------------------------------------------------------------------  
using namespace cv;

//-----------------------------------【main( )函数】--------------------------------------------
//		描述：控制台应用程序的入口函数，我们的程序从这里开始
//-----------------------------------------------------------------------------------------------
int main()
{
	//载入原图  
	Mat srcImage = imread("1.jpg");
	//显示原图
	imshow("【原图】腐蚀操作", srcImage);
	//进行腐蚀操作 
	Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
	Mat dstImage;
	erode(srcImage, dstImage, element);
	//显示效果图 
	imshow("【效果图】腐蚀操作", dstImage);
	waitKey(0);

	return 0;
}
```

![image](https://ws4.sinaimg.cn/large/005LymWFgy1g0l910b2kaj30p90gd7gz.jpg)

## 第三个程序图像模糊

```c++
//---------------------------------【头文件、命名空间包含部分】---------------------------
//		描述：包含程序所使用的头文件和命名空间
//-----------------------------------------------------------------------------------------------
#include "opencv2/highgui/highgui.hpp" 
#include "opencv2/imgproc/imgproc.hpp" 
using namespace cv;

//-----------------------------------【main( )函数】--------------------------------------------
//		描述：控制台应用程序的入口函数，我们的程序从这里开始
//-----------------------------------------------------------------------------------------------
int main()
{
	//【1】载入原始图
	Mat srcImage = imread("1.jpg");

	//【2】显示原始图
	imshow("均值滤波【原图】", srcImage);

	//【3】进行均值滤波操作
	Mat dstImage;
	blur(srcImage, dstImage, Size(7, 7));

	//【4】显示效果图
	imshow("均值滤波【效果图】", dstImage);

	waitKey(0);
}
```



![image](https://ws3.sinaimg.cn/large/005LymWFgy1g0l9ehzem9j30rr0lp7wh.jpg)

## 第四个程序边缘检测



```c++
//---------------------------------【头文件、命名空间包含部分】----------------------------
//		描述：包含程序所使用的头文件和命名空间
//-------------------------------------------------------------------------------------------------
#include <opencv2/opencv.hpp>
#include<opencv2/imgproc/imgproc.hpp>
using namespace cv;

//-----------------------------------【main( )函数】--------------------------------------------
//		描述：控制台应用程序的入口函数，我们的程序从这里开始
//-------------------------------------------------------------------------------------------------
int main()
{
	//【0】载入原始图  
	Mat srcImage = imread("1.jpg");  //工程目录下应该有一张名为1.jpg的素材图
	imshow("【原始图】Canny边缘检测", srcImage); 	//显示原始图 
	Mat dstImage, edge, grayImage;	//参数定义

	//【1】创建与src同类型和大小的矩阵(dst)
	dstImage.create(srcImage.size(), srcImage.type());

	//【2】将原图像转换为灰度图像
	//此句代码的OpenCV2版为：
	//cvtColor( srcImage, grayImage, CV_BGR2GRAY );
	//此句代码的OpenCV3版为：
	cvtColor(srcImage, grayImage, COLOR_BGR2GRAY);

	//【3】先用使用 3x3内核来降噪
	blur(grayImage, edge, Size(3, 3));

	//【4】运行Canny算子
	Canny(edge, edge, 3, 9, 3);

	//【5】显示效果图 
	imshow("【效果图】Canny边缘检测", edge);

	waitKey(0);

	return 0;
}
```

![image](https://ws2.sinaimg.cn/large/005LymWFgy1g0l9r2k265j30xk0dedzb.jpg)

函数解析：

cvtColor():将原图像转为灰度图像

```c++

/** @brief Converts an image from one color space to another.

The function converts an input image from one color space to another. 

@param src input image: 8-bit unsigned, 16-bit unsigned ( CV_16UC... ), or single-precision
floating-point.
@param dst output image of the same size and depth as src.
@param code color space conversion code (see #ColorConversionCodes).
@param dstCn number of channels in the destination image; if the parameter is 0, the number of the
channels is derived automatically from src and code.

@see @ref imgproc_color_conversions
 */
CV_EXPORTS_W void cvtColor( InputArray src, OutputArray dst, int code, int dstCn = 0 );
```



blur()函数：图像模糊已达到降噪的目的

```c++
/*The function smooths an image using the kernel:
@param src input image; it can have any number of channels, which are processed independently, but the depth should be CV_8U, CV_16U, CV_16S, CV_32F or CV_64F.
@param dst output image of the same size and type as src.
@param ksize blurring kernel size.
@param anchor anchor point; default value Point(-1,-1) means that the anchor is at the kernel
center.
@param borderType border mode used to extrapolate pixels outside of the image, see #BorderTypes
@sa  boxFilter, bilateralFilter, GaussianBlur, medianBlur
 */
CV_EXPORTS_W void blur( InputArray src, OutputArray dst,
                        Size ksize, Point anchor = Point(-1,-1),
                        int borderType = BORDER_DEFAULT );
```

Canny()函数：进行边缘检测

```c++
/** @brief Finds edges in an image using the Canny algorithm @cite Canny86 .

The function finds edges in the input image and marks them in the output map edges using the
Canny algorithm. The smallest value between threshold1 and threshold2 is used for edge linking. The
largest value is used to find initial segments of strong edges. 

@param image 8-bit input image.
@param edges output edge map; single channels 8-bit image, which has the same size as image .
@param threshold1 first threshold for the hysteresis procedure.
@param threshold2 second threshold for the hysteresis procedure.
@param apertureSize aperture size for the Sobel operator.
@param L2gradient a flag, indicating whether a more accurate \f$L_2\f$ norm
\f$=\sqrt{(dI/dx)^2 + (dI/dy)^2}\f$ should be used to calculate the image gradient magnitude (
L2gradient=true ), or whether the default \f$L_1\f$ norm \f$=|dI/dx|+|dI/dy|\f$ is enough (
L2gradient=false ).
 */
CV_EXPORTS_W void Canny( InputArray image, OutputArray edges,
                         double threshold1, double threshold2,
                         int apertureSize = 3, bool L2gradient = false );
```

参考

[【浅墨的游戏编程Blog】毛星云（浅墨）的专栏](https://blog.csdn.net/poem_qianmo)](https://blog.csdn.net/poem_qianmo/article/category/1923021)

openCV3编程入门