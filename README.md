# chargemaster #
Control various SkyRC, Voltcraft,.. battery charger with javascript/nodejs

Tested with SkyRC iMAX B6AC V2 and linux.

#### What works
  - charging
  - discharging
  - status info
  - read device info

#### What does not work
  - change programs
  - change system options
  - update firmeware

#### Tested devices
  - Voltcraft Ultimate 1000W (earlier fork)
  - SkyRC iMAX B6AC V2 (current version)


## Warning!
very early version! Bugs can inflame your batteries! Use at your own risk! Neither me or other contributors will take responsibility for damages.

## Dependencies
At least:
```
libusb-1.0-0-dev
```

## Usage as CLI Tool
```bash
# install
$ npm install -g chargemaster

# help
$ chargemaster --help

# example
$ sudo chargemaster --type LiIon --mode charge --cells 7 --current 0.5


```
## Usage as npm module
```
$ npm install chargemaster
```
```js
var ChargeMaster = require('chargemaster');
```
## API



#### new ChargeMaster(vendorId, deviceId)
  connects to Charger over USB. You can get the vendorId and deviceId with ```$ lsusb```
  ```js
  var cm = new ChargeMaster(0x0000, 0x0001);
  ```

#### cm.getStatus()
gets current status as ChargeInfo instance

###### Example Response
  ```js
    ChargeInfo {
      workState: 1,
      errorCode: 0,
      OutVoltage: 4008, // mV
      Current: 0, // mA
      ChargeTimer: 4, // s
      ChargeMah: 0, // mAh
      ExtTemp: 0,
      IntTemp: 22,
      Intimpedance: 0,
      CELL1: 6,
      CELL2: 5,
      CELL3: 5,
      CELL4: 6,
      CELL5: 5,
      CELL6: 5,
      CELL7: 5,
      CELL8: 5
}
  ```
#### cm.setBattType(string type)
Possible types: ```LiPo, LiIon, LiFe, LiHv, NiMH, NiCd, Pb```

#### cm.charge(Object options) ```Promise```
starts charging. Pass options as an Object
###### Options
```js
{
    cells: 1, // number of cells in series
    current: 0.1, // in A
    cellVoltage: 3.1, // in V
    endVoltage: 4.2 // in V
}
```

#### cm.discharge(Object options) ```Promise```
starts discharging
###### Options
```js
{
    cells: 1, // number of cells in series
    current: 0.1, // in A
    cellVoltage: 3.1 // in V
}
```
#### cm.stop(Object options) ```Promise```
starts discharging

####  cm.getMachineInfo() ```Promise```
  gets details about the charger
###### Example Response
```js
{
    machineId: '100069',
    coreType: '',
    upgradeType: 1,
    isEncrypted: false,
    customerId: 4,
    languageId: 0,
    softwareVersion: 1.06,
    hardwareVersion: 1,
    reserved: 0,
    mctype: 0,
    checksum: 195
}
```

#### cm.discharge(Object options) ```Promise```
starts discharging.


#### Event: "connected"
#### Event: "status"
- ```info``` object with a lot of status informations

#### Event: "started"
#### Event: "stopped"
#### Event: "finish"
#### Event: "error"
- ```error``` the error object


## Example
```js
const ChargeMaster = require('chargemaster');

const cm = new ChargeMaster(0x0000, 0x0001);

cm.on('error', (err) => {
  console.log('Error:', err);
});

cm.getMachineInfo().then( (info) => {
  console.log(`MachineId: ${info.machineId}\nFirmware: ${info.softwareVersion}`);
});

cm.setBattType('LiIon');

cm.charge({
  cells: 1,
  current: 1.0 // A
});
```

## License
####The MIT License (MIT)
Copyright (c) 2016 Andreas Langecker

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
