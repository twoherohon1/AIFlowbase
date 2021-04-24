# node-red-contrib-google-video-object-detection

This is a Node Red node that takes a video input, sends it to the Google Video
Intelligence API and returns a list of objects from the video clip along with
the confidence of each object and details of when it was seen in the video.

## Installation

Please follow the Node-RED
[documentation](https://nodered.org/docs/getting-started/adding-nodes) to
install `node-red-contrib-google-video-object-detection`.

## Google Cloud Credentials

The node created in this package requires Google Cloud Service Credentials to
run.

In your Google Cloud Project please add Billing to your account and then add the
Vision API.

The credentials for a service account can be acquired from the
[Google Cloud APIs & Services](https://console.cloud.google.com/apis/credentials)
menu. After you finish creating a service account key, it will be downloaded in
JSON format and saved to a local file.

In the node configuration you have to supply the path to the JSON credentials
file.

## How to use

This package creates a new node in your Node Red instance called
`google video object detection` under the `Object Detection` category in your
node list.

![Google Video Object Detection node in Node
Red](https://imgur.com/ceaYLIK.png)

Drag this node into your flow.

Double click on the node to set the path to your Google Cloud services
credentials JSON file.

Input a `msg` object with either of the following:

- `msg.filename` a relative path to the video clip file you wish to process (on
  your local filesystem)
- `msg.payload` a Base64 representation of the video clip

See the below example for more information.

## Example flow

Copy the below JSON and use "import" in node red to import this flow.

![Google Video Object Detection node in use in a
flow](https://imgur.com/tJGPVYS.png)

```json
[
  {
    "id": "f26d411d.674f2",
    "type": "file in",
    "z": "ff2832e2.b6c11",
    "name": "",
    "filename": "/path/to/video.mp4",
    "format": "utf8",
    "chunk": false,
    "sendError": false,
    "encoding": "base64",
    "x": 430,
    "y": 180,
    "wires": [["dc482fa2.3e6d5"]]
  },
  {
    "id": "dc482fa2.3e6d5",
    "type": "google-video-object-detection",
    "z": "ff2832e2.b6c11",
    "keyFilename": "/path/to/service_account.json",
    "name": "",
    "x": 630,
    "y": 180,
    "wires": [["3f4a5e23.9f0512"]]
  },
  {
    "id": "3f4a5e23.9f0512",
    "type": "function",
    "z": "ff2832e2.b6c11",
    "name": "Create array of objects",
    "func": "let objs = []\nconst objects = msg.payload.objectAnnotations;\nobjects.forEach(object => {\n    objs.push(object.entity.description);\n});\n\nmsg.payload = objs;\n\nreturn msg;",
    "outputs": 1,
    "noerr": 0,
    "x": 840,
    "y": 180,
    "wires": [["791bf534.bace7c"]]
  },
  {
    "id": "791bf534.bace7c",
    "type": "debug",
    "z": "ff2832e2.b6c11",
    "name": "",
    "active": true,
    "tosidebar": true,
    "console": false,
    "tostatus": false,
    "complete": "false",
    "x": 1050,
    "y": 180,
    "wires": []
  },
  {
    "id": "a339af40.9bee8",
    "type": "inject",
    "z": "ff2832e2.b6c11",
    "name": "",
    "topic": "",
    "payload": "",
    "payloadType": "date",
    "repeat": "",
    "crontab": "",
    "once": false,
    "onceDelay": 0.1,
    "x": 240,
    "y": 180,
    "wires": [["f26d411d.674f2"]]
  }
]
```
