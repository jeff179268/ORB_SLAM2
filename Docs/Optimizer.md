# Document for Optimizer

## void Optimizer::GlobalBundleAdjustemnt(Map* pMap, int nIterations, bool* pbStopFlag, const unsigned long nLoopKF, const bool bRobust)
```
Input:
	pMap: Map class
    nIterations: num of iterations
    pbStopFlag: whether force stop optimizer
    nLoopKF: num of loop
    bRobust: whether robust mode
Output:
	None
```

Function:
	Get all KeyFrames and MapPoints and store in vector. Then, call BundleAdjustment() that corrects all KeyFrames and all MapPoints fixing the initial KeyFrame (the KeyFrame with id 0). Again to avoid a change in the surroundings of your current KeyFrame, fix the current KeyFrame instead of the initial KeyFrame.

## void Optimizer::BundleAdjustment(const vector &lt;KeyFrame *&gt; &amp;vpKFs, const vector &lt;MapPoint *&gt; &amp;vpMP, int nIterations, bool* pbStopFlag, const unsigned long nLoopKF, const bool bRobust)
```
Input:
	vpKFs: all key frames
    vpMP: all map points
    ......
Output:
	None
```

Function:

    1. Set KeyFrame vertices
    2. Set MapPoint vertices
    3. Optimize
    4. Set Keyframes and Points

## int Optimizer::PoseOptimization(Frame *pFrame)
```
Input:
	pFrame:
Output:

```

Function:

## void Optimizer::LocalBundleAdjustment(KeyFrame *pKF, bool* pbStopFlag, Map* pMap)
```
Input:
	pKF: current key frames
    pbStopFlag: whether force stop optimizer
    pMap: Map class
Output:
	None
```

Function:

## void Optimizer::OptimizeEssentialGraph(Map* pMap, KeyFrame* pLoopKF, KeyFrame* pCurKF, const LoopClosing::KeyFrameAndPose &NonCorrectedSim3, const LoopClosing::KeyFrameAndPose &CorrectedSim3, onst map&lt;KeyFrame *, set&lt;KeyFrame *&gt; &gt; &LoopConnections, const bool &bFixScale)
```
Input:

Output:
	None
```

Function:
	This is a pose graph optimization that distributes the error in the relative pose between the current KeyFrame and a loop closing KeyFrame (an old KeyFrame in your map). In this optimization, the loop closing KeyFrame is fixed, and the rest of KeyFrames are corrected. After the optimization, the MapPoints are corrected according to the corrections of their reference KeyFrames. That means that MapPoints seen by current KeyFrame (and very likely seen by your current camera) will be corrected, as the current KeyFrame pose will be corrected. A way to avoid a jump on the camera, is to fix the current KeyFrame instead of the loop closing KeyFrame. 

## int Optimizer::OptimizeSim3(KeyFrame *pKF1, KeyFrame *pKF2, vector&lt;MapPoint *&gt; &vpMatches1, g2o::Sim3 &g2oS12, const float th2, const bool bFixScale)
```
Input:

Output:

```

Function:



http://www.cnblogs.com/gaoxiang12/p/5304272.html
http://www.cnblogs.com/luyb/p/5447497.html