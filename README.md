# Fronius PV Inverter smarthome
Connect Fronius Symo Solar PV Inverter with Node Red and Alexa Echo

## Features

* Ask Alexa for power values of Fronius inverter
* Start / stop electriv devices depending of solar power

## Requirements

* Fronius Symo Inverter
* Raspberry Pi with Node Red (https://nodered.org/docs/getting-started/raspberrypi)
  * Install in Node-Red:
    * https://flows.nodered.org/node/node-red-contrib-virtual-smart-home
    * https://flows.nodered.org/node/node-red-contrib-alexa-remote2
* Amazon Alexa Acount
* Echo Smart Plug in the example or Shally plug.
* Fronius and Raspberry must be in same local network 

## Hints

* Get Fronius Realtime Data with Json Request: 
  * http://192.168.0.188/solar_api/v1/GetPowerFlowRealtimeData.fcgi  (select correct IP of Fronius Inverter)
  

## Example

Import to node red:

[{"id":"ecfafe50.ff408","type":"tab","label":"Flow 1","disabled":false,"info":""},{"id":"570d3d14.d60544","type":"inject","z":"ecfafe50.ff408","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":180,"y":320,"wires":[["b265dda9.6188d"]]},{"id":"b265dda9.6188d","type":"http request","z":"ecfafe50.ff408","name":"fronius","method":"GET","ret":"obj","paytoqs":"ignore","url":"http://192.168.0.188/solar_api/v1/GetPowerFlowRealtimeData.fcgi","tls":"","persist":false,"proxy":"","authType":"","x":430,"y":320,"wires":[["f8c91f52.f0528"]]},{"id":"c718d277.62082","type":"alexa-remote-routine","z":"ecfafe50.ff408","name":"Plug On","account":"b2848ee4.f53ed","routineNode":{"type":"smarthome","payload":{"entity":"14eebfae-b55f-4479-a13f-60d75d9a57d6","action":"turnOn"}},"x":920,"y":220,"wires":[[]]},{"id":"918dfba9.6f9228","type":"debug","z":"ecfafe50.ff408","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":930,"y":160,"wires":[]},{"id":"a7bfffa4.4fbb4","type":"alexa-remote-routine","z":"ecfafe50.ff408","name":"Plug Off","account":"b2848ee4.f53ed","routineNode":{"type":"smarthome","payload":{"entity":"14eebfae-b55f-4479-a13f-60d75d9a57d6","action":"turnOff"}},"x":900,"y":360,"wires":[[]]},{"id":"5eb1bf12.df0ab","type":"debug","z":"ecfafe50.ff408","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":910,"y":440,"wires":[]},{"id":"f8c91f52.f0528","type":"switch","z":"ecfafe50.ff408","name":"","property":"payload.Body.Data.Site.P_PV","propertyType":"msg","rules":[{"t":"gt","v":"1100","vt":"str"},{"t":"lt","v":"1000","vt":"str"}],"checkall":"true","repair":false,"outputs":2,"x":630,"y":320,"wires":[["c718d277.62082","918dfba9.6f9228"],["a7bfffa4.4fbb4","5eb1bf12.df0ab"]]},{"id":"e850c689.6e3268","type":"alexa-remote-routine","z":"ecfafe50.ff408","name":"","account":"b2848ee4.f53ed","routineNode":{"type":"speak","payload":{"type":"announcement","text":{"type":"msg","value":"PV_Daten"},"devices":["G0911B05947314C6"]}},"x":680,"y":600,"wires":[["4d06a275.562e6c"]]},{"id":"c64b51cd.f256c","type":"http request","z":"ecfafe50.ff408","name":"fronius","method":"GET","ret":"obj","paytoqs":"ignore","url":"http://192.168.0.188/solar_api/v1/GetPowerFlowRealtimeData.fcgi","tls":"","persist":false,"proxy":"","authType":"","x":330,"y":620,"wires":[["488f74d9.3f5acc"]]},{"id":"409e105e.e9573","type":"http request","z":"ecfafe50.ff408","name":"fronius","method":"GET","ret":"obj","paytoqs":"ignore","url":"http://192.168.0.188/solar_api/v1/GetPowerFlowRealtimeData.fcgi","tls":"","persist":false,"proxy":"","authType":"","x":370,"y":120,"wires":[["ccfc8d75.010e3","d0506421.67e9b8","af499942.1c9668","261c342e.0fea3c"]]},{"id":"6217d134.45657","type":"inject","z":"ecfafe50.ff408","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":170,"y":120,"wires":[["409e105e.e9573"]]},{"id":"ccfc8d75.010e3","type":"debug","z":"ecfafe50.ff408","name":"PV (kW)","active":true,"tosidebar":true,"console":false,"tostatus":true,"complete":"payload.Body.Data.Site.P_PV","targetType":"msg","statusVal":"payload","statusType":"auto","x":560,"y":120,"wires":[]},{"id":"d0506421.67e9b8","type":"debug","z":"ecfafe50.ff408","name":"Netz (kW)","active":true,"tosidebar":true,"console":false,"tostatus":true,"complete":"payload.Body.Data.Site.P_Grid","targetType":"msg","statusVal":"payload","statusType":"auto","x":560,"y":180,"wires":[]},{"id":"af499942.1c9668","type":"debug","z":"ecfafe50.ff408","name":"Verbrauch (kW)","active":true,"tosidebar":true,"console":false,"tostatus":true,"complete":"payload.Body.Data.Site.P_Load","targetType":"msg","statusVal":"payload","statusType":"auto","x":580,"y":240,"wires":[]},{"id":"261c342e.0fea3c","type":"debug","z":"ecfafe50.ff408","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload.Body.Data.Site","targetType":"msg","statusVal":"","statusType":"auto","x":610,"y":80,"wires":[]},{"id":"4d06a275.562e6c","type":"debug","z":"ecfafe50.ff408","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":790,"y":720,"wires":[]},{"id":"488f74d9.3f5acc","type":"function","z":"ecfafe50.ff408","name":"","func":"\n\nmsg.PV_Daten = \"Stromverbrauch: \"  \n    + msg.payload.Body.Data.Site.P_Grid + \"kilowatt\" \n    + \", Solarstrom: \"  \n    + msg.payload.Body.Data.Site.P_PV + \"kilowatt\" ;\n\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","x":470,"y":720,"wires":[["e850c689.6e3268"]]},{"id":"53ca9790.635828","type":"vsh-virtual-device","z":"ecfafe50.ff408","name":"PV Switch","connection":"21a58516.c9540a","template":"SWITCH","passthrough":false,"x":130,"y":620,"wires":[["c64b51cd.f256c"]]},{"id":"b2848ee4.f53ed","type":"alexa-remote-account","name":"Alexa Account Eidmann","authMethod":"proxy","proxyOwnIp":"192.168.0.117","proxyPort":"3457","cookieFile":"/home/pi/echo-auth/alexa-cookie.json","refreshInterval":"3","alexaServiceHost":"layla.amazon.de","amazonPage":"amazon.de","acceptLanguage":"de-DE","userAgent":"","useWsMqtt":"on","autoInit":"on"},{"id":"21a58516.c9540a","type":"vsh-connection","name":"Account Eidmann","port":"8883","accessTokenExpiry":"1603486187702"}]
