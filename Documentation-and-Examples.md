## API Documentation

- [Javascript/Node.js](https://github.com/KanoComputing/community-sdk/wiki/Node.js-SDK-API-Documentation)
- [Python 3](https://github.com/KanoComputing/community-sdk/wiki/Python-SDK-API-Documentation)

## Examples

### Javascript/Node.js

- [Listing all available Kano devices connected to your computer.](https://github.com/KanoComputing/community-sdk/blob/nodejs/examples/list_connected_devices.js)
- [Getting proximity and gesture data and change modes on Motion Sensor Kit.](https://github.com/KanoComputing/community-sdk/blob/nodejs/examples/msk_proximity_and_gesture_data.js)
- [Changing polling interval on Motion Sensor Kit.](https://github.com/KanoComputing/community-sdk/blob/nodejs/examples/msk_set_interval.js)
- [Scanning for available networks and getting battery and wifi status on Pixel Kit.](https://github.com/KanoComputing/community-sdk/blob/nodejs/examples/rpk_get_status.js)
- [Streaming to Pixel Kit.](https://github.com/KanoComputing/community-sdk/blob/nodejs/examples/rpk_stream_frame.js)
- [Getting button, joystick and dial events from Pixel Kit.](https://github.com/KanoComputing/community-sdk/blob/nodejs/examples/rpk_button_events.js)
- [Connecting Pixel Kit to Wifi.](https://github.com/KanoComputing/community-sdk/blob/nodejs/examples/rpk_connect_to_wifi.js)
- [Connecting to Pixel Kit without cables.](https://github.com/KanoComputing/community-sdk/blob/nodejs/examples/rpk_wireless.js)

### Python 3

- [Listing all available Kano devices connected to your computer](https://github.com/KanoComputing/community-sdk/blob/python/example_list_connected_devices.py)
<!-- - Getting proximity and gesture data from Motion Sensor Kit -->
<!-- - Changing modes on Motion Sensor Kit -->
- [Getting proximity values from Motion Sensor Kit.](https://github.com/KanoComputing/community-sdk/blob/python/example_motion_sensor_proximity_data.py)
- [Getting gesture name from Motion Sensor Kit.](https://github.com/KanoComputing/community-sdk/blob/python/example_motion_sensor_gesture_data.py)
- [Getting proximity values and plotting a graph on Mu Editor.](https://github.com/KanoComputing/community-sdk/blob/python/example_motion_sensor_plotter.py)
- [Changing polling interval on Motion Sensor Kit.](https://github.com/KanoComputing/community-sdk/blob/python/example_motion_sensor_polling_interval.py)
- [Scanning for available networks and getting battery and wifi status on Pixel Kit.](https://github.com/KanoComputing/community-sdk/blob/python/example_pixel_kit_get_status.py)
- [Streaming to Pixel Kit.](https://github.com/KanoComputing/community-sdk/blob/python/example_pixel_kit_stream_frame.py)
- [Getting button, joystick and dial events from Pixel Kit.](https://github.com/KanoComputing/community-sdk/blob/python/example_pixel_kit_buttons.py)
<!-- - [Connecting Pixel Kit to Wifi.]() -->

### Note for Windows users

As noticed on [this issue](https://github.com/KanoComputing/community-sdk/issues/11), Windows users might get an error saying `communitysdk` module can't be found. In those cases, adding the following lines to the beginning of your script will "fix" the issue:

```python
import sys,os
sys.path.append(os.path.abspath(os.path.dirname(__file__)))
```