Technology description 
Our task is to develop the control software for an automated retrieval system inside an automated 
warehouse. The system forwards totes (boxes) from storage bays to the output conveyor using a rail
guided cart. 

Each of the storage bays (A and B) are 
equipped with two conveyors, which can be 
driven individually. Totes are stored on the 
main conveyor, which – when turned on – 
transfer them to the output conveyor, which 
forwards the first tote onto the cart. A 
proximity switch is installed next to both 
output conveyors, which provides an active 
signal if a tote has reached the end of the 
output conveyor. Each storage bay is also 
equipped with a tower light to notify the 
operator if the main conveyor is empty, and a 
pushbutton, which is used to acknowledge 
the refill of the storage bay. 
The cart is moved along the track by a motor 
with direction set by a PLC output. A code 
track with black and white stripes is installed 
next to the track and is observed by an optical 
sensor on the cart. By counting the rising edges of its signal, the position of the cart can be determined 
(see incremental encoders). 
The cart is also equipped with a roller conveyor and a proximity switch next to it.  
Totes shall be transported to the output conveyor of the system (note that it is different from the output 
conveyors of the storage bay), which is located at the end of the track and is running constantly. At the 
position of the cart corresponding to the output conveyor (i.e. at the end of the track), a limit switch is 
installed, which is active if the cart is at the end of track.
Storage bays shall be operated the same way. When the system is turned on, output conveyors of the 
storage bay is empty. Then both conveyors (both the main and the output conveyor) shall be turned off 
and kept running until a tote arrives at the proximity switch of the bay. When the signal of the proximity 
switch changes to active, both conveyors shall be stopped immediately. 
When a tote shall be transferred from the storage bay and the cart has arrived, the output conveyor of 
the station shall be turned on (the main conveyor shall be kept off). As soon as the tote on the output 
conveyor leaves the proximity switch (i.e. its signal changes to inactive), the main conveyor shall be turned 
on and the two conveyors shall be kept running until an other tote reaches the proximity switch.
If no tote arrives at the proximity switch within 3 seconds after turning the main conveyor on, the storage 
bay is considered empty. Then both conveyors shall be switched off and the operator shall be notified by 
turning the tower light on. The operator acknowledges the refill of the storage bay by pressing the ACK 
button (this pushbutton needs to be protected against weighing down, i.e. you shall use the edge and not 
the level of its signal). After the acknowledgment the tower light shall be switched off and both conveyors 
shall be started and kept running until a tote reaches the proximity switch. 
The position of the cart is unknown when the system is initialized, so it shall moved to the limit switch by 
turning the motor on and setting the direction output to 0. When the limit switch is reached, the motor 
shall be immediately turned off. If the retrieval of a tote is requested, the cart shall be moved to the 
corresponding storage bay by turning the motor on and setting the direction output to 1. Position of the 
cart can be determined by counting the rising edges of the optical sensor. The counter shall be reset every 
time the cart reaches the limit switch next to the output conveyor (reference position). 
As the cart reaches a storage bay, the roller conveyor on it shall be turned on (output conveyor of the 
storage bay shall be turned on at the same time). When the signal of the proximity switch on the cart 
becomes active, the roller conveyor shall be turned off and the cart shall be moved to the output conveyor 
of the system by turning the motor on and setting the direction output to 0 (there is no need to count the 
edges of the optical sensor, the cart shall move until the limit switch turns active). 
When the cart reaches the end of the track, it shall be turned off and the roller conveyor on the cart shall 
be turned on. The conveyor shall be switched off 1 second after the signal of the cart’s proximity switch 
becomes inactive. The cart shall be kept at the limit switch until a new retrieval operation is requested. 
The control system communicates with the Warehouse Management System (WMS) using 3 digital and 1 
numeric channel. The WMS requests the retrieval of a tote by a rising edge on the request input of the 
PLC (%IX0.7). The SKU (identifier of the product stored in the totes) is available as an 8-bit unsigned 
integer at the input channel %IB1 of the PLC. Note that this value might vary during the operation, so the 
SKU shall be considered valid only when a rising edge of the request input is detected. 
The control system sends to signals towards the WMS. If the cart is at the limit position (at the output 
conveyor of the system) and has no tote on it (i.e. the conveyor of the cart is off), the ready output 
(%QX0.6) shall be active. The WMS sends a new request only while the ready output is active, i.e. rising 
edge of the input %IX0.7 can occur only if the value of output %QX0.6 is 1. 
Upon a request for retrieval, the control system can decide whether it can be fulfilled or not. It the SKU 
does not correspond to either of the storage bays or the corresponding bay is empty, the output 
corresponding to an invalid request (%QX0.7) shall be active until the next request (rising edge of 
%IX0.7). Otherwise, the output %QX0.7 shall be set to 0 and the cart shall be started towards the 
corresponding storage bay. Parameters of the storage bays are given in Table 1. 
Table 1 - Parameters of the storage bays 
Station 
Position (increment) 
SKUs stored 
A 
158 
2, 3, 4, 5, 6, 7 
B 
78 
39, 40, 41, 42, 43, 44, 45
