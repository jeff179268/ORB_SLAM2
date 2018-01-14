# Keyframe.cc 

## Overview
1. ## KeyFrame::KeyFrame(Frame &F, Map *pMap, KeyFrameDatabase *pKFDB)  
    * Caller : **Tracking::StereoInitialization** , used to **initialize(constructor)**.
    * Do : Store the map point and set the position of keyframe .

2. ## KeyFrame::ComputeBoW()
    * Caller : **TrackReferenceKeyFrame**
    * DO : Transfer the **feature vectors to bows vectors**

3. ## KeyFrame::SetPose(const cv::Mat &Tcw_)
    * Caller : **KeyFrame::KeyFrame**
    * Do : Use to set the **camera center(Frame's Coordinate System)** of the first keyframe , including stereo and monocular
    * Output : a **cv::mat**,including **stereo and monocular**

4. ## cv::Mat KeyFrame::GetPose()
    * Caller : **MapDrawer**
    * Do : Get the **position of frame in the world coordinate system**
    * Output : a **cv::mat**,including **the position of the frame**

5. ## cv::Mat KeyFrame::GetPoseInverse()       
    * Caller : **Track**
    * Do : Get the **position of frame in the frame coordinate system**
    * Output : a **cv::mat**,including **the position of the frame**

6. ## cv::Mat KeyFrame::GetCameraCenter() ## Monocular
    * Caller : **Track**
    * Do : Get the **position of camera center(Frame's Coordinate System)**
    * Output : a **cv::mat**,including **the position of the camera center**

7. ## cv::Mat KeyFrame::GetStereoCenter() ## Srereo
    * Caller : **Track**
    * Do : Get the **position of camera center(Frame's Coordinate System)**
    * Output : a **cv::mat**,including **the position of the camera center**

8. ## cv::Mat KeyFrame::GetTranslation()
    * Caller : **Track**
    * Do : Get the **translation matrix**
    * Output : a **cv::mat**,including **translation matrix**

9. ## cv::Mat KeyFrame::GetRotation()
    * Caller : **Track**
    * Do : Get the **rotation matrix**
    * Output : a **cv::mat**,including **rotation matrix**
