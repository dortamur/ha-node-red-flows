# Subflows #

These are reusable subflows used by other flows & examples.

## StopTimer+ ##

This is a subflow reimplementation of the `stoptimer` node, using Delay, with better support for dynamically controlling timer duration (through node config, or input attributes).

TODO: Actual details of operation/use

## Lights Motion Control ##

This is a Home Assistant automation for automating lights turning on based on a Presence event, but also retains manual light 
control. Features include:
 * If a light is _manually_ changed to be on, presence events are ignored (the light will remain on)
 * If a light is _manually_ changed to be off, presence events are ignored for a number of seconds defined by the `manualCooldown` property
 * If lights are off, and not recently manually changed, then any presence event will turn on the lights.
 * Automated lights will remain on until a number of seconds defined by the `timeOffAfter` properties. Any new presence event will 
 reset this timer.
 * Light state transition times can be set through the `transitionOn` and `transitionOff` properties
 * Light brightness can be controlled through a special `config` input, allowing external factors to control default brightness. 
 The default is 100% brightness.
 * TODO: Node Parameter for default brightness

Inputs to this node are:
 * Light Device Home Assistant State Event - a light entity eg; `light.kitchen_light`. It can take multiple light inputs, 
 in which case, by default, the automation will switch all detected lights.
 * Presence Device Home Assistant State Event - a presence detection entity eg; `binary_sensor.kitchen_presence`
 * A config message, with topic starting with `config`, being an object with config properties. Used to set light brightness (percent value).

By default, the subflow switches on and off the lights internal to the subflow. If some other response is required, set 
`manualLight` to True in the node properties, which will instead output events on the first and second outputs for "on" and "off" 
events respectively. When `manualLight` is False, these outputs instead contain the output of the Home Assistant service call to 
change the light status.
