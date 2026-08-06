---
title:        'Pause and Resume'
description:  'Procedures that help pausing, aborting, resuming a print'
tag: pause

author: DerAndere1
contrib: DerAndere1
category: [ features, pause ]
---


<!-- # Introduction -->

Marlin includes several features related to aborting (stopping), pausing, resume, saving and recovering the position. 

# Commands for pause, resume and recovery

The following commands deal with pause, resume and recovery:

| G-Code command                  | Requirements            | Instant | Resume                  -| Description   |
|---------------------------------|-------------------------|---------|--------------------------|---------------|
| [`G4`](/docs/gcode/G4.html)     | None                    | No      | After dwell time expires | Dwell: Pause for a specified amount of time, keep heaters and power on. |Interruption of the pause is impossible. Use M0 instead if you want to stop until the user resumes or M226 to wait for pin state |
| [`G27`](/docs/gcode/G27.html)   | NOZZLE_PARK_FEATURE     | No      | No pause | Raise and park the nozzle according to a predefined XY position and Z raise (or minimum height) value. The preferred command M125 is equivalent to G60 S0 followed by G27, followed by G60 Q0.
| [`G60`](/docs/gcode/G60.html)   | SAVED_POSITIONS         | No      | No pause. Use G60 Q... to restore position | Save position. Use G60 Q... to restore position. This can be helpful for advanced programming and in the `*_EVENT_GCODE` and `*_SCRIPT_*` options in the Configuration.h and Configuration_adv.h files. To pause in case of problems and resume manufacturing, M413, or M25/M524 and M524 are preferred.
| [`G61`](/docs/gcode/G61.html)   | SAVED_POSITIONS         | No      | Resumes from G60     | Move to a saved position. The quivalent G60 Q0 is preferred. 
| [`M0`](/docs/gcode/M0.html)     | EMERGENCY_PARSER or LCD | No      | M108, Continue (LCD) | Unconditional stop until user resumes. Use M226 instead if you want to wait for a pin state. |
| [`M24`](/docs/gcode/M24.html)   | SDSUPPORT, POWER_LOSS_RECOVERY | No | Resumes from M25 or M524 | Start or resume a file selected with M23. | 
| [`M25`](/docs/gcode/M25.html)   | SDSUPPORT               | No      | M108, "Resume" (host prompt), Continue (LCD) |  Pause the SD print in progress. If PARK_HEAD_ON_PAUSE is enabled, save the file position and park the nozzle. 
| [`M76`](/docs/gcode/M76.html)   | None                    | No      | No pause    | Only pauses the print job timer, does not pause machining.
| [`M77`](/docs/gcode/M76.html)   | None                    | No      | No pause    | Only stops the print job timer, does not stop machining.
| [`M110`](/docs/gcode/M110.html) | None                    | No      | No pause    | Set and report line number. This can help identifying the section of G-code a G-code script in which a problem occured. |
| [`M112`](/docs/gcode/M112.html) | EMERGENCY_PARSER        | Yes with EMERGENCY_PARSER | Reset with M999     | Used for immediate halt, M112 shuts down the machine, turns off all the steppers and heaters, and if possible, turns off the power supply. A reset with M999 is required to return to operational mode. Use in case of an emergency. Resumption from the last position is not easily possible, but M110 and M114 can help. M524 is preferred if resumtion from the previous position is important. |
| [`M125`](/docs/gcode/M125.html) | PARK_HEAD_ON_PAUSE      | No      | M108, Continue (LCD) | Save the current nozzle position and move to the configured park position. Preferrably use M25 if you want to pause a print from storage media that was initiated with M24. |
| [`M226`](/docs/gcode/M26.html)  | DIRECT_PIN_CONTROL      | No      | Resume when a pin has a certain state | Wait for a pin to have a certain value or state. Use to synchronize with external hardware. |
| [`M400`](/docs/gcode/M400.html) | None                    | No      | No pause     | Finish moves. This command causes G-code processing to pause and wait in a loop until all moves in the planner are completed. Use at end of print or before commands that do not involve movement but that should be executed after all previous moves finished. |
| [`M410`](/docs/gcode/M410.html) | EMERGENCY_PARSER        | Yes with EMERGENCY_PARSER | Reset with M999 | Stop all steppers instantly. Since there will be no deceleration, expect steppers to be out of position after this command but M110 and M114 can help. Preferrably use P000 and R000 instead of M410. |
| [`M413`](/docs/gcode/M413.html) | SDSUPPORT, POWER_LOSS_RECOVERY, LCD | Yes | No pause. Resume via the LCD | Set and report line number. This can help identifying the section of G-code a G-code script in which a problem occured. |
| [`M524`](/docs/gcode/M524.html) | SDSUPPORT               | Yes     | No pause     |  Abort an SD print started with M24. |
| [`M999`](/docs/gcode/M999.html) | None                    | Yes     | Resumes from M112 or STOP |  Return the machine to Running state.  The default behavior is to flush the serial buffer and request a resend to the host starting on the last N line received (see M110). |
| [`P000`](https://github.com/MarlinFirmware/Marlin/blob/2b04e57f4d8fce7d0561839d8b868ff4992925fc/Marlin/Configuration_adv.h#L2867) | EMERGENCY_PARSER, REALTIME_REPORTING_COMMANDS | Yes     | Resume with R000 | Instant Pause / Hold. Enable SOFT_FEED_HOLD for soft deceleration. |
| [`R000`](https://github.com/MarlinFirmware/Marlin/blob/2b04e57f4d8fce7d0561839d8b868ff4992925fc/Marlin/Configuration_adv.h#L2868) | EMERGENCY_PARSER, REALTIME_REPORTING_COMMANDS | Yes     | Resmes from P000 | Resume from Pause / Hold. Enable SOFT_FEED_HOLD for soft acceleration. |
