## Importing the SDK

All the examples assume you are working on files inside the unzipped folder.

```python
import communitysdk
```

## Available Classes and Methods

The Node.js SDK offers access to 3 classes:

- `list_connected_devices`: List all the Kano devices currently connected to the computer over USB serial
- `MotionSensorKit`: Offers methods to send and receive data to a Motion Sensor Kit
- `RetailPixelKitSerial`: Offers methods to send and receive data to a "retail" Pixel Kit over USB serial

Example:

```python
from communitysdk import list_connected_devices, MotionSensorKit, RetailPixelKitSerial
```

## `list_connected_devices` Method

### `listConnectedDevices()`

Static method that returns all the devices connected to the computer over USB serial.

Returns an an array of devices. Each item of this array is an instance of correspondent device already connected.

- Returns: `<Array>`

Example:

```python
from communitysdk import list_connected_devices
devices = list_connected_devices()
```

## `MotionSensorKit` Class

### `MotionSensorKit(path)`

Create an instance of `MotionSensorKit` class.

- `path` `<string>`: USB serial port path
- Returns: `<MotionSensorKit>`

Example:

```python
from communitysdk import MotionSensorKit
msk = MotionSensorKit(path='/dev/tty.pathtoyourmotionsensorkithere')
```

### `msk.connect()`

Opens a connection with the hardware. When getting your `MotionSensorKit` instance from `list_connected_devices()`, it comes already connected. If you instantiate it yourself you must call `msk.connect()`, otherwise no connection will be established.

- Returns: `None`

Example:

```javascript
from communitysdk import MotionSensorKit
msk = MotionSensorKit(path='/dev/tty.pathtoyourmotionsensorkithere')
msk.connect()
```

### `msk.set_mode(mode)`

Switch Motion Sensor Kit modes. The available modes are `proximity` and `gesture`. It returns `True` if successfully changed and `None` otherwise.

- `mode` `<string>`: Which mode to set. Must be `proximity` or `gesture`
- Returns: `<Object>` | `<boolean>` | `None`

Example:

```python
response = msk.set_mode('gesture')
```

### `msk.set_interval(interval)`

Set how often the Motion Sensor Kit will poll for proximity data. It returns `True` if successfully changed and `None` otherwise.

- `interval` `<integer>`: Interval in milliseconds between data polling.
- Returns: `<Object>` | `<boolean>` | `None`

Example:

```python
response = msk.set_interval('gesture')
```

### `msk.on_proximity(proximityValue)`

This method is called every time Motion Sensor gets proximity data.

- `proximityValue` `<integer>`: Value of proximity data. Range from `0` to `255`

Example:

```python
def print_proximity(proximityValue):
    print(proximityValue)
msk.on_proximity = print_proximity
```

### `msk.on_gesture(gestureValue)`

This method is called every time Motion Sensor gets gesture data.

- `gestureValue` `<string>`: Name of gesture detected by Motion Sensor. Values are `up`, `down`, `left` or `right`

Example:

```python
def print_gesture(gestureValue):
    print(gestureValue)
msk.on_gesture = print_gesture
```

## `RetailPixelKitSerial` Class

### `RetailPixelKitSerial(path)`

Create an instance of `RetailPixelKitSerial` class.

- `path` `<string>`: USB serial port path.
- Returns: `<RetailPixelKit>`

Example:

```python
from communitysdk import RetailPixelKitSerial as PixelKit
rpk = PixelKit(path='/dev/tty.pathtoyourpixelkithere')
```

### `rpk.connect()`

Opens a connection with the hardware. When getting your `RetailPixelKitSerial` instance from `list_connected_devices()`, it comes already connected. If you instantiate it yourself you must call `rpk.connect()`, otherwise no connection will be established.

- Returns: `None`

Example:

```javascript
from communitysdk import RetailPixelKitSerial as PixelKit
rpk = PixelKit(path='/dev/tty.pathtoyourmotionsensorkithere')
rpk.connect()
```

### `rpk.stream_frame(frame)`

Stream a frame containing color information for the 128 RGB pixels on the Pixel Kit.

- `frame` `Array`: Array containing 128 strings with hexadecimal colors prefixed with `#`.
- Returns: `None`

Example:

