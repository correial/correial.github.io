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

#### Codebase and BLE Discussion

Bluetooth (specifically Bluetooth LE) at a high level is used to establish a connection between my computer and the Artemis: 

- Bluetooth LE radio acts like a community bulletin board where Computers (community members) can connect to read the  board. If the radio is a bulletin board we call that a peripheral device and it is responsible for posting data. If the radio is a reader (central device) it reads from any of the bulletin boards. Essentially, central devices view the services, get the data, then move on, all within a few milliseconds allowing multiple central devices can get data from one peripheral.

- Services are identified by unique numbers known as UUIDs. The ability to define services and characteristics depends on the radio you're using and its firmware. Bluetooth LE peripherals will provide services, which in turn provide characteristics.

- There are four things that a central device can do with a characteristic: `Read`, `Write`, `Indicate`, and `Notify`

The codebase is collection of source code files that make up our system. Some important components:

- The Artemis' unique UUID and Mac address allow for undisrupted communication and transmission of BLExCharacteristics

- ble_arduino.ino is the code running on the Artemis, edited in the Arduino IDE

-  EString allows us to easily manipulate and send character arrays

- Demo.ipynb is where you find the python code sending commands to the Artemis

- Some relevant functions used to communicate between the computer and Artemis:
    - `send_command(cmd_type, data)`to send a command
    - `ble.connect()` and `ble.disconnect` to connect with the Artemis
    - `receive_string(uuid)` to recieve a string from our board
    - `start_notify(uuid, notification_handler)` to activate the notification handler
    -  `ble.bytearray_to_string(byteArray)` to convert recieved data into string

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
I made a twenty-five step loop that gets the current time in milliseconds using the `GET_TIME_MILLIS` function to then be processed by `notification_handler()`. From my output shown below you can see that there was an average 33.5 ms gap between the prints. This translates to 29.85 message transmissions per second. With each message being 9 bytes, this results an effective data transfer rate of **269 bytes per second** for this method.

```python
x = 0
while x < 25:
    k = ble.send_command(CMD.GET_TIME_MILLIS, "")
    x+=1
```

<img src="/Fast Robots Media/Lab 1/GetTimeMillisLoop.png" alt="Alt text" style="display:block;" height="300">
<figcaption>GET_TIME_MILLIS Loop Output</figcaption>

#### Task 6
I created a command `SEND_TIME_DATA` that loops though to add generated time steps via the `millis()` function and then stores them in an array. Then, in `SEND_TIME_DATA` I loop through the array and send each data point as a string to my laptop to be processed.

Arduino Code
```c++
case SEND_TIME_DATA: {

    int Millis_Cur = 0;
    
    if (!success) {
        return;
    }
    
    for (int i = 0; i < 24; i++){ 
    Millis_Cur = millis();
    millisArray[i] = Millis_Cur;
    }

    for (int x = 0; x < 24; x++){ 
    tx_estring_value.clear();
    tx_estring_value.append("T: ");
    tx_estring_value.append(millisArray[x]);
    tx_characteristic_string.writeValue(tx_estring_value.c_str());
    }

break;
} 
```

Jupyter Code:


```python
ble.send_command(CMD.SEND_TIME_DATA, "")
```


<img src="/Fast Robots Media/Lab 1/SendTimeData.png" alt="Alt text" style="display:block;" height="300">
<figcaption>SEND_TIME_DATA Output</figcaption>

#### Task 7
I created a second array to store fahrenheit temperatures readings with the same length as the one used in task 6. Each element in both arrays (`millisArray[]` and `tempArray[]`) correspond to each other. The command `GET_TEMP_READINGS` loops through both arrays concurrently and sends each temperature reading with a time stamp. The notification handler parses these strings and add populate the data into two lists. 

```c++
case GET_TEMP_READINGS: {
            
    int Millis_Cur2 = 0;
    float temp_f_cur = 0;
    
    if (!success) {
        return;
    }
    
    for (int i = 0; i < 24; i++){ 
    Millis_Cur2 = millis();
    float temp_f_cur = getTempDegF();
    millisArray[i] = Millis_Cur2;
    tempArray[i] = temp_f_cur;
    }
    
    for (int x = 0; x < 24; x++){ 
    tx_estring_value.clear();
    tx_estring_value.append("T: ");
    tx_estring_value.append(millisArray[x]);
    tx_estring_value.append(" , ");
    tx_estring_value.append("F: ");
    tx_estring_value.append(tempArray[x]);
    tx_characteristic_string.writeValue(tx_estring_value.c_str());
    }
    
break; 
}
```

<img src="/Fast Robots Media/Lab 1/MillisandTemp.png" alt="Alt text" style="display:block;" height="300">
<figcaption>Temp and Time Output</figcaption>

#### Task 8



### Discussion and Conclusion (Lab 1A & 1B)

1. Learned about what commands are responsible for communication between my computer and Artemis
2. At first I was fairly confused about the relationship between different data types and their byte size. However, the later questions in the lab taught me the bytes that correspond to these data values and the most efficient way to send them to the computer depending on the task
3. The largest problem I faced was understanding the parameters needed for the notification handler!