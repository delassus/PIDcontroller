# PIDcontroller
This code implements a PID controller to steer an autonomous vehicle

The first challenge is to find appropriate parameters for Kp, Kd, and Ki.
The second challenge is to allow a smooth ride, avoiding as much as possible 
zig-zagging.

What happens when each of the constants is increased (see "An introduction and tutorial for PID controllers", by George Gillard).
 
Constant:   Rise time:     Overshoot:   Settling time:   Steady-state error:   Stability:
Kp          decrease       increase     Small change     decrease              degrade
Ki          decrease       increase     increase         decrease              degrade
Kd          minor change   decrease     decrease         No effect             Improve (if small enough)

Given the above observations, a method to tune the PID is implemented.
Method used to tuned the parameters:

(1) Set all three parameters Kp, Kd, Ki to zero.
(2) Increase Kp until the car reaches the first curve in the road.
(3) Increase Kd until the car passes the first curve in the road.
(4) Adjust Kp until the car passes the second curve in the road.
(5) Adjust Kd until the car passes the third curve in the road.
(6) Adjust Kp until the steepest curve is passed.
(7) Adjust Kd until oscillations ine the car trajectory are low frequency.
(8) Adjust Ki to keep off the side road.

At this stage the car drives around the circuit consistently. The trajectory is not smooth a low frequency oscillation occurs especially on straight road.

Low pass filtering of the error
To smooth the trajectory a low pass filter is implemeted on the cross track error before any processing.
The low pass filter chosen is an average of the error using the last ten errors samples.
This filter has a positive effect of smoothing the oscillation of the car trajectory.
It also has a detrimental effect of adding a delay in the system. Due to this delay the car cannot drive around the steepest curve.
To mitigate the delay, Kp and Kd have to be increased until the car drives around the circuit. 

Low pass filtering of the command
Finally, to smooth further the trajectory, a low pass filter is applied to the command. This filter is an average of the last three commands.
This filter will again add a delay in the system leading to more latency in the command.
To mitigate this detrimental effect, Kp and Kd have to be increased further.

Eventually, this gradual tuning process leads to an almost smooth trajectory.


