# Document for ORBextractor
Records the functionality of ORB-realted .cc files.

## ORBmatcher.cc

### ORBmatcher::ORBmatcher(float nnratio, bool checkOri): mfNNratio(nnratio), mbCheckOrientation(checkOri)
* Func: 
Constructor of ORB matcher.

* Input & Return:
```
Input ratio, orientation that should be checked.
```

* Use:
None.

### int ORBmatcher::SearchByProjection(Frame &F, const vector<MapPoint*> &vpMapPoints, const float th)
* Func: 
Note that there are **4 different types** of function of **SearchByProjection** in ORB matcher.

* Input & Return:
```
Give an example
```

* Use:
Tracking::TrackWithMotionModel(), 
Tracking::SearchLocalPoints(),
Tracking::Relocalization().

### float ORBmatcher::RadiusByViewingCos(const float &viewCos)
* Func: 
If viewing cosine is approximate 1, return 2.5, or return 4.0.

* Input & Return:
```
Input the viewing cosine value.
```

* Use:
ORBmatcher::SearchByProjection.

### bool ORBmatcher::CheckDistEpipolarLine(const cv::KeyPoint &kp1,const cv::KeyPoint &kp2,const cv::Mat &F12,const KeyFrame* pKF2)
* Func: 
Check the distance of Epipolar Line.
Since there are some constraint on geometry.
Two feature point must fullfill the equation (x'T)Fx = 0, T represent the transpose.

* Input & Return:
```
Two featured point(keypoint) that obtained from the 2D projection of image plane.
F is the fundamental matrix. 	
```

* Use:
ORBmatcher::SearchForTriangulation.

### int ORBmatcher::SearchByBoW(KeyFrame* pKF,Frame &F, vector<MapPoint*> &vpMapPointMatches)	
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::SearchByProjection(KeyFrame* pKF, cv::Mat Scw, const vector<MapPoint*> &vpPoints, vector<MapPoint*> &vpMatched, int th)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::SearchForInitialization(Frame &F1, Frame &F2, vector<cv::Point2f> &vbPrevMatched, vector<int> &vnMatches12, int windowSize)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::SearchByBoW(KeyFrame *pKF1, KeyFrame *pKF2, vector<MapPoint *> &vpMatches12)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::SearchForTriangulation(KeyFrame *pKF1, KeyFrame *pKF2, cv::Mat F12, vector<pair<size_t, size_t> > &vMatchedPairs, const bool bOnlyStereo)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::Fuse(KeyFrame *pKF, const vector<MapPoint *> &vpMapPoints, const float th)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::Fuse(KeyFrame *pKF, cv::Mat Scw, const vector<MapPoint *> &vpPoints, float th, vector<MapPoint *> &vpReplacePoint)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::SearchBySim3(KeyFrame *pKF1, KeyFrame *pKF2, vector<MapPoint*> &vpMatches12, const float &s12, const cv::Mat &R12, const cv::Mat &t12, const float th)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::SearchByProjection(Frame &CurrentFrame, const Frame &LastFrame, const float th, const bool bMono)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::SearchByProjection(Frame &CurrentFrame, KeyFrame *pKF, const set<MapPoint*> &sAlreadyFound, const float th , const int ORBdist)	
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### void ORBmatcher::ComputeThreeMaxima(vector<int>* histo, const int L, int &ind1, int &ind2, int &ind3)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

### int ORBmatcher::DescriptorDistance(const cv::Mat &a, const cv::Mat &b)
* Func: 
Functionality.

* Input & Return:
```
Give an example
```

* Use:
Caller1, Caller2.

## Concept of some technique

### FAST

### BRIEF

### Fundamental matrix

### Epipolar Geometry

### Triangulation
	

## Learning Website 
* https://www.csie.ntu.edu.tw/~cyy/courses/vfx/05spring/lectures/scribe/08scribe2.pdf
* https://www.csie.ntu.edu.tw/~cyy/courses/vfx/05spring/lectures/scribe/10scribe.pdf