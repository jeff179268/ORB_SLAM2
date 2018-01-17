# Document for ORBextractor
Records the functionality of ORB-realted .cc files.

## ORBextractor.cc
### static float IC_Angle(const Mat& image, Point2f pt,  const vector<int> & u_max)
* Func: 
Compute angle

* Input & Return:
```
Input image and point to compute angle bt "fastAtan2"
```

* Use:
computeOrientation().

### static void computeOrbDescriptor(const KeyPoint& kpt,const Mat& img, const Point* pattern, uchar* desc)
* Func: 
Compute ORB descriptors of keypoint "kpt"

* Input & Return:
```
Input keypoint & image. 
After computing, store the descriptor value in pointer "desc".
```

* Use:
computeDescriptors().	

### ORBextractor::ORBextractor(int _nfeatures, float _scaleFactor, int _nlevels,int _iniThFAST, int _minThFAST):nfeatures(_nfeatures), scaleFactor(_scaleFactor), nlevels(_nlevels),iniThFAST(_iniThFAST), minThFAST(_minThFAST)

* Func: 
ORB extractor is the fusion of FAST detector& BRIEF descriptor.
The is the constructor of the class "ORBextractor".
It first set several common-used parameters, such as mvScaleFactor, mvLevelSigma2, mvImagePyramid, mnFeaturesPerLevel
under given input parameters.
Then  pre-compute the end of a row in a circular patch for orientation.


* Input & Return:
```
Constructor : nfeatures, scaleFactor, nlevels, iniThFAST, minThFAST.
```

* Use:
Whenever the use of class.

### static void computeOrientation(const Mat& image, vector<KeyPoint>& keypoints, const vector<int>& umax)
* Func: 
Compute Orientation by computing every keypoints angle.

* Input & Return:
```
Input image and keypoints.
```

* Use:
ORBextractor::ComputeKeyPointsOctTree(),  ORBextractor::ComputeKeyPointsOld.	

### void ExtractorNode::DivideNode(ExtractorNode &n1, ExtractorNode &n2, ExtractorNode &n3, ExtractorNode &n4)
* Func: 
Define boundaries of childs and associate points to childs.

* Input & Return:
```
Input 4 node that extracted.
```

* Use:
ORBextractor::DistributeOctTree().

### vector<cv::KeyPoint> ORBextractor::DistributeOctTree(const vector<cv::KeyPoint>& vToDistributeKeys, const int &minX, const int &maxX, const int &minY, const int &maxY, const int &N, const int &level)
* Func: 
Create a node list, after computing, there should attain the best point in each node.
Then the node list which contains the best point would be return.

* Input & Return:
```
Input the max & min of "X" and "Y" respectively, also with the input of level.
Return the result of Keys called "vResultKeys".
```

* Use:
ComputeKeyPointsOctTree().	

### void ORBextractor::ComputeKeyPointsOctTree(vector<vector<KeyPoint> >& allKeypoints)
* Func: 
First resized to meet the level. For each level, reserve size of 10 times features for DistributeKeys.
Then push the cell that detect by "FAST" to DistributeKeys, which named "vToDistributeKeys".
Each keypoints should have the size of numbers of "nfeatures".
Compute keypoints by "DistributeOctTree()".
After that we adding border to coordinates and scale information.
Before finishing this function, we'll compute the "orientation" for "all keypoints" by "Image Pyramid".

* Input & Return:
```
Simply input "all keypoints".
Setting values within the given pointer "&allKeypoints"
```

* Use:
ORBextractor::operator().

### void ORBextractor::ComputeKeyPointsOld(std::vector<std::vector<KeyPoint> > &allKeypoints)
* Func: 
For each level, define "featured cells". Which celled "key points".
Create cellImage with certain row, col range in each level. Reserve (5* numbers of features cell) for cellKeyPoints.
The compuing results using FAST.
After retain by score and transform coordinates, filter the best key points "cell", which should also meet the number of attained points.
Once chosen the cell, push every keypoints to "keypoints".
However, the number of keypoints in cell may not be same as we desired.
So filter "keypoints" to pick top n features that desired.

At the end of the function, compute orientations of all keypoints again with Image Pyramid.

* Input & Return:
```
Simply input "all keypoints".
Setting values within the given pointer "&allKeypoints"
```

* Use:
ORBextractor::operator().

### static void computeDescriptors(const Mat& image, vector<KeyPoint>& keypoints, Mat& descriptors, const vector<Point>& pattern)
* Func: 
Compute all descriptors with certain image.

* Input & Return:
```
Input image & keypoints. 

for (the size of keypoints)
computeOrbDescriptor(each "keypoint" in "image", with "&pattern[0]", store in pointer "descriptors.ptr()" );
```

* Use:
ORBextractor::operator().

###  void ORBextractor::operator()( InputArray _image, InputArray _mask, vector<KeyPoint>& _keypoints, OutputArray _descriptors)

* Func: 
The operator in ORBextractor class.
Such as pre-computed the scale pyramid.
ComputePyramid(image) ->   ComputeKeyPointsOctTree(allKeypoints)
For each level, preprocessed the resized image -> compute the descriptors
-> Scale keypoint coordinates -> And add the keypoints to the output.

* Input & Return:
```
Input should be an array of image and mask, vector of keypoints.
Then return(output) the descriptor of keypoints of image that masked.
```
* Use:
Whoever used the ORBextractor class.

### void ORBextractor::ComputePyramid(cv::Mat image)
*Func: 
Compute the Image Pyramid of an image.

* Input & Return:
```
Simply input an image.
```

* Use:
ORBextractor::operator().	