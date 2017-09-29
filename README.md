# Airtame CLI

**Disclaimer: This is a demo version. This is not complete and this is not tested for production.**

## How to use it ?

As you would use any other software:

```
airtame-application --set-settings=/home/test/settings.json --ip=192.168.1.1
```

This command will simply set some settings for the device with the ip indicated.

## What's in it ?

#### --set-settings

As stated earlier, the simplest command is:

```
airtame-application --set-settings=/home/test/settings.json --ip=192.168.1.1
```

You can also change settings for multiple devices:

```
airtame-application --set-settings=/home/test/settings.json,/home/test/settings2.json --ip=192.168.1.1,192.168.1.2
```

You simply need to add more files and more ips by separating them with a comma !

_PS: You need as many files as there are ips. 1st file for 1st ip, 2nd file for 2nd ip, ... A file can be used multiple times (but try not to use the same `device_name` or `cloud_token`)_

##### Format of the settings file:

```js
{
    "ap_24_channel": 6,
    "ap_24_enabled": true,
    "ap_52_channel": 40,
    "ap_52_enabled": false,
    "ap_psk": "", // If you want to protected the access point of the device (only if at least one of the access point is true)
    "auto_update": true,
    "cloud_token": "",
    "device_name": "This is a test",
    "discovery_protocol": "both", // can be both, ssdp, mdns, none
    "led_enabled": true,
    "manage_password": "", // empty if no password, or set a password, it will protect the device
    "network": { // should be empty object or with at least the mandatory fields
        "bssid": "",
        "frequency": 5180, // mandatory
        "key_mgmt": "", // mandatory
        "psk": "",
        "ssid": "", // mandatory
        "eap": "",
        "identity": "",
        "ca_cert": "", // content of the certicate in base64
        "private_key": "", // content of the certicate in base64
        "private_key_passwd": "",
        "client_cert": "", // content of the certicate in base64
        "password": "",
        "phase1": "",
        "phase2": "",
        "anonymous_identity": ""
    },
    "pincode_enabled": false,
    "pincode_override_enabled": false,
    "resolution": "", // Usually the software will choose that for you
    "splash_content": [], // explained just bellow
    "splash_custom_text": "markworn text here",
    "splash_layout": "guide", // can be guide, side, minimal, full
    "update_channel": "alpha", // can be nightly (hightly discouraged), alpha (still not a good idea), beta, ga
    "wifi_forwarding": false
}
```


Splash content is an array and contain either 1 of the two objects bellow:
```json
{
  "type": "image",
  "content": "base64formattedimage" // cf. bottom of this doc
}
// Or
{
  "type": "url",
  "content": "http://airtame.com"
}
```

#### --get-settings

You can also retrieve settings from devices:

```
airtame-application --get-settings --ip=192.168.1.1,192.168.1.2
```

You can as well put one or more ip for this command, and the result will be:

```json
{
  "192.168.1.1": {
    "settings": {
      "ap_24_channel": 6,
      "ap_24_enabled": true,
      "ap_52_channel": 40,
      "ap_52_enabled": false,
      "ap_psk": "",
      "auto_update": true,
      "cloud_token": "",
      "device_name": "This is a test",
      "discovery_protocol": "both",
      "led_enabled": true,
      "manage_password": false,
      "network": {
        "bssid": "",
        "frequency": 5180,
        "key_mgmt": "WPA-PSK",
        "psk": "somepassword",
        "ssid": "thisisanicessid"
      },
      "pincode_enabled": false,
      "pincode_override_enabled": false,
      "resolution": "",
      "splash_content": [],
      "splash_custom_text": "# Present Wirelessly\n\nConnect to WiFi **{{WIFI_NAME}}**\n\nDownload app **airtame.com/start**\n\nPresent from app **{{DEVICE_NAME}}** or type
 **{{DEVICE_IP}}**\n\n",
      "splash_layout": "side",
      "update_channel": "alpha",
      "wifi_forwarding": false
    }
  },
  "192.168.1.2": {
    "settings": {
      "ap_24_channel": 6,
      "ap_24_enabled": true,
      "ap_52_channel": 40,
      "ap_52_enabled": false,
      "ap_psk": "",
      "auto_update": true,
      "cloud_token": "",
      "device_name": "This is another test",
      "discovery_protocol": "both",
      "led_enabled": true,
      "manage_password": false,
      "network": {
        "bssid": "",
        "frequency": 5180,
        "key_mgmt": "WPA-PSK",
        "psk": "somepassword",
        "ssid": "thisisanicessid"
      },
      "pincode_enabled": false,
      "pincode_override_enabled": false,
      "resolution": "",
      "splash_content": [],
      "splash_custom_text": "# Present Wirelessly\n\nConnect to WiFi **{{WIFI_NAME}}**\n\nDownload app **airtame.com/start**\n\nPresent from app **{{DEVICE_NAME}}** or type
 **{{DEVICE_IP}}**\n\n",
      "splash_layout": "side",
      "update_channel": "alpha",
      "wifi_forwarding": false
    }
  }
}
```

