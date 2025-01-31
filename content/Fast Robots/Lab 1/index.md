+++
title = "Lab 1: Artemis Setup and Communication"
date = 2025-01-26
weight = 1
[taxonomies]
tags = ["Robotics", "C++", "Sensors", "Python", "Embedded Software", "Microcontroller" ]
+++

## Lab 1a

During section 1a of the lab I installed the Arduino IDE and established a wired connection to communicate with the Artemis Nano. To connect I had to select the correct board and port in the Arduino IDE. Then, to test the connection and explore the Arduino environment I completed the following assigned example sketches in the Arduino IDE:
- Basics_blink
- Apollo3_serial
- Apollo3_analogRead
- PDM_microphoneOutput

#### Blink
You can see the Artemis board flash a bright blue led
<iframe width="450" height="315" src="https://youtube.com/embed/5VB6kE0aCQg"allowfullscreen></iframe>
<figcaption>Blink test video</figcaption>

#### Serial
Here we can see the Artemis recieves the string and echos it back
<img src="/Fast%20Robots%20Media/Lab%201/Serial.png" alt="Alt text" style="display:block;">
<figcaption>Serial output test</figcaption>

#### analogRead Temperature Sensor
<iframe width="450" height="315" src="https://www.youtube.com/embed/RllC7NYdNTk"allowfullscreen></iframe>
<figcaption>Temperature sensor test</figcaption>

#### Microphone Output
From the video you can see the Artemis microphone successfully picking up the difference in sounds in the serial monitor
<iframe width="450" height="315" src="https://www.youtube.com/embed/gBEvY_Qi8gs"allowfullscreen></iframe>
<figcaption>Micrphone output test</figcaption>

## Lab 1b

#### Codebase and BLE

#### Configurations and Setup
1. I started with installing venv: `python3 -m pip install --user virtualenv`
2. I created the "FastRobots_ble" virtual environment inside my project directory: `python3 -m venv FastRobots_ble`

3. I activated the virtual environment: source `FastRobots_ble/bin/activate`

3. I downloaded the provided ble_robot_1.2 codebase into my project directory

4. It is now time to start the Jupyter server: `jupyter lab`

5. Updated the Artemis MAC Address on the Computer. Run the `ble_arduino.ino` file in the Arduino IDE and check the serial monitor the MAC address: 

<img src="/Fast Robots Media/Lab 1/MAC Address.png" alt="Alt text" style="display:block;">
<figcaption>MAC Address</figcaption>

6. Generate new UUID: run `from uuid import uuid4` and `uuid4()`. Input the generated UUID into the `#define BLE_UUID_TEST_SERVICE` line in ble_arduino.ino and into the `ble_service:` line in connections.yaml

<img src="/Fast Robots Media/Lab 1/UUID Yaml.png" alt="Alt text" style="display:block;">
<figcaption>connections.yaml</figcaption>

<img src="/Fast Robots Media/Lab 1/UUID Ble.png" alt="Alt text" style="display:block;">
<figcaption>ble_arduino.ino</figcaption>

7. Connect to the Artemis Nano via BLE

```python
# Get ArtemisBLEController object
ble = get_ble_controller()

# Connect to the Artemis Device
ble.connect()
```

<img src="/Fast Robots Media/Lab 1/BLE Connect.png" alt="Alt text" style="display:block;">
<figcaption>Successful BLE Connection</figcaption>

#### Task 1
I sent a string value from my computer to the Artemis board using the  `ECHO` command and the computer recieved and printed an augmented string

Arduino Code:

```c++ 
case ECHO:
    char char_arr[MAX_MSG_SIZE];

    // Extract the next value from the command string as a character array
    success = robot_cmd.get_next_value(char_arr);
    if (!success)
        return;

    tx_estring_value.clear();
    tx_estring_value.append("Robot is saying: ");
    tx_estring_value.append(char_arr);
    tx_estring_value.append("!!");
    
    
    tx_characteristic_string.writeValue(tx_estring_value.c_str());

    Serial.print("Sent back: ");
    Serial.println(tx_estring_value.c_str());
    
    break;
```

Jupyter Lab Code: 
```python
ble.send_command(CMD.ECHO, "Hi friend")
```

```python
l = ble.receive_string(ble.uuid['RX_STRING'])
print(l)
```

<img src="/Fast Robots Media/Lab 1/Hi Friend.png" alt="Alt text" style="display:block;">
<figcaption>ECHO Output</figcaption>


#### Task 2
I sent three floats to the Artemis board using the `SEND_THREE_FLOATS` command and extracted the three floats in the Arduino sketch

Arduino Code:
```c++
case SEND_THREE_FLOATS:
            
    float float_a, float_b, float_c;

    // Extract the next value from the command string as an integer
    success = robot_cmd.get_next_value(float_a);
    if (!success)
        return;

    // Extract the next value from the command string as an integer
    success = robot_cmd.get_next_value(float_b);
    if (!success)
        return;

    success = robot_cmd.get_next_value(float_c);
    if (!success)
        return;   

    Serial.print("Three Floats: ");
    Serial.print(float_a);
    Serial.print(", ");
    Serial.println(float_b);
    Serial.print(", ");
    Serial.println(float_c);

    break;
```

Jupyter Lab Code:
```python
ble.send_command(CMD.SEND_THREE_FLOATS, "3.14|2.22|8.89")
```
<img src="/Fast Robots Media/Lab 1/Sendthreefloats.png" alt="Alt text" style="display:block;">
<figcaption>SEND_THREE_FLOATS Output</figcaption>

#### Task 3
I added a `GET_TIME_MILLIS` which makes the robot reply write a string to the string characteristic. Note that `GET_TIME_MILLIS` had to be added to the `cmd_types.py` file.

```c++
case GET_TIME_MILLIS:
    unsigned long elapsedT = 1;
    
    elapsedT = millis();
    tx_estring_value.clear();
    tx_estring_value.append("T: ");
    String strTime = String(elapsedT);
    tx_estring_value.append(strTime.c_str());
    tx_characteristic_string.writeValue(tx_estring_value.c_str());
        
        break;
```
#### Task 4
I setup a `notification_handler` function to receive the string value from the Artemis board and, in the callback function, extract the time from the string.

```python
def notification_handler(bleuuid, byteArray):
    x = ble.bytearray_to_string(byteArray)
    print(x)
```

```python
ble.start_notify(ble.uuid['RX_STRING'], notification_handler)
print("Started notifications on RX_STRING")
```

```python
ble.send_command(CMD.GET_TIME_MILLIS, "")
```
<img src="/Fast Robots Media/Lab 1/NotiHandler.png" alt="Alt text" style="display:block;">
<figcaption>Notification Handler Output</figcaption>

#### Task 5


#### Task 6

#### Task 7


## Discussion