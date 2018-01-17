# Document for keyframeDatabase 

## KeyFrameDatabase

```
Input:
	ORBVocabulary &voc
```

Function:

	Class object init
	Use ORBVocabulary to init(set shape) a KeyFrame list mvInvertedFile

Caller:

Create in:
+ System
	
Used in:
+ KeyFrame
+ LoopClosing
+ Tracking
	
## void add

```
Input:
	KeyFrame* pKF
```

Function:
	
	Push KeyFrame to KeyFrame list mvInvertedFile
	
Caller:

+ KeyFrame
+ LoopClosing
+ ORBextractor
+ ORBmatcher

## void erase

```
Input:
	KeyFrame* pKF
```

Function:
	
	Erase elements in the Inverse File for the entry KeyFrame
	
Caller:

+ KeyFrame
+ LoopClosing
+ ORBextractor
+ ORBmatcher

## void clear

```
Input:
	None
```

Function:

	Clean up mvInvertedFile

Caller:

Caller:

+ KeyFrame
+ LoopClosing
+ ORBextractor
+ ORBmatcher

   // Loop Detection
## std::vector<KeyFrame *> DetectLoopCandidates

```
Input:
	KeyFrame* pKF, float minScore
Output:
	vector<KeyFrame *>
```

Function:

	Loop Detection
	Search all keyframes that share a word(mnLoopWords) with current keyframes
	Discard keyframes connected to the query keyframe
	Only compare against those keyframes that share enough words
	Compute similarity score. Retain the matches whose score is higher than minScore(0.75*bestScore)

Caller:

+ LoopClosing

## std::vector<KeyFrame*> DetectRelocalizationCandidates

```
Input:
	Frame* F
Output:
	vector<KeyFrame *>
	
```

Function:

	Relocalization
	Search all keyframes that share a word(mnRelocWords) with current frame
	Only compare against those keyframes that share enough words
	Compute similarity score
	Return all those keyframes with a score higher than 0.75*bestScore
	
Caller:

+ Tracking