#### --verbose (or -v)

If you encounter issues you can use the verbose command to add the logs:

```
airtame-application --get-settings --ip=192.168.1.1 -v
```

Which will result in something like:

```
silly: [initializer]: [>] initPath
silly: [initializer]: [<] initPath
silly: [app]: [>] constructor
silly: [app]: [=] _handleExceptions
silly: [app]: [<] constructor
silly: [app]: [>] start
silly: [app]: [<] start
silly: [app]: [>] run
silly: [app]: [=] get version
info: AIRTAME Version: 0.0.0 Channel: devel
info: [app]: Getting configuration from 10.13.37.94
silly: [app]: Request body chunk: {"settings":{"ap_24_channel":6,"ap_24_enabled":true,"ap_52_channel":40,"ap_52_enabled":false,"ap_psk":"","auto_update":true,"clo
ud_token":"","device_name":"This is a test","discovery_protocol":"both","led_enabled":true,"manage_password":false,"network":{"bssid":"","frequency":5180,"
key_mgmt":"WPA-PSK","psk":"","ssid":""},"pincode_enabled":false,"pincode_override_enabled":false,"resolution":"","splash_content":[],"splash_custom_text
":"# Present Wirelessly\n\nConnect to WiFi **{{WIFI_NAME}}**\n\nDownload app **airtame.com/start**\n\nPresent from app **{{DEVICE_NAME}}** or type **{{DEVICE_IP}}**\n\n","s
plash_layout":"side","update_channel":"alpha","wifi_forwarding":false}}
silly: [app]: Request ended.
{
  "10.13.37.94": {
    "settings": {
      "ap_24_channel": 6,
      "ap_24_enabled": true,
      "ap_52_channel": 40,
      "ap_52_enabled": false,
      "ap_psk": "",
      "auto_update": true,
      "cloud_token": "",
      "device_name": "This is a test",
      "discovery_protocol": "both",
      "led_enabled": true,
      "manage_password": false,
      "network": {
        "bssid": "",
        "frequency": 5180,
        "key_mgmt": "WPA-PSK",
        "psk": "",
        "ssid": ""
      },
      "pincode_enabled": false,
      "pincode_override_enabled": false,
      "resolution": "",
      "splash_content": [],
      "splash_custom_text": "# Present Wirelessly\n\nConnect to WiFi **{{WIFI_NAME}}**\n\nDownload app **airtame.com/start**\n\nPresent from app **{{DEVICE_NAME}}** or type
 **{{DEVICE_IP}}**\n\n",
      "splash_layout": "side",
      "update_channel": "alpha",
      "wifi_forwarding": false
    }
  }
}
silly: [app]: [>] quit
silly: [app]: [<] quit
```

#### --stream (or -s)

```
airtame-application -s --ip=192.168.1.1,192.168.1.2
```

You can stream to multiple devices that way.

**This functionality is not properly tested. Use it at your own risk.**

#### --help (or -s)

This will display the help of the CLI:

```
airtame-application -h
```

Which gives:

```
Options:
  --set-settings   Configuration file to apply to airtames              [string]
  --stream, -s     Allows you to stream to a device                    [boolean]
  --get-settings   Allows you to retrieve settings from a device in JSON format
                                                                       [boolean]
  --get-state      Allows you to retrieve the state from a device in JSON format
                                                                       [boolean]
  --ip             IP of the device                                     [string]
  --help           Show help                                           [boolean]
  --reset-default                                     [boolean] [default: false]
  --verbose, -v                                       [boolean] [default: false]
```

## What's not in it ?

- Upload of images (a workaround is explained at the bottom of this file)
- Communication with password protected devices
- Changing settings for streaming (for streaming quality)
- Discovery of devices
- Discovery of not setup devices
- Extreme error handling


### How to upload an image ?

You can provide a `splash_content` that way:

```json
"splash_content": {
  "type": "image",
  "content": "data:<mime type>;base64,<encoded data>"
}
```

`<mime type>` should be replaced with the mime type of the image:

Supported format are:

- JPEG: mime_type = "jpeg"
- PNG: mime_type = "png"
- GIF: mime_type = "gif"

`<encoded data>` should be replaced with the content of the image encoded in base64.