# Watering machine parameters

Watering machine is a device that control a watering of the garden. This particular machine provides sequential zones watering. This was made to avoid low watering pressure that can happens in case of the parallel mode zone watering.

This device is a control unit with outputs to the external valves - one per watering zone.

<!--![picture](https://cloud.dynacore.systems/apps/files_sharing/publicpreview/L8qHpaeza94Mbzw?x=800&y=685&a=true&file=main.png&scalingup=00 "") -->
![picture](https://i.imgur.com/G4pfpaF.png)


## Modes
The device is designed to work in a two primary modes.

**Normal watering mode**

Used for the regular day-by-day watering. Each zone watering time interval set by the particular zone settings.

**Service mode**

Used for the system blowing before the coservation.
Usually in this mode pump provides an air instead of water. In this case switching to the next zone should be done manually. Through the control button/external input instead of zones watering interval time settings.

According to this, some of button has a different functions depends on the current mode and device state.

## Device logical structure
* [PCB 3D view](#pcb-3d-view "PCB 3D view")
* [Inputs](#Inputs "Inputs")
    * [Power inputs](#power-inputs "Power inputs")
	    * [Main power input](#main-power-input "Main power input")
		    * [Internal low-voltage circuits powering](#internal-low-voltage-circuits-powering "Internal low-voltage circuits powering")
            * [Valves powering](#valves-powering "Valves powering")
        * [Batteries bay](#batteries-bay "Batteries bay")
    * [Logical inputs](#logical-inputs "Logical inputs")
	    * [Control buttons](#control-buttons "Control buttons")
	    * [24 hours internal timer](#24-hours-internal-timer "24 hours internal timer")
	    * [External inputs](#external-inputs)
	        * [External TTL-levels input](#external-ttl-levels-input "External TTL-levels input")
		    * [External rain sensor input](#external-rain-sensor-input "External rain sensor input")
* [Outputs](#outputs "Outputs")
	* [State indicators](#state-indicators "State indicators")
	* [Valves outputs](#valves-outputs "Valves outputs")
	* [Cascading output (not implemented yet)](#cascading-output "Cascading output (not implemented yet)")

## PCB 3D view

![picture](https://cloud.dynacore.systems/apps/files_sharing/publicpreview/zAkpRHqrjZDFHtE?x=1710&y=534&a=true&file=pcb_3d_top.png&scalingup=0&t=2 "")
	
## Inputs

### Power inputs

#### Main power input

This device can be powered by the 9-36 VAC / 500 mA+ power supply.
Power supply characteristics should be calculated by a next formula:

Power supply voltage = valve voltage. Accepted range: [9 VAC - 36 VAC].

Power supply minimal current = 500 mA (internal circuits) + valve maximal current. Accepterd range: [500 mA - 2 A].

##### Internal low-voltage circuits powering
To power input low-voltage circuits was used primary power supply through current rectification and stabilisation. There is +5 VDC powering used for the low-voltage TTL circuits.
* U = +5 VDC (+/- 5%)
* I <= 700 mA

##### Valves powering
Valves solenoids are powering directly from the [main power input](#main-power-input "Main power input") through triacs without any stabilization or transformation.

#### Batteries bay
Battery powering is consists of the battery bay & controller which control charge level/charge batteries. It powers only chips required to run internal time and store device 24 hours timer state. Because they should be invariant for the external power outages.
Ther was used battery bay for 2x18650 battries. Bacause each battery voltage is 3.8-4.2 VDC. And if they are connected sequetely then they provides ~8 VDC voltage. Which is enough to provide +5 VDC after the stabilization.

### Logical inputs

#### Control buttons
Control buttons should be used to set the current watering mode, zone watering time intervals and change the device state - start, stop watering, switch to the next zone manually.

##### Mode switch button

This button toggle normal watering mode / service mode.

##### Start/Stop/Next button

_In the normal watering mode_

In this mode Start/Stop/Next button works in a Start/Stop toggle mode. When device not in a progress state and rain sensor circuit is disonnected then it start watering. Otherwise if device is in progress then stop.

_In the serivce mode_

In this mode Start/Stop/Next button works in a Start->Next->Stop mode. When watering is stopped then this button starts it in case rain sensor circuit is disconnected. On the next push it switch watering zone to the next one. On push when the last zone is in progress it stops the watering.

##### Timer On/Off switch button

This button toggle [24 hours internal timer](#24 hours-internal-timer "24 hours internal timer") on/off.

##### Zone watering time interwal switch

Each zone watering time can be set with a 3 minutes percision.

There are 2 x 5-position sliding switches per zone. One for the rough setup and one for the fine setup. The total zone watering time interval is a sum of rough and fine values.

_Rough setup 5-position sliding switch_

Positions: [1] - 0 minutes, [2] - 15 minutes, [3] - 30 minutes, [4] - 45 minutes, [5] - 60 minutes

_Fine setup 5-position sliding switch_

Positions: [1] - 0 minutes, [2] - 3 minutes, [3] - 6 minutes, [4] - 9 minutes, [5] - 12 minutes

Example: If rough switch in a position [3] (30 minutes) and fine switch in a position [4] (9 minutes) then zone time interval is equal to the  30 minutes + 9 minutes = 39 minutes


#### 24 hours internal timer
24 hours internal timer is used to start watering each 24 hours with everyday recuring.

Timer On/Off button toggle timer function.

##### Watering start time setup
Watering start time could be setup only in normal watering model. This can be made through a manual start watering by the pressing a start/stop/next or by external TTL-levels input high signal. And after the 24 hours it will starts watering again if device is not in a progress state and rain sensor circuit is disconnected.

#### External inputs

##### External TTL-levels input
The external TTL-levels input behaviour is a same as [Start/Stop/Next button](#start/stop/next-button "Start/Stop/Next button") behaviour. Each high TTL level toggle watering state depends on the current mode and the rain sensor circuit conection state.

##### External rain sensor input
It can be used to connect external rain sensor. Rain sensor shold connect both connector pins on rain begins and disconnect on rain ends. When pins are connected then watering is blocking with a maximal priohttps://lh3.googleusercontent.com/proxy/UT-IOb_x4KlqyPp-zGWHQO0mF21MnzdFfucUCoKksH5z4R97lfu_s4vADvaQHDftPSTWcJwHzlrEs6CZ2Ug1a_u0tiFyEd62Ig8tiyE5xuh8IShrMpPpzD3dQ1ai8TikYl29j_DfLLTafLSWqNq6JWqDb4vaO4U0q_7ACtbsgnMrity - no one signal can start watering.
If it connects pins when watering was is in progress then watering stops. And starts only when next start signal comes in case if rain sensor connector pins will be disconnected. 

## Outputs

### State indicators
* Power indicator: 
    * Off if device powered off
    * On if device is powered on
* Mode indicator: 
    * Off if device is in normal watering mode
    * On if device is in service mode
* In progress indicator:
    * Off if device in suspend mode (waiting for the start watering signal)
    * On if watering is in progress
* Zone watering in progress indicator. Each zone has own it's one:
    * Off if a current zone watering not is in progress state
    * On if a current zone watering is in progress state
* Rain indicator:
    * Off if rain sensor circuit is disconnected
    * On if rain sensor circuit is connected
* Change battery indicator:
    * Off if battery in a good state
    * Flashes if battery is corrupted and need replacement

### Valves outputs
There are ouptuts with [valves powering](#valves-powering "Valves powering"). At the moment only one output is active, according to the current watering zone.


### Cascading output
On last zone watering finish there is raises short high-level TTL impulse to cascade next watering device in watering devices sequence.
