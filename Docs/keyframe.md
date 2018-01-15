# Keyframe.cc

## Overview
1. ## KeyFrame::KeyFrame(Frame &F, Map *pMap, KeyFrameDatabase *pKFDB)
*  Caller : **Tracking::StereoInitialization**  used to **initialize(constructor)**.
*  Do : Store the map point and set the position of keyframe .

2. ## KeyFrame::ComputeBoW()
*  Caller : **TrackReferenceKeyFrame**
*  Do : Transfer the **feature vectors to bows vectors**

3. ## KeyFrame::SetPose(const cv::Mat &Tcw_)
*  Caller : **KeyFrame::KeyFrame**
*  Do : Use to set the **camera center(Frame's Coordinate System)** of the first keyframe , including stereo and monocular
*  Output : a **cv::mat** including **stereo and monocular**

4. ## cv::Mat KeyFrame::GetPose()
*  Caller : **MapDrawer()**
*  Do : Get the **position of frame in the world coordinate system**
*  Output : a **cv::mat** including **the position of the frame**

5. ## cv::Mat KeyFrame::GetPoseInverse()
*  Caller : **Track()**
*  Do : Get the **position of frame in the frame coordinate system**
*  Output : a **cv::mat** including **the position of the frame**

6. ## cv::Mat KeyFrame::GetCameraCenter() Monocular
*  Caller : **Track()**
*  Do : Get the **position of camera center(Frame's Coordinate System)**
*  Output : a **cv::mat** including **the position of the camera center**

7. ## cv::Mat KeyFrame::GetStereoCenter() Srereo
*  Caller : **Track()**
*  Do : Get the **position of camera center(Frame's Coordinate System)**
*  Output : a **cv::mat** including **the position of the camera center**

8. ## cv::Mat KeyFrame::GetTranslation()
*  Caller : **Track()**
*  Do : Get the **translation matrix**
*  Output : a **cv::mat** including **translation matrix**

9. ## cv::Mat KeyFrame::GetRotation()
*  Caller : **Track()**
*  Do : Get the **rotation matrix**
*  Output : a **cv::mat** including **rotation matrix**

10. ## void KeyFrame::AddConnection(KeyFrame *pKF, const int &weight)
*  Caller : **KeyFrame::UpdateConnections()**
*  Do : Add two keyfame to connect if they have more than enough common features

11. ## void KeyFrame::UpdateBestCovisibles()
*  Caller : **KeyFrame::AddConnection()**
*  Do : According to the **number of feature points** to updated their relation

12. ## set<KeyFrame*> KeyFrame::GetConnectedKeyFrames()
*  Caller : **Map::Save**
*  Do : Get the keyframe which is connected by the target keyframe
*  Output : A set which is connected keyframe

13. ## vector<KeyFrame*> KeyFrame::GetVectorCovisibleKeyFrames()
*  Caller : **KeyFrame::SetBadFlag()**
*  Do : Get the keyframe which is sorted by the number of map points
*  Output : A vector which is the id of the keyframe

14. ## vector<KeyFrame*> KeyFrame::GetBestCovisibilityKeyFrames(const int &N)
*  Caller : **Tracking::UpdateLocalKeyFrames()**
*  Do : Give a number N and return the **first N like keyframes**
*  Output : A vector which has first N like keyframes

15. ## vector<KeyFrame*> KeyFrame::GetCovisiblesByWeight(const int &w)
*  Caller :  **Optimizer::OptimizeEssentialGraph**
*  Do : Give a array w and return the **first n like keyframes**
*  Output :  A vector which has first N like keyframes

16. ## int KeyFrame::GetWeight(KeyFrame *pKF)
*  Caller : **KeyFrame::SetBadFlag()**
*  Do : Get a common map point according to the keyframes
*  Output : An array contain the map points

17. ## void KeyFrame::AddMapPoint(MapPoint *pMP, const size_t &idx)
*  Caller : **Tracking::StereoInitialization()**
*  Do : **Add a map points**

18. ## void KeyFrame::EraseMapPointMatch(const size_t &idx)
*  Caller : **Optimizer::BundleAdjustment**
*  Do : **Erase the map point**

19. ## void KeyFrame::ReplaceMapPointMatch(const size_t &idx, MapPoint* pMP)
*  Caller : **MapPoint::Replace**
*  Do : **Replace the map point**

20. ## set<MapPoint*> KeyFrame::GetMapPoints()
*  Caller : **ORBmatcher::Fuse**
*  Do : Get the all map points from the frames
*  Output : A set which contains **all map points from the frames**

21. ## int KeyFrame::TrackedMapPoints(const int &minObs)
*  Caller : **Tracking::NeedNewKeyFrame()**
*  Do : To observe **which map point can be detect by multiple  keyframe**
*  Output : An integer which means the number of map points **can be detect by multiple keyframes**

22. ## vector<MapPoint*> KeyFrame::GetMapPointMatches()
*  Caller : **Tracking::CreateInitialMapMonocular()**
*  Do : Get the map points from the frames
*  Output : An array pointer

23. ## void KeyFrame::UpdateConnections()
*  Caller : **Tracking::CreateInitialMapMonocular()**
*  Do : **According to the map points and update the connection and add the child**

24. ## void KeyFrame::AddChild(KeyFrame *pKF)
*  Caller : **KeyFrame::UpdateConnections()**
*  Do : Add the child to this frame

25. ## void KeyFrame::EraseChild(KeyFrame *pKF)
*  Caller : **KeyFrame::UpdateConnections()**
*  Do : Erase the child to this frame

26. ## void KeyFrame::ChangeParent(KeyFrame *pKF)
* Caller : **KeyFrame::UpdateConnections()**
* Do : Change the parent from original to another

27. ## set<KeyFrame*> KeyFrame::GetChilds()
* Caller : **UpdateLocalKeyFrames()**
* Do : Get the child of this frames
* Output : A set that contain the children of this frames

28. ## KeyFrame* KeyFrame::GetParent()
* Caller : **UpdateLocalKeyFrames()**
* Do : Get the parent of this frames
* Output : A set that contain the parent of this frames

29. ## bool KeyFrame::hasChild(KeyFrame *pKF)
* Caller : **Optimizer::OptimizeEssentialGraph()**
* Do : Detect if the frame has the child  or not
* Output : **True or False**

30. ## void KeyFrame::AddLoopEdge(KeyFrame *pKF)
* Caller : **LoopClosing::CorrectLoop()**
* Do : If it has a Loop set a flag to let it not to be erase and add a loop edge

31. ## set<KeyFrame*> KeyFrame::GetLoopEdges()
* Caller : **Optimizer::OptimizeEssentialGraph**
* Do : Get the loopedge
* Output : Return the loopedge

32. ##  void KeyFrame::SetNotErase()
* Caller : **LoopClosing::DetectLoop()**
* Do : If this frame has a loop that set a flag to let it can not to be erase

33. ## void KeyFrame::SetErase()
* Caller : **LoopClosing::DetectLoop()**
* Do : Let the frame can be Erase

34. ## void KeyFrame::SetBadFlag()
* Caller : **KeyFrame::SetErase()**
* Do : Determine if it can be erase , if it can update its connected keyframe

35. ## KeyFrame::isBad()
* Caller : **KeyFrame::TrackedMapPoints()**
* Do : Determine **if the frame is existed**

36. ## void KeyFrame::EraseConnection(KeyFrame* pKF)
* Caller : **KeyFrame::SetBadFlag()**
* Do : Erase all frames which **connected by the input keyframe** and update the **UpdateBestCovisibled**

37. ## vector<size_t> KeyFrame::GetFeaturesInArea(const float &x, const float &y, const float &r) const
* Caller : **ORBmatcher::SearchByProjection()**
* Do : Determine **how many map points in this area**
* Output : Return **the array that contains the map points**

38. ## bool KeyFrame::IsInImage(const float &x, const float &y) const
* Caller :**ORBmatcher::SearchByProjection**
* Do : Determine **if the map point is in the image(frame)**
* Output : Return ture or false

39. ## cv::Mat KeyFrame::UnprojectStereo(int i)
* Caller : **Tracking::StereoInitialization()**
* Do : Project the map points to the **world coordinate system**
* Output : The cv::mat that **contain the frames position**

40. ## float KeyFrame::ComputeSceneMedianDepth(const int q)
* Caller : **Tracking::CreateInitialMapMonocular()**
* Do : Compute **the depth of the map points**
* Output :The array that **contains the depth**
