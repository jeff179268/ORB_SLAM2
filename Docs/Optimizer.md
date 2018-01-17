# Document for Optimizer

## Tutorial
```
一、 為什麼要優化

因為攝像機標定（camera calibration）和追踪（tracking）的精度不夠。攝像機標定的誤差會體現在重建中（比如三角法重建時），而追踪的誤差則會體現在不同關鍵幀之間的位姿中，和重建中（單目）。誤差的不斷累積會導致後面幀的位姿離實際位姿越來越遠，最終會限制系統整體的精度

1.1 攝像機標定

單目SLAM文獻中一般假設攝像機標定的結果是準確的，並不考慮這個因素帶來的誤差（大概因為很多時候跑標準的數據集，認為攝像機標定的誤差是相似的）。然而對於一個產品，不同類型的傳感器對應的標定誤差並不相同，甚至有可能差異很大。因此，如果要評估整個系統的精度，這方面的誤差必須要考慮進去。

1.2 追踪

無論在單目、雙目還是RGBD中，追踪得到的位姿都是有誤差的。單目SLAM中，如果兩幀之間有足夠的對應點，那麼既可以直接得到兩幀之間的位姿（像初始化中那樣），也可以通過求解一個優化問題得到（如solvePnP）。由於單目中尺度的不確定性，還會引入尺度的誤差。由於tracking得到的總是相對位姿，前面某一幀的誤差會一直傳遞到後面去，導致tracking到最後位姿誤差有可能非常大。為了提高tracking的精度，可以1. 在局部和全局優化位姿；2. 利用閉環檢測（loop closure）來優化位姿。

二、 如何優化

2.1 優化的目標函數在SLAM問題中，常見的幾種約束條件為： 1.三維點到二維特徵的映射關係（通過投影矩陣）；2.位姿和位姿之間的變換關係（通過三維剛體變換）；3.二維特徵到二維特徵的匹配關係（通過F矩陣）；5.其它關係（比如單目中有相似變換關係）。如果我們能夠知道其中的某些關係是準確的，那麼可以在g2o中定義這樣的關係及其對應的殘差，通過不斷迭代優化位姿來逐步減小殘差和，從而達到優化位姿的目標。

2.2 局部優化

當新的關鍵幀加入到convisibility graph時，作者在關鍵幀附近進行一次局部優化。關聯的局部信息，參與Bundle Adjustment，但取值保持不變。優化的目標是讓投影誤差最小。

2.3 全局優化

在全局優化中，所有的關鍵幀（除了第一幀）和三維點都參與優化

2.4 閉環處的Sim3位姿優化

當檢測到閉環時，閉環連接的兩個關鍵幀的位姿需要通過Sim3優化（以使得其尺度一致）。優化求解兩幀之間的相似變換矩陣，使得二維對應點（feature）的投影誤差最小。


```

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
    Update all MapPoints and KeyFrames. Local Mapping was active during BA, that means that there might be new keyframes not included in the Global BA and they are not consistent with the updated map. Propagate the correction through the spanning tree

    1. Set KeyFrame vertices
    2. Set MapPoint vertices
    3. Optimize
    4. Set Keyframes and Points

## int Optimizer::PoseOptimization(Frame *pFrame)
```
Input:
	pFrame: now frame
Output:
    nInitialCorrespondences-nBad: all correspondences minus inliers
```

Function:
    1. Set Frame vertex
    2. Set MapPoint vertices
    3. Set Edges and Robust Kernel(Monocular, Stereo observation)
    4. Perform 4 optimizations, after each optimization, it will classify observation as inlier/outlier. At the next optimization, outliers are not included, but at the end they can be classified as inliers again.
    5. Recover optimized pose and return number of inliers

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
    該關鍵幀和鄰近關鍵幀，去除Outlier
    1. Local KeyFrames: First Breath Search from Current Keyframe
    2. Find local MapPoints seen in Local KeyFrames
    3. Fixed Keyframes. Keyframes that see Local MapPoints but that are not Local Keyframes
    4. Setup optimizer
    5. Set Local KeyFrame vertices
    6. Set Fixed KeyFrame vertices
    7. Set MapPoint vertices -> Set edges(Monocular, Stereo observation)
    8. Check inlier observations
    9. Optimize again without the outliers and check inlier observations again
    10. Recover optimized data(Keyframes and points)

## void Optimizer::OptimizeEssentialGraph(Map* pMap, KeyFrame* pLoopKF, KeyFrame* pCurKF, const LoopClosing::KeyFrameAndPose &NonCorrectedSim3, const LoopClosing::KeyFrameAndPose &CorrectedSim3, onst map&lt;KeyFrame *, set&lt;KeyFrame *&gt; &gt; &LoopConnections, const bool &bFixScale)
```
Input:
    pMap: Map class
    pLoopKF: key frames
    pCurKF: current key frames
    NonCorrectedSim3: for checking normal edges
    CorrectedSim3: for checking loop edges
    LoopConnections: loop edges
    bFixScale: set _fix_scale
Output:
	None
```

Function:
	This is a pose graph optimization that distributes the error in the relative pose between the current KeyFrame and a loop closing KeyFrame (an old KeyFrame in your map). In this optimization, the loop closing KeyFrame is fixed, and the rest of KeyFrames are corrected. After the optimization, the MapPoints are corrected according to the corrections of their reference KeyFrames. That means that MapPoints seen by current KeyFrame (and very likely seen by your current camera) will be corrected, as the current KeyFrame pose will be corrected. A way to avoid a jump on the camera, is to fix the current KeyFrame instead of the loop closing KeyFrame. 

## int Optimizer::OptimizeSim3(KeyFrame *pKF1, KeyFrame *pKF2, vector&lt;MapPoint *&gt; &vpMatches1, g2o::Sim3 &g2oS12, const float th2, const bool bFixScale)
```
Input:
    pKF1: first key frame
    pKF2: second key frame
    vpMatches1: match MapPoints
    g2oS12: g2o::Sim3 class
    th2: threshold
    bFixScale: set _fix_scale
Output:

```

Function:
    The step is similar to PoseOptimization but using Sim3 to optimize two key frame.

```
g2o全稱通用圖優化，是一個用來優化非線性誤差函數的c ++框架。如果閱讀了前幾篇graph slam tutorial的博客

http://www.cnblogs.com/gaoxiang12/p/5304272.html

http://www.cnblogs.com/luyb/p/5447497.html
```