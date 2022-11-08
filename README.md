# Multiple connections problem POC

This repository contains a simple POC showcasing a problem occurring when trying to work with multiple connections at once. In order for the problem to show up we need at least one inbound and one outbound connection to be up. 

This POC performs the following steps to show the problem:
1. Sets up a GATT table containing one service and one characteristic. 
    - When the characteristic is subscribed you will receive, each second, a notification containing a 4byte integer counter.
2. Starts advertising using the name `BLE652-POC`
3. Await for an inbound connection
4. Once the inbound connection is established it will wait 5 seconds
5. After 5 seconds it will try to connect to a BLE Heart Rate sensor available at an hard-coded address. 

## POC Setup
In order to make the POC work you will need to change the hardcoded HR sensor MAC address. To do so edit the file "sensor.sb" and change the following line (line 4) to match the address of your sensor.
```
#define TARGET_ADDRESS "01d6c4313792ec"
```
You will also need a BLE652 chip running the 28.11.8.0 firmware.

You can install and run the POC using UwTerminalX.

## Expected behaviour
- if the notification, for the only exposed characteristic, is enabled it will keep working after the connection.

## Current behaviour
- When the connection to the sensor is established an event is dispatched in the SmartBasic runtime telling the program that the notification has been disabled (CCCD set to 0). 
    - This can be replicated by subscribing to notifications during the 5 seconds wait.
- If the notification is disabled and then re-enabled the call to `BleCharValueNotify` will fail with error `6208`. 
    - Can be replicated by disabling and re-enabling the notification on the characteristic

