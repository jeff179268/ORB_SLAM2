# Document for Tracking

## Tracking::Tracking

Function:

	Mainly load parameter from xml file(calibration)
	1. Load camera parameters from settings file
    	(1) Intrinsic camera matrix
            fx  0   cx
            0   fy  cy
            0   0   0
    	(2) Distortion matrix
            k1
            k2
            p1
            p2
	    (3) bf: Horizontal focal length (in pixels) * Baseline (in meters)
        (4) fps: Default to 30 FPS
        (5) Max/Min Frames to insert keyframes and to check relocalisation
        (6) nRGB: 1 -> RGB, 0 -> BGR

	2. Load ORB parameters
		(1) nFeatures: Number of Features
        (2) fScaleFactor: Scale Factor
        (3) nLevels: Scale Levels
        (4) fIniThFAST: Initial Fast Threshold
        (5) fMinThFAST: Minimum Fast Threshold
      	(6) mThDepth(STEREO, RGBD): Depth Threshold (Close/Far Points)
        (7) mDepthMapFactor(RGBD)


## void Tracking::SetLocalMapper(LocalMapping *pLocalMapper)
```
Input:
	pLocalMapper: definition in LocalMapping.cc
```

Function:
	Let LocalMapping use in Tracking interface.


## void Tracking::SetLoopClosing(LoopClosing *pLoopClosing)
```
Input:
	pLoopClosing: definition in LoopClosing.cc
```

Function:
	Let LoopClosing use in Tracking interface.

## void Tracking::SetViewer(Viewer *pViewer)
```
Input:
	pViewer: definition in Viewer.cc
```

Function:
	Let Viewer use in Tracking interface.

## cv::Mat Tracking::GrabImageStereo(const cv::Mat &imRectLeft, const cv::Mat &imRectRight, const double &timestamp)
```
Input:
	imRectLeft: left image matrix
    imRectRight: right image matrix
    timestamp: to cut frame in Frame()

Output:
	mCurrentFrame.mTcw.clone(): return cloning frame
```

Function:
	For stereo mode, we should have left and right image with type of cv::Mat, then use mImGray and mbRGB distinguish RGB or RGBA and RGB or BGR. Finally, make the frame and call Track().
	[cvtColor(src, goal, mode): Convert color]

## cv::Mat Tracking::GrabImageRGBD(const cv::Mat &imRGB,const cv::Mat &imD, const double &timestamp)
```
Input:
	imRGB: RGB image matrix
    imD: Depth image matrix
    timestamp: to cut frame in Frame()

Output:
	mCurrentFrame.mTcw.clone(): return cloning frame
```

Function:
	For RGBD mode, use mbRGB distinguish RGB or RGBA and RGB or BGR of imRGB(RGB image). Then, adjust depth images. Finally, make the frame and call Track().
	cvtColor(src, goal, mode): Convert color

## cv::Mat Tracking::GrabImageMonocular(const cv::Mat &im, const double &timestamp)
```
Input:
	im: image matrix
    timestamp: to cut frame in Frame()

Output:
	mCurrentFrame.mTcw.clone(): return cloning frame
```

Function:
	Since Moncular mode only need one image, so only need to deal with im. Then, make the frame and call Track().

## void Tracking::Track()
```
Input:
	None

Output:
	None
```

Function:
	If not initialized, initialize first by Stereo(RGBD) mode and Monocular, respectively. When system initialized, Initial camera pose estimation using motion model or relocalization (if tracking is lost). Then, Local Mapping is activated. If state is OK, then Local Mapping might have changed some MapPoints tracked in last frame. Otherwise, system will enter Localization Mode(Local Mapping is deactivated). If the state is LOST, then it will do Relocalization(). Or it will compute two camera poses, one from motion model and one doing relocalization. If relocalization is sucessfull we choose that solution, otherwise we retain the "visual odometry" solution.

	When all things done, it will update drawer. If tracking were good, check if we insert a keyframe. Finally, store frame pose information to retrieve the complete camera trajectory afterwards.

## void Tracking::StereoInitialization()
```
Input:
	None

Output:
	None
```

Function:
	For Stereo mode or RGB mode initialization, it must have at least 500 points for each frame. If so, set Frame pose to the origin and create keyframe. Then, insert keyframe in the map and create MapPoints and asscoiate to KeyFrame. Finally, after store the information of keyframe and points, it will set state of initialization as OK.


## void Tracking::MonocularInitialization()
```
Input:
	None

Output:
	None
```

Function:
	For Monocular mode, since we only input one frame each timestamp, we need to set Reference frame. Then, it will try to find correspondences and check if there are enough correspondences. If so, then it will set frame pose and call CreateInitialMapMonocular().

## void Tracking::CreateInitialMapMonocular()
```
Input:
	None

Output:
	None
```

Function:
	In this function, it should set the KeyFrame and estimate the depth information for Monocular mode. Moreover, it wull create new map when initialization.

## void Tracking::CheckReplacedInLastFrame()
```
Input:
	None

Output:
	None
```

Function:
	Local Mapping might have changed some MapPoints tracked in last frame.

## bool Tracking::TrackReferenceKeyFrame()
```
Input:
	None

Output:
	bool type: judge if nmatchesMap>=10
```

