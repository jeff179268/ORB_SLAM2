# Document for Loop Closing

## Functions

1. LoopClosing(Map* pMap, KeyFrameDatabase* pDB, ORBVocabulary* pVoc,const bool bFixScale)

+ Caller: 

+ DO: INIT and set mnCovisibilityConsistencyTh = 3

2. void SetTracker(Tracking* pTracker)

+ Caller: 

+ DO: set local tracker

3. void SetLocalMapper(LocalMapping* pLocalMapper)

+ Caller: 

+ DO: set local mapper

#### Main function

4. void Run()

+ Caller: 

+ DO: CheckNewKeyFrames -> DetectLoop -> ComputeSim3 -> CorrectLoop and listen to Reset or break

5. void InsertKeyFrame(KeyFrame *pKF)

+ Caller: 

+ DO: add KeyFrame to mlpLoopKeyFrameQueue

6. void RequestReset()

+ Caller: 

+ DO: Launch reset and set flag to enable ResetIfRequested()

#### This function will run in a separate thread
7. void RunGlobalBundleAdjustment(unsigned long nLoopKF)

+ Caller: 

+ DO: Update all MapPoints , KeyFrames and new keyframes(caused by Local Mapping) not included in the Global BA. Propagate the correction through the spanning tree

8. bool isRunningGBA(){
        unique_lock<std::mutex> lock(mMutexGBA);
        return mbRunningGBA;
    }
    
+ Caller: 

+ DO: return mbRunningGBA
    
9. bool isFinishedGBA(){
        unique_lock<std::mutex> lock(mMutexGBA);
        return mbFinishedGBA;
    }   

+ Caller: 

+ DO: return mbFinishedGBA

10. void RequestFinish()

+ Caller: 

+ DO: set mbFinishRequested = true

11. bool isFinished()

+ Caller: 

+ DO: return mbFinished

12. bool CheckNewKeyFrames()

+ Caller: 

+ DO: 

13. bool DetectLoop()

+ Caller: 

+ DO: 

14. bool ComputeSim3()

+ Caller: 

+ DO: 

15. void SearchAndFuse(const KeyFrameAndPose &CorrectedPosesMap)

+ Caller: 

+ DO: For each (KeyFrameAndPose &CorrectedPosesMap), use its next KeyFrameAndPose to do ORBmatcher.fuse and check whether to Replace mvpLoopMapPoints or not

16. void CorrectLoop()

+ Caller: 

+ DO: 
        + Send stop signal to Local Mapping
        + abort Global Bundle Adjustment if running
        + Correct all MapPoints obsrved by current keyframe and neighbors
        + Update keyframe pose with corrected Sim3, Loop Fusion
        + Update matched map points and replace if duplicated
        + Project MapPoints observed in the neighborhood of the loop keyframe into the current keyframe and neighbors using corrected poses.
        + Fuse duplications
        + Update connections and detect new links
        + Optimize graph
        + Launch a new thread to perform Global Bundle Adjustment

17. void ResetIfRequested()

+ Caller: 

+ DO: clean mlpLoopKeyFrameQueue and mLastLoopKFid

18. bool CheckFinish()

+ Caller: 

+ DO: return mbFinishRequested

19. void SetFinish()

+ Caller: 

+ DO: mbFinished = true
