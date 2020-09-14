# OpenCV

## Mat

- RGB->BGR

- scan

  ```C++
  Mat& ScanImageAndReduceIterator(Mat& I, const uchar* const table)
  {
      // accept only char type matrices
      CV_Assert(I.depth() == CV_8U);
      const int channels = I.channels();
      switch(channels)
      {
      case 1:
          {
              MatIterator_<uchar> it, end;
              for( it = I.begin<uchar>(), end = I.end<uchar>(); it != end; ++it)
                  *it = table[*it];
              break;
          }
      case 3:
          {
              MatIterator_<Vec3b> it, end;
              for( it = I.begin<Vec3b>(), end = I.end<Vec3b>(); it != end; ++it)
              {
                  (*it)[0] = table[(*it)[0]];
                  (*it)[1] = table[(*it)[1]];
                  (*it)[2] = table[(*it)[2]];
              }
          }
      }
      return I;
  }
  ```

- transform

  ```C++
  Mat lookUpTable(1, 256, CV_8U);
  uchar* p = lookUpTable.ptr();
  for( int i = 0; i < 256; ++i)
      p[i] = table[i];
       
  LUT(I, lookUpTable, J); //#include<opencv2/core.hpp>
  ```

- mask(sharpen)

  ```C++
  Mat kernel = (Mat_<char>(3,3) <<  0, -1,  0,
                                    -1,  5, -1,
                                    0, -1,  0);
                                    
  filter2D( src, dst1, src.depth(), kernel );
  //#include<#include <opencv2/imgproc/hal/hal.hpp>
  ```

  ## image

- load

  ```C++
  Mat img = imread(filename);
  //or
  Mat img = imread(filename, IMREAD_GRAYSCALE);//灰色load
  ```

- write

  ```C++
  imwrite(filename, img);
  ```

- pixel

  ```C++
  Scalar intensity = img.at<uchar>(y, x);
  Scalar intensity = img.at<uchar>(Point(x, y));
  // pay attention to the order
  
  // 3 channels
  Vec3b intensity = img.at<Vec3b>(y, x);
  uchar blue = intensity.val[0];
  uchar green = intensity.val[1];
  uchar red = intensity.val[2];
  
  // float
  vector<Point2f> points;
  //... fill the array
  Mat pointsMat = Mat(points);
  /*or*/
  Point2f point = pointsMat.at<Point2f>(i, 0);
  ```

- visualize

  ```c++
  Mat img = imread("image.jpg");
  namedWindow("image", WINDOW_AUTOSIZE);
  imshow("image", img);
  waitKey();//空等待一个key，n等待n ms
  ```

- blending

  ```C++
  addWeighted(src1.alpha,src2,beta,gamma,dst)
  //dst(I)=saturate(src1(I)∗alpha+src2(I)∗beta+gamma)
  ```

- contrast and brightness

  ```C++
  Mat lookUpTable(1, 256, CV_8U);
  uchar* p = lookUpTable.ptr();
  for( int i = 0; i < 256; ++i)
      p[i] = saturate_cast<uchar>(pow(i / 255.0, gamma_) * 255.0);
  Mat res = img.clone();
  LUT(img, lookUpTable, res);
  ```

  

