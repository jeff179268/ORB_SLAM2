# Document for Loop Closing

## Functions

1. LoopClosing(Map* pMap, KeyFrameDatabase* pDB, ORBVocabulary* pVoc,const bool bFixScale)

+ Caller: 

+ DO: 

2. void SetTracker(Tracking* pTracker)

+ Caller: 

+ DO: 

3. void SetLocalMapper(LocalMapping* pLocalMapper)

+ Caller: 

+ DO: 

#### Main function

4. void Run()

5. void InsertKeyFrame(KeyFrame *pKF)

6. void RequestReset()

#### This function will run in a separate thread
7. void RunGlobalBundleAdjustment(unsigned long nLoopKF)

8. bool isRunningGBA(){
        unique_lock<std::mutex> lock(mMutexGBA);
        return mbRunningGBA;
    }
    
9. bool isFinishedGBA(){
        unique_lock<std::mutex> lock(mMutexGBA);
        return mbFinishedGBA;
    }   

10. void RequestFinish()

11. bool isFinished()

12. bool CheckNewKeyFrames()

13. bool DetectLoop()

14. bool ComputeSim3()

15. void SearchAndFuse(const KeyFrameAndPose &CorrectedPosesMap)

16. void CorrectLoop()

17. void ResetIfRequested()

18. bool CheckFinish()

19. void SetFinish()
