<h2 align="center">
  <a href="https://reolink.com"><img src="./logo.png" alt="Reolink logotype" width="200"></a>
  <br>
  <i>Home Assistant Reolink addon</i>
  <br>
</h2>

<p align="center">
  <a href="https://github.com/custom-components/hacs"><img src="https://img.shields.io/badge/HACS-Custom-orange.svg"></a>
  <img src="https://img.shields.io/github/v/release/fwestenberg/reolink_dev" alt="Current version">
</p>

The `reolink` implementation allows you to integrate your [Reolink](https://www.reolink.com/) devices in Home Assistant.

You can configure the Reolink integration by going to the integrations page inside the configuration panel.

## Installation
To install this repository using HACS, please follow [this](https://hacs.xyz/docs/faq/custom_repositories) guide.
It can also be installed manually, this requires some knowledge I will not explain here.

In your Home Assistant installation go to: Configuration > Integrations, click the button Add Integration > Reolink IP camera
Enter the details for your camera. The camera will now be available as an entity.

For the motion detection to work, Home Assistant must be reachable via http from your local network. So when using https internally, motion detection will not work at this moment.

## Services

The Reolink integration supports all default camera [services](https://www.home-assistant.io/integrations/camera/#services) and additionally provides the following services:

### Service `reolink.set_sensitivity`

Set the motion detection sensitivity of the camera. Either all time schedule presets can be set at once, or a specific preset can be specified.

| Service data attribute  | Optional  | Description  |
| :---------------------- | :-------- | :----------- |
| `entity_id`             | no        | The camera to control.
| `sensitivity`           | no        | The sensitivity to set, a value between 1 (low sensitivity) and 50 (high sensitivity).
| `preset`                | yes       | The time schedule preset to set. Presets can be found in the Web UI of the camera.

### Service `reolink.set_daynight`

Set the day and night mode parameter of the camera.  

| Service data attribute  | Optional  | Description  |
| :---------------------- | :-------- | :----------- |
| `entity_id`             | no        | The camera to control.
| `mode`                  | no        | The day and night mode parameter supports the following values: `AUTO` Auto switch between black & white mode `COLOR` Always record videos in color mode `BLACKANDWHITE` Always record videos in black & white mode.

### Service `reolink.ptz_control`

Control the PTZ (Pan Tilt Zoom) movement of the camera.

| Service data attribute  | Optional  | Description  |
| :---------------------- | :-------- | :----------- |
| `entity_id`             | no        | The camera to control.
| `command`               | no        | The command to execute. Possibe values are: `AUTO`, `DOWN`, `FOCUSDEC`, `FOCUSINC`, `LEFT`, `LEFTDOWN`, `LEFTUP`, `RIGHT`, `RIGHTDOWN`, `RIGHTUP`, `STOP`, `TOPOS`, `UP`, `ZOOMDEC` and `ZOOMINC`.
| `preset`                | yes       | In case of the command `TOPOS`, pass the preset ID here. The possible presets are listed as attribute on the camera.
| `speed`                 | yes       | The speed at which the camera moves. Not applicable for the commands: `STOP` and `AUTO`.

<div class='note'>
The camera keeps moving until the `STOP` command is passed to the service.
</div>

## Camera

This integration creates a camera entity, providing a live-stream configurable from the integrations page. In the options menu, the following parameters can be configured:

| Parameter               | Description                                                                                                 |
| :-------------------    | :---------------------------------------------------------------------------------------------------------- |
| Stream                  | Switch between Sub or Main camera stream.                                                                   |
| Protocol                | Switch between the RTMP or RTSP streaming protocol.                                                         |
| Channel                 | When using a single camera, choose stream 0. When using a NVR, switch between the different camera streams. |

## Binary Sensor

When the camera supports motion detection events, a binary sensor is created for real-time motion detection. The time to switch motion detection off can be configured via the options menu, located at the integrations page. Please notice: for using the motion detection, your Homa Assistant should be reachable (within you local network) over http (not https).

| Parameter               | Description                                                                                                 |
| :-------------------    | :---------------------------------------------------------------------------------------------------------- |
| Motion sensor off delay | Control how many seconds it takes (after the last motion detection) for the binary sensor to switch off.    |

## Switch

Depending on the camera, the following switches are created:

This integration creates a camera entity, which can be configured from the integrations page. In the options menu, the following parameters can be configured:

| Switch               | Description |
| :------------------- | :------------------------------------------------------------ |
| Email                | Switch email alerts from the camera when motion is detected.  |
| FTP                  | Switch FTP upload of photo and video when motion is detected. |
| IR lights            | Switch the infrared lights to auto or off.                    |
| Record audio         | Record auto or mute. This also implies the live-stream.       |
| Recording            | Switch recording to the SD card.                              |

## Unsupported models

The following models are not to be supported:

- E1
- E1 Pro
- Battery-powered cameras
- B800: Only with NVR
- B400: Only with NVR
- D400: Only with NVR
