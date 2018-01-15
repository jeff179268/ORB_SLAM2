# Document for ORBVocabulary 

Records the functionality of ORB-realted .cc files.

## ORBextractor.cc
### static float IC_Angle(const Mat& image, Point2f pt,  const vector<int> & u_max)
*Func: 
Compute angle

*Input & Return:
```
Input image and point to compute angle bt "fastAtan2"
```

*Use:
caller1 : computeOrientation().

### static void computeOrbDescriptor(const KeyPoint& kpt,const Mat& img, const Point* pattern, uchar* desc)
*Func: 
Compute ORB descriptors of keypoint "kpt"

*Input & Return:
```
Input keypoint & image. 
After computing, store the descriptor value in pointer "desc".
```

*Use:
Caller1: computeDescriptors().	

### ORBextractor::ORBextractor(int _nfeatures, float _scaleFactor, int _nlevels,int _iniThFAST, int _minThFAST):nfeatures(_nfeatures), scaleFactor(_scaleFactor), nlevels(_nlevels),iniThFAST(_iniThFAST), minThFAST(_minThFAST)

*Func: 
ORB extractor is the fusion of FAST detector& BRIEF descriptor.
The is the constructor of the class "ORBextractor".
It first set several common-used parameters, such as mvScaleFactor, mvLevelSigma2, mvImagePyramid, mnFeaturesPerLevel
under given input parameters.
Then  pre-compute the end of a row in a circular patch for orientation.


*Input & Return:
```
Constructor : nfeatures, scaleFactor, nlevels, iniThFAST, minThFAST.
```

*Use:
Whenever the use of class.

### static void computeOrientation(const Mat& image, vector<KeyPoint>& keypoints, const vector<int>& umax)
*Func: 
Compute Orientation by computing every keypoints angle.

*Input & Return:
```
Input image and keypoints.
```

*Use:
Caller1: ORBextractor::ComputeKeyPointsOctTree(), Caller2: ORBextractor::ComputeKeyPointsOld.	

### void ExtractorNode::DivideNode(ExtractorNode &n1, ExtractorNode &n2, ExtractorNode &n3, ExtractorNode &n4)
*Func: 
Define boundaries of childs and associate points to childs.

*Input & Return:
```
Input 4 node that extracted.
```

*Use:
Caller1: ORBextractor::DistributeOctTree.

### vector<cv::KeyPoint> ORBextractor::DistributeOctTree(const vector<cv::KeyPoint>& vToDistributeKeys, const int &minX, const int &maxX, const int &minY, const int &maxY, const int &N, const int &level)
*Func: 
Create a node list, after computing, there should attain the best point in each node.
Then the node list which contains the best point would be return.

*Input & Return:
```
Input the max & min of "X" and "Y" respectively, also with the input of level.
Return the result of Keys called "vResultKeys".
```

*Use:
Caller1: ComputeKeyPointsOctTree.	

### void ORBextractor::ComputeKeyPointsOctTree(vector<vector<KeyPoint> >& allKeypoints)
*Func: 
First resized to meet the level. For each level, reserve size of 10 times features for DistributeKeys.
Then push the cell that detect by "FAST" to DistributeKeys, which named "vToDistributeKeys".

Each keypoints should have the size of numbers of "nfeatures".
Compute keypoints by "DistributeOctTree()".
After that we adding border to coordinates and scale information.
Before finishing this function, we'll compute the "orientation" for "all keypoints" by "Image Pyramid".

*Input & Return:
```
Simply input "all keypoints".
Setting values within the given pointer "&allKeypoints"
```

*Use:
Caller1: ORBextractor::operator().

### void ORBextractor::ComputeKeyPointsOld(std::vector<std::vector<KeyPoint> > &allKeypoints)
*Func: 
For each level, define "featured cells". Which celled "key points".
Create cellImage with certain row, col range in each level. Reserve (5* numbers of features cell) for cellKeyPoints.
The compuing results using FAST.

After retain by score and transform coordinates, filter the best key points "cell", which should also meet the number of attained points.
Once chosen the cell, push every keypoints to "keypoints".

However, the number of keypoints in cell may not be same as we desired.
So filter "keypoints" to pick top n features that desired.

At the end of the function, compute orientations of all keypoints again with Image Pyramid.

*Input & Return:
```
Simply input "all keypoints".
Setting values within the given pointer "&allKeypoints"
```

*Use:
Caller1: ORBextractor::operator().

### static void computeDescriptors(const Mat& image, vector<KeyPoint>& keypoints, Mat& descriptors, const vector<Point>& pattern)
*Func: 
Compute all descriptors with certain image.

*Input & Return:
```
Input image & keypoints. 

for (the size of keypoints)
computeOrbDescriptor(each "keypoint" in "image", with "&pattern[0]", store in pointer "descriptors.ptr()" );
```

*Use:
Caller1: ORBextractor::operator().