Function:
	Compute Bag of Words vector and combine with Optimizer. Last, discard outliers and judge if nmatchesMap>=10.

## void Tracking::UpdateLastFrame()
```
Input:
	None

Output:
	None
```

Function:
	Update pose according to reference keyframe and create "visual odometry" MapPoints. Insert all close points (depth<mThDepth). If less than 100 close points, we insert the 100 closest ones.

## bool Tracking::TrackWithMotionModel()
```
Input:
	None:

Output:
	bool type: judge if nmatchesMap>=10
```

Function:
	Update last frame pose according to its reference keyframe. If few matches, uses a wider window search. Then, optimize frame pose with all matches and discard outliers.

## bool Tracking::TrackLocalMap()
```
Input:
	None

Output:
	bool type: judge if it would be better
```

Function:
	Estimation of the camera pose and some map points tracked in the frame. Then, retrieve the local map and try to find matches to points in the local map. Last, Decide if the tracking was succesful. More restrictive if there was a relocalization recently

## bool Tracking::NeedNewKeyFrame()
```
Input:
	None

Output:
	bool type: judge if the system need new key frame
```

Function:
	If Local Mapping is freezed by a Loop Closure or if not enough frames have passed from last relocalisation do not insert keyframes. And it will judge 2 things to determine if it should need a new KeyFrame.
    1. (1) More than "MaxFrames" have passed from last keyframe insertion
       (2) More than "MinFrames" have passed and Local Mapping is idle
       (3) Few tracked points compared to reference keyframe. Lots of visual odometry compared to map matches.
    2. Few tracked points compared to reference keyframe. Lots of visual odometry compared to map matches.
    If the mapping accepts keyframes, insert keyframe. Otherwise send a signal to interrupt BA.

## void Tracking::CreateNewKeyFrame()
```
Input:
	None

Output:
	None
```

Function:
	The goal is to create new keyframe. We sort points by the measured depth by the stereo/RGBD sensor. We create all those MapPoints whose depth < mThDepth. If there are less than 100 close points we create the 100 closest.

## void Tracking::SearchLocalPoints()
```
Input:
	None

Output:
	None
```

Function:
	Do not search map points already matched. Project points in frame and check its visibility. Use nToMatch to calculate match points and use matcher to store.

## void Tracking::UpdateLocalMap()
```
Input:
	None

Output:
	None
```

Function:
	call UpdateLocalKeyFrames() and UpdateLocalPoints() respectively. When TrackLocalMap() is called, it will update local map.

## void Tracking::UpdateLocalPoints()
```
Input:
	None

Output:
	None
```

Function:
	use pKF->GetMapPointMatches() to get map points before and judge the new frame whether have same points.

## void Tracking::UpdateLocalKeyFrames()
```
Input:
	None

Output:
	None
```

Function:
	Each map point vote for the keyframes in which it has been observed. All keyframes that observe a map point are included in the local map. Also check which keyframe shares most points. Include also some not-already-included keyframes that are neighbors to already-included keyframes.

## bool Tracking::Relocalization()
```
Input:
	None

Output:
	bool type: judge whether relocalization success or not
```

Function:
	Relocalization is performed when tracking is lost. Track Lostis that query KeyFrame Database for keyframe candidates for relocalisation. We perform first an ORB matching with each candidate. If enough matches are found we setup a PnP solver. Alternatively perform some iterations of P4P RANSAC until finding a camera pose supported by enough inliers. If the pose is supported by enough inliers stop ransacs and continue and it will return true at the end.

```
1、首先計算當前幀的BOW

2、挑選候選關鍵幀：
（1）、先挑選出和當前幀共享了詞節點的所有關鍵（哪怕隻共享了一個）
（2）、計算出一個關鍵幀共享詞節點最大的數量，然後將共享詞節點最小閾值設置為最大數量的0.8，篩掉不滿足數量的關鍵幀
（3）、計算剩下關鍵幀和當前幀的BOW相似性得分
（4）、返回所有滿足最高得分的0.75的關鍵幀

3、逐個比較當前幀和關鍵幀SearchByBoW進行匹配，成功的個數小於15的篩掉:
匹配特徵點（在同一個詞節點上進行匹配），當最好的距離和次優距離滿足一定關係匹配成功。
檢測方向，當匹配上時，統計處於三個主方向且匹配成功的個數

4、通過前面得到的匹配關係，用pnp優化估計一個當前幀的RT
再與每個符合條件的關鍵幀進行SearchByProjection()，計算滿足條件的擴充的匹配點個數進行篩選關鍵幀。當匹配個數滿足時則重定位成功

最後判斷是否滿足以上條件，決定是否重定位成功
```


## void Tracking::Reset()
```
Input:
	None

Output:
	None
```

Function:
	Reset Local Mapping, Loop Closing, BoW Database, Clear Map and Clear useable parameters.

## void Tracking::ChangeCalibration(const string &strSettingPath)
```
Input:
	strSettingPath: xml file location

Output:
	None
```

Function:
	Change camera parameter.

## void Tracking::InformOnlyTracking(const bool &flag)
```
Input:
	flag: judge whether only tracking

Output:
	None
```

Function:
	set mbOnlyTracking value.
