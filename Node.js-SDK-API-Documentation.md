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

Returns a promise that resolves with an array of devices. Each item of this array is an instance of correspondent device **already connected**.

- Returns: `<Promise>`

Example:

```javascript
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

Create an instance of `MotionSensorKit` class. Creating the instance does not open a connection to the device, for that, check the `msk.connect()` method.

- `options` `<Object>`: Initialization options
    - `options.path` `<string>`: USB serial port path
- Returns: `<MotionSensorKit>`

Example:

```javascript
const MotionSensorKit = require('communitysdk').MotionSensorKit;
let msk = new MotionSensor({ path: '/dev/tty.pathtoyourmotionsensorkithere' });
```
### `msk.connect()`

Opens a connection with the hardware. When getting your `MotionSensorKit` instance from `listConnectedDevices()`, it comes already connected. If you instantiate it yourself you must call `msk.connect()`, otherwise no connection will be established. This method resolves with the connected instance.

- Returns: `<Promise>`

Example:

```javascript
const MotionSensorKit = require('communitysdk').MotionSensorKit;
let msk = new MotionSensorKit({path: '/dev/tty.pathtoyourpixelkithere'});
msk.connect()
.then((connectedMsk) => {
    // Do something with `connectedMsk` or `msk`
});
```

### `msk.setMode(mode)`

Switch Motion Sensor Kit modes. The available modes are `proximity` and `gesture`.

- `mode` `<string>`: Which mode to set. Must be `proximity` or `gesture`
- Returns: `<Promise>`

Example:

```javascript
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

