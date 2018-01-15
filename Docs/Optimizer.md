# Document for Optimizer

## void Optimizer::GlobalBundleAdjustemnt(Map* pMap, int nIterations, bool* pbStopFlag, const unsigned long nLoopKF, const bool bRobust)
```
Input:

Output:

```

Function:

## void Optimizer::BundleAdjustment(const vector &lt;KeyFrame *&gt; &amp;vpKFs, const vector &lt;MapPoint *&gt; &amp;vpMP, int nIterations, bool* pbStopFlag, const unsigned long nLoopKF, const bool bRobust)
```
Input:

Output:

```

Function:

## int Optimizer::PoseOptimization(Frame *pFrame)
```
Input:

Output:

```

Function:

## void Optimizer::LocalBundleAdjustment(KeyFrame *pKF, bool* pbStopFlag, Map* pMap)
```
Input:

Output:

```

Function:

## void Optimizer::OptimizeEssentialGraph(Map* pMap, KeyFrame* pLoopKF, KeyFrame* pCurKF, const LoopClosing::KeyFrameAndPose &NonCorrectedSim3, const LoopClosing::KeyFrameAndPose &CorrectedSim3, onst map&lt;KeyFrame *, set&lt;KeyFrame *&gt; &gt; &LoopConnections, const bool &bFixScale)
```
Input:

Output:

```

Function:

## int Optimizer::OptimizeSim3(KeyFrame *pKF1, KeyFrame *pKF2, vector&lt;MapPoint *&gt; &vpMatches1, g2o::Sim3 &g2oS12, const float th2, const bool bFixScale)
```
Input:

Output:

```

Function:

