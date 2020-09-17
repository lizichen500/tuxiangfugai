#include<iostream>
#include<opencv2\opencv.hpp>
#include <opencv2\imgproc\imgproc.hpp>
#include <opencv2\highgui\highgui.hpp>
#include <opencv2\photo\photo.hpp>

using namespace cv;
using namespace std;
Mat src, gray;

void creat(int, void*);
int thress = 235;
Mat czphoto = Mat::zeros(gray.size(), gray.type());
int main(int argc, void* argv)
{
	namedWindow("原图像", WINDOW_GUI_NORMAL);
	namedWindow("改变后的图像", WINDOW_GUI_NORMAL);
	namedWindow("未改图", WINDOW_GUI_NORMAL);
	src = imread("E:\\VS项目\\风暴之怒.jpg");
	Mat srcs = imread("E:\\VS项目\\timg.jfif");
	if (!src.data)
	{
		cout << "can't load the picture" << endl;
	}
	cvtColor(src,gray,COLOR_BGR2GRAY);



	//调用函数
	creat(0,0);

    createTrackbar("adjust threshold", "改变后的图像",&thress,255,creat);
	imshow("未改图", srcs);
	waitKey();

	
}
void creat(int, void*) {
	threshold(gray, czphoto, thress, 255, THRESH_BINARY);

	Mat photodliate = getStructuringElement(MORPH_RECT, Size(3, 3));

	dilate(czphoto, czphoto, photodliate);

	Mat finally;
	inpaint(src, czphoto, finally, 5, INPAINT_NS);
	imshow("原图像", src);
	imshow("改变后的图像", finally);
	
}
