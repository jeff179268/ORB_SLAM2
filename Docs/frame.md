# Frame.cc 

## Overview
1. ## Frame::Frame(const cv::Mat &imLeft, const cv::Mat &imRight, const double &timeStamp, ORBextractor* extractorLeft, ORBextractor* extractorRight, ORBVocabulary* voc, cv::Mat &K, cv::Mat &distCoef, const float &bf, const float &thDepth):mpORBvocabulary(voc),mpORBextractorLeft(extractorLeft),mpORBextractorRight(extractorRight), mTimeStamp(timeStamp), mK(K.clone()),mDistCoef(distCoef.clone()), mbf(bf), mThDepth(thDepth),mpReferenceKF(static_cast<KeyFrame*>(NULL))
    * Caller: **Tracking::GrabImageStereo()**
    * Do : used to **initialize(constructor)** and **get some value** (**Stereo**)

2. ##  Frame::Frame(const cv::Mat &imGray, const cv::Mat &imDepth, const double &timeStamp, ORBextractor* extractor,ORBVocabulary* voc, cv::Mat &K, cv::Mat &distCoef, const float &bf, const float &thDepth):mpORBvocabulary(voc),mpORBextractorLeft(extractor),mpORBextractorRight(static_cast<ORBextractor*>(NULL)), mTimeStamp(timeStamp), mK(K.clone()),mDistCoef(distCoef.clone()), mbf(bf), mThDepth(thDepth)
    * Caller : **cv::Mat Tracking::GrabImageRGBD()**
    * Do :  used to **initialize(constructor)** and **get some value** (**RGBD**)

3. ## Frame::Frame(const cv::Mat &imGray, const cv::Mat &imDepth, const double &timeStamp, ORBextractor* extractor,ORBVocabulary* voc, cv::Mat &K, cv::Mat &distCoef, const float &bf, const float &thDepth):mpORBvocabulary(voc),mpORBextractorLeft(extractor),mpORBextractorRight(static_cast<ORBextractor*>(NULL)),mTimeStamp(timeStamp), mK(K.clone()),mDistCoef(distCoef.clone()), mbf(bf), mThDepth(thDepth)
    * Caller : **cv::Mat Tracking::GrabImageMonocular()**
    * Do : used to **initialize(constructor)** and **get some value** (**Monocular**)

4. ## void Frame::AssignFeaturesToGrid() 
    * Caller : **Frame::Frame()**
    * Do : Assign the feature to **the grid(64*48)**

5. ## void Frame::ExtractORB(int flag, const cv::Mat &im)
    * Caller : **ExtractORB()**
    * Do : **Extract the orb_features**

6. ## void Frame::SetPose(cv::Mat Tcw)
    * Caller : **Frame::Frame()**
    * Do : Set the **Frame position** in the **world coordinate system**

7. ## void Frame::UpdatePoseMatrices()
    * Caller : **Frame::SetPose()**
    * Do : According to the frame position getting the **camera center in the frame corrdinate system** 

8. ## bool Frame::isInFrustum(MapPoint *pMP, float viewingCosLimit)
    * Caller : **Tracking::SearchLocalPoints()**
    * Do : Determine **if a MapPoint is within the viewing angle** and **store the scalelevel and view angel**
    * Output : **True or Flase**

9. ## vector<size_t> Frame::GetFeaturesInArea(const float &x, const float  &y, const float  &r, const int minLevel, const int maxLevel) const
    * Caller : **ORBmatcher::Fuse()**
    * Do : Get the MapPoints in the specified area
    * Output : A array that **contain the MapPoint in the specified area**

10. ## bool Frame::PosInGrid(const cv::KeyPoint &kp, int &posX, int &posY)
    * Caller : **Frame::AssignFeaturesToGrid()**
    * Do : **Determine if a MapPoint is within the grid or not**
    * Output :**True or False**

11. ## void Frame::ComputeBoW()
    *  Caller : **bool Tracking::Relocalization()**
    *  Do : Transfer the **feature vectors to bows vectors**

12. ## void Frame::UndistortKeyPoints()
    * Caller : **Frame::Frame()**
    * Do : According to the **distort parameters updating the position of the MapPoints**

13. ## void Frame::ComputeImageBounds(const cv::Mat &imLeft)
    * Caller : **Frame::Frame()**
    * Do : **If the image has be distorted update the image bounds if not keep the origin size**

14. ## void Frame::ComputeStereoFromRGBD(const cv::Mat &imDepth)
    * Caller :　**Frame::Frame()**
    * Do : **Match the MapPoints and depths**
    
15. ## cv::Mat Frame::UnprojectStereo(const int &i)
    * Caller : **Tracking::StereoInitialization()**
    * Do :  project the MapPoints to the **world coordinate system**