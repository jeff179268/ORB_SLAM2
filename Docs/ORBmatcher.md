# Document for ORBextractor
Records the functionality of ORB-realted .cc files.

## ORBmatcher.cc

### ORBmatcher::ORBmatcher(float nnratio, bool checkOri): mfNNratio(nnratio), mbCheckOrientation(checkOri)
* Func: 
Constructor of ORB matcher.

* Input & Return:
```
Input : 
		ratio, orientation that should be checked.
```

* Use:
None.

### int ORBmatcher::SearchByProjection(Frame &F, const vector<MapPoint*> &vpMapPoints, const float th)
* Func: 
Note that there are **4 different types** of function of **SearchByProjection** in ORB matcher.

* Input & Return:
```
Input : 
		Map Points, and the frame F that should be search by projection.
		A constant threshold "th".
Return: 
		Number of matches.
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
Input : 
		the viewing cosine value.

Return:
		2.5 or 4.0
```

* Use:
ORBmatcher::SearchByProjection().

### bool ORBmatcher::CheckDistEpipolarLine(const cv::KeyPoint &kp1,const cv::KeyPoint &kp2,const cv::Mat &F12,const KeyFrame* pKF2)
* Func: 
Check the distance of Epipolar Line.
Since there are some constraint on geometry.
Two feature point must fullfill the equation (x'T)Fx = 0, T represent the transpose.

* Input & Return:
```
Input : Two featured point(keypoint) that obtained from the 2D projection of image plane.
		F is the fundamental matrix.
Return: 
		Whether the equation fullfilled or not.
```

* Use:
ORBmatcher::SearchForTriangulation.

### int ORBmatcher::SearchByBoW(KeyFrame* pKF,Frame &F, vector<MapPoint*> &vpMapPointMatches)	
* Func: 
Search the Map points that aimed to match in Keyframe F by "BoW".

* Input & Return:
```
Input :
		MapPoint class and the address stored MapPoint that should be matched.
Return: 
		Number of matches.
```

* Use:
Caller1, Caller2.

### int ORBmatcher::SearchByProjection(KeyFrame* pKF, cv::Mat Scw, const vector<MapPoint*> &vpPoints, vector<MapPoint*> &vpMatched, int th)
* Func: 
Provide another way to search mapping points in key frame F.

* Input & Return:
```
Input :
		Target key frame F.
		Scw 
		Mapping points, with threshold th.
Return: 
		Number of matches.
```

* Use:
Tracking::TrackWithMotionModel(), 
Tracking::SearchLocalPoints(),
Tracking::Relocalization().

### int ORBmatcher::SearchForInitialization(Frame &F1, Frame &F2, vector<cv::Point2f> &vbPrevMatched, vector<int> &vnMatches12, int windowSize)
* Func: 
In order to find the initialization, and find the correspondence between initial frame and current frame.

* Input & Return:
```
Input :
		Frame 1 indicates the initial frame
		Frame 2 indicates the current frame
		vbPrevMatched stands for Points that matched before
		Window size
Return: 
		Number of matches.
```

* Use:
void Tracking::MonocularInitialization()

### int ORBmatcher::SearchByBoW(KeyFrame *pKF1, KeyFrame *pKF2, vector<MapPoint *> &vpMatches12)
* Func: 
Provide another approach to search the Map points that aimed to match in Keyframe F by "BoW".
Mathces the map points in current frame with that in reference frame.

* Input & Return:
```
Input :
		pKF1 represents the key frame that are candidates(reference frame).
		pKF2 represents the key frame that are current key frame.
		vpMatches12 represents the Map points matches.

Return: 
		Number of matches.
```

* Use:
bool Tracking::TrackReferenceKeyFrame()
bool Tracking::Relocalization()


### int ORBmatcher::SearchForTriangulation(KeyFrame *pKF1, KeyFrame *pKF2, cv::Mat F12, vector<pair<size_t, size_t> > &vMatchedPairs, const bool bOnlyStereo)
* Func: 
Help to search matches that fullfil epipolar constraint, with two frame pKF1, pKF2.

* Input & Return:
```
Input :
		A reference frame and a current frame.
		Fundamental matrix "F12".
		Matching Indices, vMatchedPairs.
		If is stereo, bOnlyStereo.
Return: 
		Number of matches.
```

* Use:
void LocalMapping::CreateNewMapPoints()

### int ORBmatcher::Fuse(KeyFrame *pKF, const vector<MapPoint *> &vpMapPoints, const float th)
* Func: 
First Check every points are **positive in depth** and **Depth must be inside the scale pyramid of the image**.
Viewing angle must be less than 60 deg. Then it'll search in a **certain radius** to match to the **most similar keypoint** in the radius.
Last, if there is already a MapPoint replace otherwise add new measurement

* Input & Return:
```
Input :
		The target frame pKF, and the mapping points that are fused.
		Threshold th.

Return:
		Number of fused.
	
```

* Use:
void LocalMapping::SearchInNeighbors()

### int ORBmatcher::Fuse(KeyFrame *pKF, cv::Mat Scw, const vector<MapPoint *> &vpPoints, float th, vector<MapPoint *> &vpReplacePoint)
* Func: 
First Check every points are **positive in depth** and **Depth must be inside the scale pyramid of the image**.
Viewing angle must be less than 60 deg. Then it'll search in a **certain radius** to match to the **most similar keypoint** in the radius.
Last, if there is already a MapPoint replace otherwise add new measurement

* Input & Return:
```
Input :
		The target frame pKF, and the mapping points that are fused.
		Scw
		Threshold th.
		Points that should be replaced
Return:
		Number of fused.
```

* Use:
void LocalMapping::SearchInNeighbors()

### int ORBmatcher::SearchBySim3(KeyFrame *pKF1, KeyFrame *pKF2, vector<MapPoint*> &vpMatches12, const float &s12, const cv::Mat &R12, const cv::Mat &t12, const float th)
* Func: 
Search the map points matches with several given matrix and constant that in reference frame in current frame.

* Input & Return:
```
Input :
		pKF1 is the current frame.
		pKF2 is the reference frame.
		Map point matches, vpMatches12
		s represents the const of **estimated scale**.
		R represents the matrix of **estimated roatation matirx**
		T represents the matrix of **estimated translation matrix**
Return: 
		Number of matches that found.
```

* Use:
bool LoopClosing::ComputeSim3()

### int ORBmatcher::SearchByProjection(Frame &CurrentFrame, const Frame &LastFrame, const float th, const bool bMono)
* Func: 
Provide another way to search mapping points in key frame F.
This is used in Monocular type camera.

* Input & Return:
```
Input : The current frame and the last frame within the range of threshold th(window).
		If if Monocular.

Return: 
		Number of matchers.

```

* Use:
Tracking::TrackWithMotionModel(), 
Tracking::SearchLocalPoints(),
Tracking::Relocalization().

### int ORBmatcher::SearchByProjection(Frame &CurrentFrame, KeyFrame *pKF, const set<MapPoint*> &sAlreadyFound, const float th , const int ORBdist)	
* Func: 
Provide another way to search mapping points in key frame F.
Number of matches only add when the best distance smaller than ORB distance.


* Input & Return:
```
Input :
		The current frame and the reference frame
		sAlreadyFound, the mapping points that are already found in ORB distance, with the threshold th(window).

Return: 
		Number of matchers.
```

* Use:
Tracking::TrackWithMotionModel(), 
Tracking::SearchLocalPoints(),
Tracking::Relocalization().

### void ORBmatcher::ComputeThreeMaxima(vector<int>* histo, const int L, int &ind1, int &ind2, int &ind3)
* Func: 
Computer the maximum in vertor histo.

* Input & Return:
```
Input :
		Three indices and its target **histo**.
Return:
		None.
```

* Use:
int ORBmatcher::SearchByProjection()

### int ORBmatcher::DescriptorDistance(const cv::Mat &a, const cv::Mat &b)
* Func: 
Compute the descriptor distance within two matrix.

* Input & Return:
```
Input : 
		Two matrix.
Return:
		Distance.
```

* Use:
int ORBmatcher::SearchByProjection()
int ORBmatcher::SearchByBoW()

## Concept of some technique

### FAST

### BRIEF

### Fundamental matrix

### Epipolar Geometry

### Triangulation
	

## Learning Website 
* https://www.csie.ntu.edu.tw/~cyy/courses/vfx/05spring/lectures/scribe/08scribe2.pdf
* https://www.csie.ntu.edu.tw/~cyy/courses/vfx/05spring/lectures/scribe/10scribe.pdf