```python
frame = []
for i in range(0, 128):
    frame.append('#FFFFFF') # White color
rpk.stream_frame(frame)
```

### `rpk.get_battery_status()`

Get battery percentage and charging status.

* Returns: <Object>
    * `battery_status` `<Object>`:
        * `battery_status.percent` `<string>`: Should be a value between `'0'` and `'100'`
        * `battery_status.status` `<string>`: Should be `'Charging'` or `'Discharging'`

Example:

```python
battery_status = rpk.get_battery_status()
```

### `rpk.get_wifi_status()`

Get status of wifi adapter on Pixel Kit.

* Returns: <Object>
    * `wifi_status` `<Object>`:
        * `wifi_status.connected` `<boolean>`: Flags if Pixel Kit is connected to a wifi network.
        * `wifi_status.ip` `<string>`: Ip address of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
        * `wifi_status.ssid` `<string>`: Name of wifi network that the Pixel Kit is connected. Value is empty (`''`) if not connected.
        * `wifi_status.signal` `<integer>`: Strength of wifi signal. Ranges from `0` to `100`
        * `wifi_status.mac_address` `<string>`: [Machine address](https://whatismyipaddress.com/mac-address) of your Pixel Kit network adapter.
        * `wifi_status.port` `<string>`: Opened port to connect via Websockets. Should always be `9998`
        * `wifi_status.netmask` `<string>`: [Network mask](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
        * `wifi_status.gateway` `<string>`: [Default gatway](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.

Example:

```python
wifi_status = rpk.get_wifi_status()
```

### `rpk.scan_wifi()`

Scan available wifi networks your Pixel Kit can connect to.

* Returns: <Array>
    * `available_networks` `<Array>`: Array of objects containing information about the available networks. This objects contain:
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

```python
available_networks = rpk.scan_wifi()
```

### `rpk.connect_to_wifi(ssid, password)`

Connect your Pixel Kit to a network.

* `ssid` `<string>`: Network's name
* `password` `<string>`: Network's password
* Returns: <Object>
    * `wifi_status` `<Object>`:
        * `wifi_status.connected` `<boolean>`: Flags if Pixel Kit is connected to a wifi network.
        * `wifi_status.ip` `<string>`: Ip address of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
        * `wifi_status.ssid` `<string>`: Name of wifi network that the Pixel Kit is connected. Value is empty (`''`) if not connected.
        * `wifi_status.value.signal` `<integer>`: Strength of wifi signal. Ranges from `0` to `100`
        * `wifi_status.mac_address` `<string>`: [Machine address](https://whatismyipaddress.com/mac-address) of your Pixel Kit network adapter.
        * `wifi_status.port` `<string>`: Opened port to connect via Websockets. Should always be `9998`
        * `wifi_status.netmask` `<string>`: [Network mask](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.
        * `wifi_status.gateway` `<string>`: [Default gatway](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) of your Pixel Kit. Returns `0.0.0.0` if not connected to any network.

Example:

```python
wifi_status = rpk.connect_to_wifi('my wifi network', 'supersecretpassword123')
```

### `rpk.on_button_down(buttonId)`

This method is called when any button or joystick is pressed.

* `buttonId` `<string>`: Button id that just got pressed. It could be `btn-A`, `btn-B`, `js-up`, `js-down`, `js-left`, `js-right`, `js-click` or `btn-reset` (yes, you can use the button on the back of your Pixel Kit!).

Example:

```python
def print_button_id(buttonId):
    print(buttonId)
rpk.on_button_down = print_button_id
```

### `rpk.on_button_up(buttonId)`

This method is called when any button or joystick is released.

* `buttonId` `<string>`: Button id that just got released. It could be `btn-A`, `btn-B`, `js-up`, `js-down`, `js-left`, `js-right`, `js-click` or `btn-reset` (yes, you can use the button on the back of your Pixel Kit!).

Example:

```python
def print_button_id(buttonId):
    print(buttonId)
rpk.on_button_up = print_button_id
```

### `rpk.on_dial(modeId)`

This method is called when the "mode" dial is turned.

* `modeId` `<string>`: Name of the mode relative to the dial position. It could be `offline-1`, `offline-2`, `online-1`, `online-2`, `online-3`.

Example:

```python
def print_mode_id(modeId):
    print(modeId)
rpk.on_dial = print_mode_id
```
