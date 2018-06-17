# Model Documentation

## Description of Model

For constructing the model I used the template implemented in the walktrough. The model consists of following parts:

* A cost function for lane change
* Determination of the nearest car in front
* Determination of safety of the lane
* Determination of the distance from the car in front of my car
* Lane change if the car is too close
* Slow down if lane change is not available
* Gradually increment speed if the car is not too close.
* Calculation of the waypoints for the car to follow
* Construction of a spline with the target points
* Using the spline for generating a curve and calculating the waypoints on the curve

The model starts with zero speed and the center lane (in which the car begins the simulation) as the target lane. The speed of the car is gradually incremented until the car reaches the maximum allowed speed or there is a car in front of my car within a certain range.  After that, a spline is constructed using the most recent points of the car and a forward target lane. Following that step, a series of waypoints are calculated with that spline and the cars speed.

With the use of spline, if the target lane is the same as the current lane, the car follows the lane. Otherwise, a lane change is panned implicitly. An explicit lane change is not necessary.

If there is a car ahead of my car within a certain distance the cars speed is gradually decremented and a lane change is evaluated. Lane change is evaluated for the nearby lanes with the defined cost function. If the cost functions of the nearby lanes is zero target lane is selected as current lane resulting only in a slowdown of the car.



## Cost Function
For the lane change logic I used a very simple cost function. The function chooses the line with the furthest car in front. The cost functions has two main parts.

The first part finds the nearest car ahead of my car in that lane. For all the cars in that lane the code finds the nearest car and uses its distance as measure..

The second part determines the safety of lane change action. The code here, checks if there is a car within a pre-determined margin. The return of this part is binary. If there is a car the function returns 0 else it returns 1.

Since safety is a _must have_ part, returns of the two parts of the cost function is multiplied. So, if switching to that lane is unsafe, the cost function 0 no matter the distance of the car in front.

## Choice of Parameters
I have a relatively older CPU, hence a larger latencies when processing. So I limited the use of previous points to 20 in order to avoid jerky lane changes.

For changing the lanes a margin of safety for 5 meters is used. A lane change is considered only safe if the car is more than 5 meters away from the car.


## Result

I have run the tests and saved a video of the results in my system.


[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/OSb4iJrATP0/0.jpg)](https://youtu.be/OSb4iJrATP0)

(Clicking on the image will take you to the youtube video.)


## Weak Points and Further Improvements
The most visible shortcoming of the algorithm is that it only evaluates lane change if the car in front is too close. This way, the car could get too close and not able to change lanes for a long while if lane change is unsafe. A better approach would be to evaluate lane change from a longer distance and avoid these situations.

Another improvement to avoid getting stuck would be considering slowing down and then passing the car, that is unsafe, to the other direction. This requires projecting one more step forward into the future and one more planning step.

A third, and smaller improvement would be is to add the cars speed to the safety checking function. When the car is at higher speed a smaller safety margin can be used.
