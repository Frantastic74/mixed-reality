---
title: Eye Tracking
description: Eye Tracking
author: sostel
ms.author: sostel
ms.date: 04/05/2019
ms.topic: article
keywords: Eye Tracking, Mixed Reality, Input, Eye Gaze
---
# Eye Tracking on HoloLens 2

> More information coming soon!

HoloLens 2 enables detecting what a user is looking at. Eye tracking enables applications to track where the user is looking in real time.  This is the main capability developers can leverage to enable a whole new level of context and human understanding within the Holographic experience. The eye tracking algorithm provides the following to applications through our API: 

1.	Timestamp, 
2.	Eye gaze origin (only one combined for left and right eye), 
3.	Eye gaze direction (again this is combined for left and right eye), 
4.	Boolean whether the gaze is valid. 

For eye tracking to work accurately, each user is required to go through an Eye Tracking user calibration. 


## Eye Tracking Calibration

> More information coming soon!

## Eye Tracking API
The _Eye Tracking API_ will be accessible through: `Windows.Perception.People.EyesPose` and will include the following:

| Variable | Data Type |  Description |
|---|---|---|
| LastUpdateTimestamp | `DateTimeOffset` | Returns last timestamp when the ET signal was updated. | 
| Gaze | `Windows.Perception.Spatial.SpatialRay?` | Ray representing the gaze based on a combination of left and right eye. |  
| Gaze.Origin  | `System.Numerics.Vector3`  | Single eye gaze origin. |  
| Gaze.Direction  | `System.Numerics.Vector3`  | Single eye gaze direction. | 
| IsCalibrationValid  | `bool`  | Indicates whether the user is calibrated. |  


## Eye Gaze Design Guidelines
Building an interaction that takes advantage of fast moving eye targeting can be challenging. 
In this section, we summarize the key advantages and challenges to take into account when designing your app. 

### Benefits of Eye Gaze Input

- **High speed pointing.** 
The eye muscle is the fastest reacting muscle in our body. 

- **Low effort.** 
Barely any physical movements are necessary. 

- **Implicitness.** 
Often described by users as "mind reading", information about a user's eye movements lets the system know which target the user plans to engage with. 

- **Alternative input channel.** 
Eye gaze can provide a powerful supporting input for hand and voice input building on years of experience from users based on their hand-eye coordination.

- **Visual attention.** 
Another important benefit is the possibility to infer what a user's is paying attention to. 
This can help in various application areas ranging from more effectively evaluating different designs to aiding in smarter User Interfaces and enhanced social cues for remote communication.

In a nutshell, using eye gaze as an input potentially offers a fast and effortless contextual signal - This is particularly powerful in combination with other inputs such as *voice* and *manual* input to confirm the user's intent.


### Challenges of Eye Gaze Input
With lots of power, comes lots of responsibility: 
While eye gaze can be used to create magical user experiences feeling like a superhero, it is also important to know what it is not good at to account for this appropriately. 
In the following, we discuss some *challenges* to take into account and how to address them when working with eye gaze input: 

- **Your eye gaze is "always on"** 
The moment you open your eye lids, your eyes start fixating things in your environment. 
Reacting to every look you make and potentially accidentally issuing actions because you looked at something for too long would result in a terrible experience!
This is why we recommend combining eye gaze with a *voice command*, *hand gesture*, *button click* or extended dwell to trigger the selection of a target.
This solution also allows for a mode in which the user can freely look around without the overwhelming feeling of involuntarily triggering something. 
This issue should also be taken into account when designing visual and auditory feedback when merely looking at a target.
Do not overwhelm the user with immediate pop-out effects or hover sounds. 
Subtlety is key! 
We will discuss some best practices for this further below when talking about design recommendations.

- **Observation vs. Control** 
Imagine you want to precisely align a photograph at your wall. 
You look at its borders and its surroundings to see if it aligns well. 
Now imagine how you would do that when at the same time you want to use your eye gaze as an input to move the picture. 
Difficult, isn't it? 
This describes the double role of eye gaze when it is required both for input and control. 

- **Leave before Click:** 
For quick target selections, research has shown that a user's eye gaze may move on before concluding a manual click (e.g., an airtap). 
Hence, special attention must be paid to synchronizing the fast eye gaze signal with slower control input (e.g., voice, hands, controller).

- **Small targets:**
Do you know the feeling when you try to read text that is just a bit too small to comfortably read? 
This straining feeling on your eyes that cause you to feel tired and worn out because you try to readjust your eyes to focus better?
This is a feeling you may invoke in your users when forcing them to select too small targets in your app using eye targeting.
For your design, to create a pleasant and comfortable experience for your users, we recommend that targets should be at least 2° in visual angle, preferably larger.