###  void ORBextractor::operator()( InputArray _image, InputArray _mask, vector<KeyPoint>& _keypoints, OutputArray _descriptors)

*Func: 
The operator in ORBextractor class.
Such as pre-computed the scale pyramid.
ComputePyramid(image) ->   ComputeKeyPointsOctTree(allKeypoints)

For each level, preprocessed the resized image -> compute the descriptors
-> Scale keypoint coordinates -> And add the keypoints to the output.

*Input & Return:
```
Input should be an array of image and mask, vector of keypoints.
Then return(output) the descriptor of keypoints of image that masked.
```
*Use:
Whoever used the ORBextractor class.

### void ORBextractor::ComputePyramid(cv::Mat image)
*Func: 
Compute the Image Pyramid of an image.

*Input & Return:
```
Simply input an image.
```

*Use:
Caller1: ORBextractor::operator().	

## ORBmatcher.cc

### ORBmatcher::ORBmatcher(float nnratio, bool checkOri): mfNNratio(nnratio), mbCheckOrientation(checkOri)
*Func: 
Constructor of ORB matcher.

*Input & Return:
```
Input ratio, orientation that should be checked.
```

*Use:
None.

### int ORBmatcher::SearchByProjection(Frame &F, const vector<MapPoint*> &vpMapPoints, const float th)
*Func: 
Note that there are **4 different types** of function of **SearchByProjection** in ORB matcher.

*Input & Return:
```
Give an example
```

*Use:
Caller1: Tracking::TrackWithMotionModel(), 
Caller2: Tracking::SearchLocalPoints(),
Caller3: Tracking::Relocalization().

###　float ORBmatcher::RadiusByViewingCos(const float &viewCos)
*Func: 
If viewing cosine is approximate 1, return 2.5, or return 4.0.

*Input & Return:
```
Input the viewing cosine value.
```

*Use:
Caller1: ORBmatcher::SearchByProjection.

### bool ORBmatcher::CheckDistEpipolarLine(const cv::KeyPoint &kp1,const cv::KeyPoint &kp2,const cv::Mat &F12,const KeyFrame* pKF2)
*Func: 
Check the distance of Epipolar Line.
Since there are some constraint on geometry.
Two feature point must fullfill the equation (x'T)Fx = 0, T represent the transpose.

*Input & Return:
```
Two featured point(keypoint) that obtained from the 2D projection of image plane.
F is the fundamental matrix. 	
```

*Use:
Caller1: ORBmatcher::SearchForTriangulation.

### int ORBmatcher::SearchByBoW(KeyFrame* pKF,Frame &F, vector<MapPoint*> &vpMapPointMatches)	
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::SearchByProjection(KeyFrame* pKF, cv::Mat Scw, const vector<MapPoint*> &vpPoints, vector<MapPoint*> &vpMatched, int th)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::SearchForInitialization(Frame &F1, Frame &F2, vector<cv::Point2f> &vbPrevMatched, vector<int> &vnMatches12, int windowSize)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::SearchByBoW(KeyFrame *pKF1, KeyFrame *pKF2, vector<MapPoint *> &vpMatches12)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::SearchForTriangulation(KeyFrame *pKF1, KeyFrame *pKF2, cv::Mat F12, vector<pair<size_t, size_t> > &vMatchedPairs, const bool bOnlyStereo)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::Fuse(KeyFrame *pKF, const vector<MapPoint *> &vpMapPoints, const float th)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::Fuse(KeyFrame *pKF, cv::Mat Scw, const vector<MapPoint *> &vpPoints, float th, vector<MapPoint *> &vpReplacePoint)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::SearchBySim3(KeyFrame *pKF1, KeyFrame *pKF2, vector<MapPoint*> &vpMatches12, const float &s12, const cv::Mat &R12, const cv::Mat &t12, const float th)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::SearchByProjection(Frame &CurrentFrame, const Frame &LastFrame, const float th, const bool bMono)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::SearchByProjection(Frame &CurrentFrame, KeyFrame *pKF, const set<MapPoint*> &sAlreadyFound, const float th , const int ORBdist)	
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### void ORBmatcher::ComputeThreeMaxima(vector<int>* histo, const int L, int &ind1, int &ind2, int &ind3)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

### int ORBmatcher::DescriptorDistance(const cv::Mat &a, const cv::Mat &b)
*Func: 
Functionality.

*Input & Return:
```
Give an example
```

*Use:
Caller1, Caller2.

## Concept of some technique

###FAST

### BRIEF

### Fundamental matrix

### Epipolar Geometry

### Triangulation
	

## Learning Website 
*https://www.csie.ntu.edu.tw/~cyy/courses/vfx/05spring/lectures/scribe/08scribe2.pdf
*https://www.csie.ntu.edu.tw/~cyy/courses/vfx/05spring/lectures/scribe/10scribe.pdf