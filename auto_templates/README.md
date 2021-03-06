## Prime Infrastructure Configuration Templates examples

This directory contains some example python code for using configuration templates.

### Before you start
make sure you edit the contents of [pi_config.py](pi_config.py) to set the name of your PI server and the credentials for the API user.

### run_template.py
is a complete example of all of the concepts below.  You provide a template name, a device IP and parameters for the template, and it will be executed on the device.  Here is an example of the "Configure Interface" template

``` bash
$ python run_template.py -t "Configure Interface" -p '{"InterfaceName" : "g1/0/2", "A1" : "access", "StaticAccessVLan" : "8", "Description" : "API example"}' -d 10.10.8.100 
Applying template:'Configure Interface' to device:10.10.8.100
An deploy job has been successfully created. 
Waiting for Job:JobCliTemplateDeployIOSDevices01_55_57_204_AM_06_20_2016


user:api run:COMPLETED result:SUCCESS Start:2016-06-20T01:56:27.247+10:00 Stop:2016-06-20T01:56:28.274+10:00
For full job history: op/jobService/runhistory.json?jobName=JobCliTemplateDeployIOSDevices01_55_57_204_AM_06_20_2016
```

### configure_interface.py
is a script that applies the "Configure Interface" template to a device with the id of 610622.  It sets the interface "gig1/0/2" to access mode and assigns it to vlan 8.  It also updates the description.

You should change these parameters for your testing.

### get_devices.py
gets a list of devices and their ID from the PI inventory.

``` bash
$ ./get_devices.py 
Getting all devices
ID     IP address
610619 10.10.10.115
610620 10.10.7.2
610621 100.1.1.5
610622 10.10.8.100
<SNIP>
```

### get_template.py
by default will get a list of all templates and their ID.

``` bash
$ python get_template.py
541541 Configure Interface
541542 HTTP SWIM Image Upgrade Template
541543 IOS-XE Controller - Small Network
541544 Embedded Event Manager Configuration-IOS
541545 Preprovision
541546 Mobility_Agent
541547 Delete_Custom_Template
541548 Delete_Vlan
541549 Medianet - PerfMon
541550 EtherChannel
541551 RADIUS_AUTH
541552 Guided_Workflow_Convert_MA_To_M
<SNIP>
```

use the "-t <id> -s" combination to see a schema (in JSON) for the template.  You can use this in a python script to simplify the code for deploying a template
It also tells you which template variables are essential as these have a value of "required".

It can also be run with the argument '-n "Configure Interface" ' to do a search by template name.

``` bash
$ python get_template.py -t 541541 -s
{
  "cliTemplateCommand": {
    "targetDevices": {
      "targetDevice": {
        "targetDeviceID": "<DEVICEID>", 
        "variableValues": {
          "variableValue": [
            {
              "name": "Description", 
              "value": ""
            }, 
            {
              "name": "InterfaceName", 
              "value": "required"
            }, 
            {
              "name": "NativeVLan", 
              "value": ""
            }, 
            {
              "name": "StaticAccessVLan", 
              "value": ""
            }, 
            {
              "name": "TrunkAllowedVLan", 
              "value": ""
            }, 
            {
              "name": "VoiceVlan", 
              "value": ""
            }, 
            {
              "name": "spd", 
              "value": ""
            }, 
            {
              "name": "A1", 
              "value": ""
            }, 
            {
              "name": "duplexField", 
              "value": ""
            }, 
            {
              "name": "PortFast", 
              "value": ""
            }
          ]
        }
      }
    }, 
    "templateName": "Configure Interface"
  }
}


```

