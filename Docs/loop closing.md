# Document for Loop Closing

## LoopClosing

```
Input:
	Map* pMap
        KeyFrameDatabase* pDB
        ORBVocabulary* pVoc
        const bool bFixScale
```

Function:

	INIT and set mnCovisibilityConsistencyTh = 3
        
Caller:

Created:

+ System

Used:

+ LocalMapping
+ Optimizer
+ Tracking

## void SetTracker

```
Input:
	Tracking* pTracker
```

Function:

	set local tracker

Caller:

+ System
+ LocalMapping

## void SetLocalMapper(LocalMapping* pLocalMapper)

```
Input:
	ORBVocabulary &voc
```

Function:

	set local mapper

Caller:

+ Tracking
+ System

#### Main function

## void Run()

```
Input:
	None
```

Function:

	Main function
	CheckNewKeyFrames 
        -> DetectLoop 
        -> ComputeSim3 
        -> CorrectLoop and listen to Reset or break

Caller:

+ System

## void InsertKeyFrame

```
Input:
	KeyFrame *pKF
```

Function:

	add KeyFrame to mlpLoopKeyFrameQueue

Caller:

+ Tracking

## void RequestReset()

```
Input:
	None
```

Function:

	Launch reset and set flag to enable ResetIfRequested()

Caller:

+ Tracking

## void RunGlobalBundleAdjustment

```
Input:
	unsigned long nLoopKF
```

Function:

	Run in a separate thread
        Update all MapPoints, KeyFrames and new keyframes(caused by Local Mapping) not included in the Global BA. 
        Propagate the correction through the spanning tree

Caller:

+ Tracking

## bool isRunningGBA(){
        unique_lock<std::mutex> lock(mMutexGBA);
        return mbRunningGBA;
    }

```
Input:
	None
```

Function:

	chcek(return) mbRunningGBA
        
Caller:
    
+ System

## bool isFinishedGBA(){
        unique_lock<std::mutex> lock(mMutexGBA);
        return mbFinishedGBA;
    }   

```
Input:
	None
```

Function:

	return mbFinishedGBA

Caller:

+ ???

## void RequestFinish

```
Input:
	None
```

Function:

	set mbFinishRequested = true to enable Finish Request
        
Caller:

+ System

## bool isFinished

```
Input:
	None
```

Function:

	return mbFinished

Caller:

+ System

## bool CheckNewKeyFrames()

```
Input:
	None
```

Function:

	return is mlpLoopKeyFrameQueue empty or not
        
Caller:

+ LoopClosing

## bool DetectLoop()

```
Input:
	None
```

Function:

	Avoid that a keyframe can be erased while it is being process by this thread
        Add Current KeyFrame if the map contains less than 10 KF or less than 10 KF have passed from last loop detection
        
        Compute reference BoW similarity score and loop candidates
        If there are no loop candidates, just add new keyframe and return false
        
        Detect a consistent loop in several consecutive keyframes to accept it
        If the group is not consistent with any previous group insert with consistency counter set to zero
        
        Update Covisibility Consistent Groups
        Add Current Keyframe to database        

Caller:

+ LoopClosing

## bool ComputeSim3()

```
Input:
	None
```

Function:
        
        Choose to accept Loop or not
        
	Compute a Sim3 for each consistent loop candidate 
        
        We compute first ORB matches for each candidate
        If enough matches are found, we setup a Sim3Solver
        
        Perform alternatively RANSAC iterations for each candidate until one is succesful or all fail
        > If Ransac reachs max. iterations discard keyframe
        > If RANSAC returns a Sim3, perform a guided matching and optimize with all correspondences
        
        Retrieve MapPoints seen in Loop Keyframe and neighbors
        Find more matches projecting with the computed Sim3
        
        If enough matches accept Loop

Caller:

+ LoopClosing

## void SearchAndFuse

```
Input:
	const KeyFrameAndPose &CorrectedPosesMap
```

Function:

	For each (KeyFrameAndPose &CorrectedPosesMap), use its next KeyFrameAndPose to do ORBmatcher.
        Fuse and check whether to Replace mvpLoopMapPoints or not

Caller:

+ LoopClosing

## void CorrectLoop

```
Input:
	None
```

Function:

	Send stop signal to Local Mapping
        Abort Global Bundle Adjustment if running
        Correct all MapPoints obsrved by current keyframe and neighbors
        Update keyframe pose with corrected Sim3, Loop Fusion
        Update matched map points and replace if duplicated
        Project MapPoints observed in the neighborhood of the loop keyframe into the current keyframe and neighbors using corrected poses.
        Fuse duplications
        Update connections and detect new links
        Optimize graph
        Launch a new thread to perform Global Bundle Adjustment

Caller:

+ LoopClosing

## void ResetIfRequested

```
Input:
	None
```

Function:

	Clear mlpLoopKeyFrameQueue and mLastLoopKFid

Caller:

+ LoopClosing

## bool CheckFinish

```
Input:
	None
```

Function:

	return mbFinishRequested

Caller:

+ LoopClosing

## void SetFinish

```
Input:
	None
```

Function:

	Set mbFinished = true

Caller:

+ LoopClosing 
