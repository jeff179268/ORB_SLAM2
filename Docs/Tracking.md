# Document for Tracking

## Tracking::Tracking(System *pSys, ORBVocabulary* pVoc, FrameDrawer *pFrameDrawer, MapDrawer *pMapDrawer, Map *pMap, KeyFrameDatabase* pKFDB, const string &strSettingPath, const int sensor): mState(NO_IMAGES_YET), mSensor(sensor), mbOnlyTracking(false), mbVO(false), mpORBVocabulary(pVoc), mpKeyFrameDB(pKFDB), mpInitializer(static_cast&lt;Initializer*&gt;(NULL)), mpSystem(pSys), mpViewer(NULL), mpFrameDrawer(pFrameDrawer), mpMapDrawer(pMapDrawer), mpMap(pMap), mnLastRelocFrameId(0)



## void Tracking::SetLocalMapper(LocalMapping *pLocalMapper)
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::SetLoopClosing(LoopClosing *pLoopClosing)
```
Input:
	pLoopClosing:

Output:
```

Function:

## void Tracking::SetViewer(Viewer *pViewer)
```
Input:
	pViewer:

Output:
```

Function:

## cv::Mat Tracking::GrabImageStereo(const cv::Mat &imRectLeft, const cv::Mat &imRectRight, const double &timestamp)
```
Input:
	imRectLeft:
    imRectRight
    timestamp

Output:
```

Function:

## cv::Mat Tracking::GrabImageRGBD(const cv::Mat &imRGB,const cv::Mat &imD, const double &timestamp)
```
Input:
	pLocalMapper:

Output:
```

Function:

## cv::Mat Tracking::GrabImageMonocular(const cv::Mat &im, const double &timestamp)
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::Track()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::StereoInitialization()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::MonocularInitialization()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::CreateInitialMapMonocular()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::CheckReplacedInLastFrame()
```
Input:
	pLocalMapper:

Output:
```

Function:

## bool Tracking::TrackReferenceKeyFrame()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::UpdateLastFrame()
```
Input:
	pLocalMapper:

Output:
```

Function:

## bool Tracking::TrackWithMotionModel()
```
Input:
	pLocalMapper:

Output:
```

Function:

## bool Tracking::TrackLocalMap()
```
Input:
	pLocalMapper:

Output:
```

Function:

## bool Tracking::NeedNewKeyFrame()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::CreateNewKeyFrame()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::SearchLocalPoints()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::UpdateLocalMap()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::UpdateLocalPoints()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::UpdateLocalKeyFrames()
```
Input:
	pLocalMapper:

Output:
```

Function:

## bool Tracking::Relocalization()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::Reset()
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::ChangeCalibration(const string &strSettingPath)
```
Input:
	pLocalMapper:

Output:
```

Function:

## void Tracking::InformOnlyTracking(const bool &flag)
```
Input:
	pLocalMapper:

Output:
```

Function:

