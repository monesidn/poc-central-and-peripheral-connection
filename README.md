# Notifications broken after disconnection for peripheral device problem POC

This repository contains a simple POC showcasing a problem occurring when trying to work with multiple connections at once. In order for the problem to show up we need at least one inbound and one outbound connection to be up. 

This POC performs the following steps to show the problem:
1. Sets up a GATT table containing one service and one characteristic. 
    - When the characteristic is subscribed you will receive, each second, a notification containing a 4byte integer counter.
2. Connect to a BLE Heart Rate which address is hardcoded in the POC code.
2. Starts advertising using the name `BLE652-POC`
3. Await for an inbound connection
4. Once the inbound connection is established it will start sending one notification per second to the connected central device.
5. After 10 seconds the BLE Heart Rate sensor connection will be terminated. 

## POC Setup
In order to make the POC work you will need to change the hardcoded HR sensor MAC address. To do so edit the file "sensor.sb" and change the following line (line 4) to match the address of your sensor.
```
#define TARGET_ADDRESS "01d6c4313792ec"
```
You will also need a BLE652 chip running the 28.11.9.0 firmware.

You can install and run the POC using UwTerminalX.

## Expected behaviour
- After the BLE Heart Rate connection is closed notifications to the connected central device will keep working.

## Current behaviour
- After the BLE Heart Rate connection is closed notifications stops working reporting the error code 6802 - BLE_INVALID_CONN_HANDLE: The provided BLE connection handle is not valid.