- **Ragged Eye Gaze Movements** 
Our eyes perform rapid movements from fixation to fixation. 
If you look at scan paths of recorded eye movements, you can see that they look ragged. 
Your eyes move quickly and in spontaneous jumps in comparison to *head gaze* or *hand motions*.  

- **Tracking Reliability:**
Eye tracking accuracy may degrade a little in changing light as your eye adjust to the new conditions.
While this should not necessarily affect your app design, as the accuracy should be within the above mentioned limitation of 2°. 
It may mean that the user has to run another calibration. 


### Design Recommendations
In the following, we list specific design recommendations based on the described advantages and challenges for eye gaze input:

1. **Eye Gaze != Head gaze:**
    - **Consider whether fast yet ragged eye movements fit your input task:** 
While our fast and ragged eye movements are great to quickly select targets across our Field of View, it is less applicable for tasks that require smooth input trajectories (e.g., for drawing or encircling annotations). 
In this case, hand or head pointing should be preferred.
  
    - **Avoid attaching something directly to the user’s eye gaze (e.g., a slider or cursor).**
In case of a cursor, this may result in the “fleeing cursor” effect due to slight offsets in the projected eye gaze signal. 
In case of a slider, it conflicts with the double role of controlling the slider with your eyes while also wanting to check whether the object is at the correct location. 
In a nutshell, users may quickly feel overwhelmed and distracted, especially if the signal is imprecise for that user. 
  
2. **Combine eye gaze with other inputs:** 
The integration of Eye Tracking with other inputs, such as hand gestures, voice commands or button presses, serves several advantages:
    - **Allow for free observation:** 
    Given that the main role of our eyes is to observe our environment, it is important to allow users to look around without triggering any (visual, auditory, ...) feedback or actions. 
    Combining ET with another input control allows for smoothly transitioning between ET observation and input control modes.
  
    - **Powerful context provider:** 
Using information about where the user is looking at while uttering a voice command or performing a hand gesture allows for effortlessly channeling the input across the field-of-view. Examples include: “Put that there” to quickly and fluently select and position a hologram across the scene by simply looking at a target and destination. 

    - **Need for synchronizing multimodal inputs (“leave before click” issue):** 
    Combining rapid eye movements with more complex additional inputs (e.g., long voice commands or hand gestures) bears the risk of moving on with your eye gaze before finishing the additional input command. 
    Hence, if you create your own input controls (e.g., custom hand gestures), make sure to log the onset of this input or approximate duration to correlate it with what a user had fixated on in the past.
    
3. **Subtle feedback for ET input:**
It is useful to provide feedback if a target is looked at (to indicate that the system is working as intended) but should be kept subtle. 
This may include slowly blending in/out visual highlights or perform other subtle target behaviors, such as slow motions (e.g., slightly increasing the target) to indicate that the system correctly detected that the user is looking at a target, however, without unnecessarily interrupting the user’s current workflow. 

4. **Avoid enforcing unnatural eye movements as input:** 
Do not force users to perform specific eye movements (gaze gestures) to trigger actions in your app.

5. **Account for imprecisions:** 
We distinguish two types of imprecisions which are noticeable to users: Offset and Jitter. The easiest way to address offsets is to provide sufficiently large targets to interact with (> 2° in visual angle – as reference: your thumbnail is about 2° in visual angle when you stretch out your arm (1)). This leads to the following guidance:
    - Do not force users to select tiny targets: Research has shown that if targets are sufficiently large (and the system is designed well), users describe the interaction as effortless and magical. If targets become too small, users describe the experience as fatiguing and frustrating.


## Use cases
Eye tracking enables applications to track where the user is looking in real time. 
This will enable a whole new level of context and human understanding within your holographic experience. 
This section describes the types of new interactions that become possible with eye tracking.

### User intent	    
-	Context for other inputs such as voice, hands and controllers
-	Fast and low-effort target selections
-	Engagement with embodied virtual agents and interactive holograms	

### Attention tracking	 
-	Remote eye gaze visualization
-	User research studies 
-	Training simulations
-	Performance monitoring
-	Design evaluations, advertisement and marketing research
-	Medical and educational research / applications	

### Implicit actions	 
-	Automatic scroll and pan
-	Smart notifications
-	Attentive holograms that react when being looked at	

### Expressiveness	 
-	Live avatar eye animation
-	Gaze direction and blinks	

### Gaming 	 
-	Additional targeting vector
-	Superpowers! 	

### Text entry 	 
-	Smarter and low-effort text entry (especially useful when speech or hands are inconvenient to use)	

---
