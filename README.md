# MMM-NewPIR
MMM-NewPIR is a module for the [MagicMirror](https://github.com/MichMich/MagicMirror) project by [Michael Teeuw](https://github.com/MichMich).

It uses a PIR sensor attached to your raspberry pi's GPIO pins to check for users. After a configurated time without any user interaction the display will turn off and hide all module for economy mode.

If you don't have PIR sensor, it can also be used for automatic turn on / turn off screen.

## What's new on V3 ?
 * Rewrite entire main code
 * New configuration
 * Use my own shared npm library
 * Add new display style
 * More tools: incomming notification for developers
 * New support for turn off screen !
 * New scripts for updating
 * Touch Screen support

## Screenshot
![](https://raw.githubusercontent.com/bugsounet/MMM-NewPIR/master/screenshoot.png)

![](https://raw.githubusercontent.com/bugsounet/MMM-NewPIR/master/screenshot_2.png)

## Installation
Needed: MagicMirror v2.13.0 and above

Clone the module into your MagicMirror module folder and execute `npm intall` in the module's directory.
```
cd ~/MagicMirror/modules
git clone https://github.com/bugsounet/MMM-NewPIR.git
cd MMM-NewPIR
npm install
```

This module will verify if all screen saver is disabled and disable it if needed

## Configuration
To display the module insert it in the config.js file. Here is an example:

## Minimal configuration
```js
{
  module: 'MMM-NewPIR',
  position: 'top_left',
  configDeepMerge: true,
  config: {
      screen: {
        delay: 2 * 60 * 1000
      },
      pir: {
        gpio: 21
      }, 
  }
},
```
## Personalized configuration
this is the default configuration defined if you don't define any value

```js
{
  module: 'MMM-NewPIR',
  position: 'top_left',
  configDeepMerge: true,
  config: {
      debug: false,
      screen: {
        delay: 2 * 60 * 1000,
        turnOffDisplay: true,
        mode: 1,
        ecoMode: true,
        displayCounter: true,
        text: "Auto Turn Off Screen:",
        displayBar: true,
        displayStyle: "Text",
        governorSleeping: false,
        displayLastPresence: true,
        LastPresenceText: "Last Presence:",
        useTouch: false
      },
      pir: {
        usePir: true,
        gpio: 21,
        reverseValue: false
      },
      governor: {
        useGovernor: false,
        sleeping: "powersave",
        working: "ondemand"
      }
  }
},
```

### Field `screen: {}`

| Option  | Description | Type | Default |
| ------- | --- | --- | --- |
| delay | Time before the mirror turns off the display if no user activity is detected. (in ms) | Number | 120000 |
| turnOffDisplay | Should the display turn off after timeout? | Boolean | true |
| mode | mode for turn on/off your screen (see bellow) | number | 1 |
| ecoMode | Should the MagicMirror hide all module after timeout ? | Boolean | true |
| displayCounter | Should display Count-down in screen ? | Boolean | true |
| text | Display a text near the counter | String | "Auto Turn Off Screen:" |
| displayBar| Should display Count-up bar in screen ? | Boolean | true |
| displayStyle| Style of the Count-down. Available: "Text", "Line", "SemiCircle", "Circle", "Bar" | String | Text |
| governorSleeping| Activate sleeping governor when screen is off | Boolean | false |
| displayLastPresence| Display the date of the last user presence | Boolean | true |
| LastPresenceText| Display this text near the last presence date | Text | Last Presence: |
| useTouch| Allow to use TouchScreen or mouse (see TouchScreen Field below) | Boolean | false |

 * Available mode:
   - `mode: 1` - use vgencmd (RPI only)
   - `mode: 2` - use dpms (version RPI)
   - `mode: 3` - use tvservice (RPI only)
   - `mode: 4` - use HDMI CEC
   - `mode: 5` - use dpms (linux version for debian, ubuntu, ...)

Note: the mode 0 disable turnOffDisplay too

### Field `pir: {}`
| Option  | Description | Type | Default |
| ------- | --- | --- | --- |
| usePir | activation of Pir sensor module | Boolean | true |
| gpio | BCM-number of the sensor pin | Number | 21 |
| reverseValue | presence detector value | Boolean | false |

### Field `governor: {}`
| Option  | Description | Type | Default |
| ------- | --- | --- | --- |
| useGovernor | When you set to true, you enable governor management | Boolean | true |
| sleeping | name of the governor when screen is in sleeping state | String | powersave |
| working | name of the governor when screen is actived | String | ondemand |

Available governor:
 * conservative
 * ondemand
 * userspace
 * powersave
 * performance
 
Notes: On boot of your RPI, your governor is reset automaticaly to ondemand

## Developer Notes

- This module broadcasts:
  * `USER_PRESENCE` notification with the payload beeing `true` or `false` you can use it to pause or disable another module.
- This module receive:
  * `USER_PRESENCE` notification with the payload `true` to force user presence or `false` to force delay to time out. 
  * `SCREEN_END` notification to force the end of the count down
  * `SCREEN_WAKEUP` notification to wake up the screen and reset count down
  * `SCREEN_LOCK` notification keep the screen on and lock it (freeze counter and stop pir detection) 
  * `SCREEN_UNLOCK` notification unlock the screen and restart counter and pir detection

## Update
```
cd ~/MagicMirror/modules/MMM-NewPIR
git pull
npm run update
```

## TouchScreen Feature
You can use this feature with a mouse or your touch screen

 * One Click on the NewPIR Area: will restart your shutdown timer
 * Long Click on the screen: Will turn off your screen or Will turn on if needed (Toogle)
 
## Donate
 [Donate](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=TTHRH94Y4KL36&source=url), if you love this module !

## Change Log

### 2020-12-29
- Review Touch Screen Feature

### 2020-12-28
- add TouchScreen Feature

### 2020-11-22
- add new supprt for turn on/off screen
- add npm checker
- add last presence
- del configMerge and take place to MM integred version

### 2020-08-01
- Add: config merge script

### 2020-07-28
- V3 initial commit
- Rewrite code
