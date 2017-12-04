### Model Documentation

---

**Path Planning**
Goals of this project are the following:
* The car is able to drive at least 4.32 miles without incident
* The car drives according to the speed limit.
* Max Acceleration and Jerk are not Exceeded.
* Car does not have collisions
* The car stays in its lane, except for the time between changing lanes.
* The car is able to change lanes
* Implementation writeup


[//]: # (Image References)
[image1]: ./examples/p1.png
[image2]: ./examples/p2.png


## [Rubric](https://review.udacity.com/#!/rubrics/1020/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### The code model for generating paths is described in detail. This can be part of the README or a separate doc labeled "Model Documentation".  

You're reading it!


Please check [main.cpp](https://github.com/sandiptambde/CarND-Path-Planning-Project/blob/master/src/main.cpp) for the implemenation.

There are two parts of the solution

1. Trajectory Generation
- I have used 'Project Walkthrough and Q&A' by Aaron & David as reference for this implementation



2. Behavior Planning
My implemenation works as below
- My car starts in center lane with velocity 1mph

```
line 206:	
			int lane = 1;
			// Have reference velocity to target
			double ref_vel = 1; //mph
```

- Cars speed will then gradually increase if there is no vehicle in-front upto 49.5mph.
```
line 423:	
			else if (ref_vel < 49.5)

			{

			ref_vel += 0.224;
	
			}
``` 

- Car will stay in the same lane as long as there is no front vehicle close it(30m).
  To detect the vehicle, we compare S-distance of each car to our car in the same lane 
```
line 274:
			if ((check_car_s > car_s) && ((check_car_s - car_s) < 30))
			{

				too_close = true;

			}
```

- If vehicle is close then we prepare for lane change
To change the lane, we check 3 condition:
1. Vehicle infront is very close(30m) 
2. ref_vel is less than 36 mph (works well with this value based on trial & error)
3. There is no Car in another lane which is less than 30m to our car in S-Coordinate system 


Then we change lane as -
```
If lane == left 
	Change to center

If lane == right
   chage to center

If lane == center
	check first left lane is available
		if yes
			change to left lane
		else
			check if right lane is available
				if yes
					change to right lane
else
	same lane

```


**Result**:
My Car is sucessfully able to drive 32miles with this implementation

1. 32miles:

![alt text][image2]


2. lane change:

![alt text][image1]


**Challenges and issues**:
1. If vehicle in-front suddenly applies break, sometimes accident occures
2. Take vehicles in back side into consideration

