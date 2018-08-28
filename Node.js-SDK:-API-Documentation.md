## Importing the SDK

If you downloaded the zip file with the entire project, you can create a file on the unzipped folder and import the SDK with the relative path:

```javascript
const CommunitySDK = require('./communitysdk')
```

If you are using the SDK as a module you can import it just as a normal node module:

```javascript
const CommunitySDK = require('communitysdk')
```

All the examples assume you are using the SDK as a node module.

## Available Classes

The Node.js SDK offers access to 3 classes:

- `DeviceManager`: List all the Kano devices currently connected to the computer over USB serial
- `MotionSensorKit`: Offers methods to send and receive data to a Motion Sensor Kit
- `RetailPixelKit`: Offers methods to send and receive data to a "retail" Pixel Kit


## `DeviceManager` Class

### `listConnectedDevices()`

Static method that returns all the devices connected to the computer over USB serial.

Returns a promise that resolves with an array of devices. Each item of this array is an instance of correspondent device already connected.

- Returns: `<Promise>`

Example:

```javascipt
const DeviceManager = require('communitysdk').DeviceManager;
DeviceManager.listConnectedDevices
.then((devices) => {
    // Do something with the `devices` array
})
.catch((error) => {
    // Couldn't list connected devices
});
```

## `MotionSensorKit` Class

### `new MotionSensorKit(options)`

Create an instance of `MotionSensorKit` class.

- `options` `<Object>`: Initialization options
    - `options.path` `<string>`: USB serial port path
- Returns: `<MotionSensorKit>`

Example:

```javascript
const MotionSensorKit = require('communitysdk').MotionSensorKit;
let msk = new MotionSensor({ path: '/dev/tty.pathtoyourmotionsensorkithere' });
```

### `msk.setMode(mode)`

Switch Motion Sensor Kit modes. The available modes are `proximity` and `gesture`.

- `mode` `<string>`: Which mode to set. Must be `proximity` or `gesture`
- Returns: `<Promise>`

Example:

```javascipt
msk.setMode('gesture')
.then(() => {
   // Motion Sensor successfully switched to gesture mode
})
.catch((error) => {
   // Motion Sensor couldn't switch to gesture mode
})
```

### `msk.setInterval(interval)`

Set how often the Motion Sensor Kit will poll for proximity data.

- `interval` `<integer>`: Interval in milliseconds between data polling.
- Returns: `<Promise>`

Example:

```javascipt
msk.setMode('gesture')
.then(() => {
   // Motion Sensor successfully switched to gesture mode
})
.catch((error) => {
   // Motion Sensor couldn't switch to gesture mode
})
```

### Event: `proximity`

Triggered when Motion Sensor Kit gets proximity data. It keeps sending data if value is `0` but doesn't if value is `255`

- `proximityValue` `<integer>`: Value of proximity data. Range from `0` to `255`

Example:

```javascript
msk.on('proximity', (proximityValue) => {
    console.log(proximityValue);
})
```

### Event: `gesture`

Triggered when Motion Sensor Kit gets gesture data. It only triggers the event when a gesture is recognized by the sensor.

- `gestureValue` `<string>`: Name of gesture detected by Motion Sensor. Values are `up`, `down`, `left` or `right`

Example:

```javascript
msk.on('gesture', (gestureValue) => {
    console.log(gestureValue);
})
```

### Event: `error-message`

Triggered when Motion Sensor Kit gets gesture data. It only triggers the event when a gesture is recognized by the sensor.

- `errorMessage` `<string>`: Message describing an error sent through RPC. 

Example:

```javascript
msk.on('erro-message', (errorMessage) => {
    console.log(errorMessage);
})
```
