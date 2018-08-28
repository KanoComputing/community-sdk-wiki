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

## `RetailPixelKit` Class

### `new RetailPixelKit(options)`

Create an instance of `RetailPixelKit` class. It's common to import this class as just `PixelKit`.

- `options` `<Object>`: Initialization options
    - `options.path` `<string>`: USB serial port path. If both `options.ip` and `options.path` are provided, `options.path` is preferred.
    - `options.ip` `<string>`: Ip address of pixel kit to connect over Websocket.
- Returns: `<RetailPixelKit>`

Example:

```javascript
const RetailPixelKit = require('communitysdk').RetailPixelKit;
let rpk = new RetailPixelKit({ 
    path: '/dev/tty.pathtoyourmotionsensorkithere',
    ip: '192.168.0.2'
});
```

### `streamFrame(frame)`

Stream a frame containing color information for the 128 RGB pixels on the Pixel Kit.

- `frame` `Array`: Array containing 128 strings with hexadecimal colors prefixed with `#`.
- Returns: `<Promise>`

Example:

```javascript
frame = []
for(let i = 0; i < 128; i++) {
    frame.push('#FFFFFF') // White color
}
rpk.streamFrame(frame)
.then(() => {
    // Frame streamed
})
.catch((error) => {
    // Error streaming frame
})
```

### `getBatteryStatus()`

Get battery percentage and charging status.

* Returns: <Promise>
  * `rpcResponse` `<Object>`:
    * `rpcResponse.type` `<string>`: This should always be `rpc-response`
    * `rpcResponse.err` `<integer>`: This should always be `0`
    * `rpcResponse.value` `<Object>`:
      * `rpcResponse.value.percent` `<string>`: Should be a value between `'0'` and `'100'`
      * `rpcResponse.value.status` `<string>`: Should be `'Charging'` or `'Discharging'`

Example:

```javascript
rpk.getBatteryStatus()
.then((response) => {
    // Do something with the `response.value`
})
```

### `getWifiStatus()`

Get status of wifi adapter on Pixel Kit.

* Returns: <Promise>
  * `rpcResponse` `<Object>`:
    * `rpcResponse.type` `<string>`: This should always be `rpc-response`
    * `rpcResponse.err` `<integer>`: This should always be `0`
    * `rpcResponse.value` `<Object>`: 
      * `rpcResponse.value.connected` `<boolean>`: Flags if Pixel Kit is connected to a wifi network.
      * `rpcResponse.value.ip` `<string>`: Ip address of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
      * `rpcResponse.value.ssid` `<string>`: Name of wifi network that the Pixel Kit is connected. Value is empty (`''`) if not connected.
      * `rpcResponse.value.mac_address` `<string>`: [Machine address](https://whatismyipaddress.com/mac-address) of your Pixel Kit network adapter.
      * `rpcResponse.value.port` `<string>`: Opened port to connect via Websockets. Should always be `9998`
      * `rpcResponse.value.netmask` `<string>`: [Network mask](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
      * `rpcResponse.value.gateway` `<string>`: [Default gatway](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.


### `scanWifi()`

Scan available wifi networks your Pixel Kit can connect to.

* Returns: <Promise>
  * `rpcResponse` `<Object>`:
    * `rpcResponse.type` `<string>`: This should always be `rpc-response`
    * `rpcResponse.err` `<integer>`: This should always be `0`
    * `rpcResponse.value` `<Array>`: Array of objects containing information about the available networks. This objects contain:
      * `ssid` `<string>`: Network name.
      * `signal` `<integer>`: Strength of wifi signal. Ranges from `0` to `100`
      * `security` `<integer>`: Name of wifi network that the Pixel Kit is connected. Value is empty (`''`) if not connected.