```javascript
msk.setInterval(250)
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

Triggered when Motion Sensor Kit gets an error message from the RPC connection.

- `errorMessage` `<string>`: Message describing an error sent through RPC.

Example:

```javascript
msk.on('error-message', (errorMessage) => {
    console.log(errorMessage);
})
```

## `RetailPixelKit` Class

### `new RetailPixelKit(options)`

Create an instance of `RetailPixelKit` class.  Creating the instance does not open a connection to the device, for that, check the `rpk.connect()` method.

- `options` `<Object>`: Initialization options
    - `options.path` `<string>`: USB serial port path. If both `options.ip` and `options.path` are provided, `options.path` is preferred.
    - `options.ip` `<string>`: Ip address of pixel kit to connect over Websocket.
- Returns: `<RetailPixelKit>`

Example:

```javascript
const RetailPixelKit = require('communitysdk').RetailPixelKit;
let rpk = new RetailPixelKit({
    path: '/dev/tty.pathtoyourpixelkithere',
    ip: '192.168.0.2'
});
```

### `rpk.connect()`

Opens a connection with the hardware. When getting your `RetailPixelKit` instance from `listConnectedDevices()`, it comes already connected. If you instantiate it yourself you must call `rpk.connect()`, otherwise no connection will be established. This method resolves with the connected instance.

- Returns: `<Promise>`

Example:

```javascript
const RetailPixelKit = require('communitysdk').RetailPixelKit;
let rpk = new RetailPixelKit({path: '/dev/tty.pathtoyourpixelkithere'});
rpk.connect()
.then((connectedRpk) => {
    // Do something with `connectedRPK` or `rpk`
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
    * `rpcResponse.id` `<string>`: RPC request id that this response relates to.
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
    * `rpcResponse.id` `<string>`: RPC request id that this response relates to.
    * `rpcResponse.value` `<Object>`:
      * `rpcResponse.value.connected` `<boolean>`: Flags if Pixel Kit is connected to a wifi network.
      * `rpcResponse.value.ip` `<string>`: Ip address of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
      * `rpcResponse.value.ssid` `<string>`: Name of wifi network that the Pixel Kit is connected. Value is empty (`''`) if not connected.
      * `rpcResponse.value.signal` `<integer>`: Strength of wifi signal. Ranges from `0` to `100`
      * `rpcResponse.value.mac_address` `<string>`: [Machine address](https://whatismyipaddress.com/mac-address) of your Pixel Kit network adapter.
      * `rpcResponse.value.port` `<string>`: Opened port to connect via Websockets. Should always be `9998`
      * `rpcResponse.value.netmask` `<string>`: [Network mask](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
      * `rpcResponse.value.gateway` `<string>`: [Default gatway](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.

Example:

```javascript
rpk.getWifiStatus()
.then((response) => {
    // Do something with the `response.value`
})
```

### `scanWifi()`

Scan available wifi networks your Pixel Kit can connect to.

* Returns: <Promise>
  * `rpcResponse` `<Object>`:
    * `rpcResponse.type` `<string>`: This should always be `rpc-response`
    * `rpcResponse.err` `<integer>`: This should always be `0`
    * `rpcResponse.id` `<string>`: RPC request id that this response relates to.
    * `rpcResponse.value` `<Array>`: Array of objects containing information about the available networks. This objects contain:
      * `ssid` `<string>`: Network name.
      * `signal` `<integer>`: Strength of wifi signal. Ranges from `0` to `100`
      * `security` `<integer>`: What kind of [authentication mode](https://esp-idf.readthedocs.io/en/latest/api-reference/wifi/esp_wifi.html#_CPPv216wifi_auth_mode_t) this network implements.
        * `0`: Open network
        * `1`: [WEP](https://en.wikipedia.org/wiki/Wired_Equivalent_Privacy)
        * `2`: [WPA_PSK](https://en.wikipedia.org/wiki/Wi-Fi_Protected_Access)
        * `3`: [WPA2_PSK](https://en.wikipedia.org/wiki/Wi-Fi_Protected_Access)
        * `4`: [WPA_WPA2_PSK](https://en.wikipedia.org/wiki/Wi-Fi_Protected_Access)
        * `5`: [WPA2_ENTERPRISE](https://en.wikipedia.org/wiki/Wi-Fi_Protected_Access)

Example:

```javascript
rpk.scanWifi()
.then((response) => {
    // Do something with the `response.value`
})
```

### `connectToWifi(ssid, password)`

Connect your Pixel Kit to a network.

* `ssid` `<string>`: Network's name
* `password` `<string>`: Network's password
* Returns: <Promise>
  * `rpcResponse` `<Object>`:
    * `rpcResponse.connected` `<boolean>`: Flags if Pixel Kit is connected to a wifi network.
    * `rpcResponse.ip` `<string>`: Ip address of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
    * `rpcResponse.ssid` `<string>`: Name of wifi network that the Pixel Kit is connected. Value is empty (`''`) if not connected.
    * `rpcResponse.value.signal` `<integer>`: Strength of wifi signal. Ranges from `0` to `100`
    * `rpcResponse.mac_address` `<string>`: [Machine address](https://whatismyipaddress.com/mac-address) of your Pixel Kit network adapter.
    * `rpcResponse.port` `<string>`: Opened port to connect via Websockets. Should always be `9998`
    * `rpcResponse.netmask` `<string>`: [Network mask](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
    * `rpcResponse.gateway` `<string>`: [Default gatway](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.

Example:

```javascript
rpk.connectToWifi('my wifi network', 'supersecretpassword123')
.then((response) => {
    // Do something with the `response.value`
})
```

### Event: `button-down`

Triggered when any button or joystick is pressed.

* `buttonId` `<string>`: Button id that just got pressed. It could be `btn-A`, `btn-B`, `js-up`, `js-down`, `js-left`, `js-right`, `js-click` or `btn-reset` (yes, you can use the button on the back of your Pixel Kit!).

Example:

```javascript
rpk.on('button-down', (buttonId) => {
    console.log(buttonId);
});
```

### Event: `button-up`

Triggered when any button or joystick is released.

* `buttonId` `<string>`: Button id that just got released. It could be `btn-A`, `btn-B`, `js-up`, `js-down`, `js-left`, `js-right`, `js-click` or `btn-reset` (yes, you can use the button on the back of your Pixel Kit!).

Example:

```javascript
rpk.on('button-up', (buttonId) => {
    console.log(buttonId);
});
```

### Event: `dial`

Triggered when the "mode" dial is turned.

* `modeId` `<string>`: Name of the mode relative to the dial position. It could be `offline-1`, `offline-2`, `online-1`, `online-2`, `online-3`.

Example:

```javascript
rpk.on('dial', (modeId) => {
    console.log(modeId);
});
```

### Event: `error-message`

Triggered when Pixel Kit gets an error message from the RPC connection.

- `errorMessage` `<string>`: Message describing an error sent through RPC.

Example:

```javascript
rpk.on('error-message', (errorMessage) => {
    console.log(errorMessage);
})
```
