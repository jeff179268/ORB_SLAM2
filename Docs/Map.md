# Map.cc 

## Overview

1. ## Map::Map()
    * Caller : **System::System**
    * Do : Used to **initialize(constructor)** 

2. ## void Map::AddKeyFrame(KeyFrame *pKF)
    * Caller :　**Tracking::StereoInitialization()**
    * Do : **Add keyframe** and **remember the keyframe Id** 

3. ## void Map::AddMapPoint(MapPoint *pMP)
    * Caller :　**Tracking::CreateInitialMapMonocular()**
    * Do : **Just add MapPoints**

4. ## void Map::EraseMapPoint(MapPoint *pMP)
    * Caller : **Optimizer::LocalBundleAdjustment(KeyFrame *pKF, bool* pbStopFlag, Map* pMap)**
    * Do : **Just erase MapPoints**

5. ## void Map::EraseKeyFrame(KeyFrame *pKF)
    * Caller : **KeyFrame::SetErase()**
    * Do : **Just erase the keyframe** 

6. ## void Map::SetReferenceMapPoints(const vector<MapPoint *> &vpMPs)
    * Caller : **void Tracking::StereoInitialization()**
    * Do : **Set reference MapPoints**

7. ## void Map::InformNewBigChange()
    * Caller : **LoopClosing::CorrectLoop()**
    * Do : **If the difference between the two frame is too large** remember the Id

8. ## int Map::GetLastBigChangeIdx()
    * Caller : **bool System::MapChanged()**
    * Do : **Get the last BigChange Id**
    * Output : **The Id of the BigChange**

9. ## vector<KeyFrame*> Map::GetAllKeyFrames()
    * Caller : **void System::SaveTrajectoryTUM(const string &filename)**
    * Do : **Just get all Keyframes**
    * Output : The vector contain **all keyframes**

10. ## vector<MapPoint*> Map::GetAllMapPoints()
    * Caller :　**Tracking::StereoInitialization()**
    * Do : **Just get all MapPoints**
    * Output : **The vector contain **all MapPoints**

11. ## long unsigned int Map::MapPointsInMap()
    * Caller : **Tracking::StereoInitialization()**
    * Do : Give **how many MapPoints in the Map**
    * Output : Return **the number of MapPoints**

12. ## long unsigned int Map::KeyFramesInMap()
    * Caller : **Tracking::Track()**
    * Do : Give **how many Keyframes in the Map**
    * Output : Return **the number of Keyframes**

13. ## vector<MapPoint*> Map::GetReferenceMapPoints()
    * Caller : **MapDrawer::DrawMapPoints()**
    * Do : Give **how many MapPoints in the ReferenceMap**
    * Output :  Return **the number of MapPoints in the  ReferenceMap**

14. ## long unsigned int Map::GetMaxKFid()
    * Caller : **Optimizer::OptimizeEssentialGraph()**
    * Do : Give the keyframe **which has the most MapPoints**
    * Output : **The keyframe Id**

15. ## void Map::clear()
    * Caller : **Tracking::UpdateLocalPoints()**
    * Do : **Just clear the map**