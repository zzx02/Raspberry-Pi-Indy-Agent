 # Raspberry Pi Interaction RFC
 
 https://github.com/hyperledger/aries-rfcs
 Type: features - RFCs that describe individual features (the features folder)
 
 ## Summary 
 
 The raspberry pi interaction message family describe how to interact with the Pi peripherals between agents.
 
 
 ## Motivation
 
 Enables agents to interact with other agents that can be reflected on external hardware, such as senseHAT.
 
 ## Tutorials
 
 ### Roles 
 
  There are two roles, sender and receiver.
  The Sender is the agent interacting with the IoT agent by IoT messages discribed below. The Receiver is the IoT agent who responds to a Senders messages.
  Proposed are two types of IoT messages, request and response, designed to fullfil the needs of those two roles, .
* IoT request messages originate from senders to IoT agents. Those messages can be 'read' or 'write' requests.
    * 'read' requests, are used to read state, for example they can initiate a read response to get data like temperature, pressure, humidity, etc.
    * 'write' requests, are used to set state, for example they can set state to display a specific text, color, or image on IoT agents LED matrix.
* IoT response messages originate from IoT agents in response to sender requests. Those messages can be in response to 'read' or 'write' requests.
    * 'read' response returns the desired state, as mention above, the temperature, pressure, humidity, etc.
    * 'write' response returns the agents state status.

(Better practice of the message design?)
     
     
### Messages

Describe the message structure here, need to include some necessary information:
An example message of existing [RFC 0095 Basic Message Protocol](https://github.com/hyperledger/aries-rfcs/tree/master/features/0095-basic-message) as below, here we should make some modification to define other fields including sufficient infomation for Pi agents to interact. We can also refer to [RFC 0113 Question Answering Protocol](https://github.com/hyperledger/aries-rfcs/tree/master/features/0113-question-answer) and [RFC 0048 Trust Ping Protocol](https://github.com/hyperledger/aries-rfcs/tree/master/features/0048-trust-ping)

    read_sensors
    {
        "@type": "did:sov:BzCbsNYhMrjHiqZDTUASHg;spec/raspberrypi_interactions/1.0/read_sensor",
        "@id": "518be002-de8e-456e-b3d5-8fe472477a86",
        "sensors" :  ["temperature", 
                  "pressure", 
                  "humidity",
                  "orientation", 
                  "accelerometer", 
                  "compass", 
                  "gyroscope",
                  "stick_events",
                  "pixels"]
    }
    
	sensor_values
	{
  	    "@type": "did:sov:BzCbsNYhMrjHiqZDTUASHg;spec/raspberrypi_interactions/1.0/sensor_values",
  	    "@id": "518be002-de8e-456e-b3d5-8fe472477a86",
  	    "temperature" : 40.4807395935,
  	    "pressure" : 1002.99536133,
  	    "humidity" : 44.4767837524,
  	    "orientation" : {"pitch" : 1.021595342861921,
                         "roll" : 1.1890620830788192,
                         "yaw" : 212.09070990062145},
  	    "accelerometer" : {"x" : -0.018019001930952072,
                           "y" : 0.021122954785823822,
                           "z" : 0.9811235070228577},
  	    "compass" : {"x" : -92.36841583251953,
                     "y" : 56.72425079345703,
                     "z" : -124.01837921142578},
  	    "gyroscope" : {"x" : 0.010323487222194672,
                       "y" : 0.0028627216815948486,
                       "z" : 0.000046096742153167725},
  	    "stick_events" : [{"timestamp" : 1565274109.343338, "direction" : "down", "action" : "pressed"}],
  	    "pixels" : [[0,0,0],...]
	}

	display_letter
	{
  		"@type": "did:sov:BzCbsNYhMrjHiqZDTUASHg;spec/raspberrypi_interactions/1.0/display_letter",
  		"@id": "518be002-de8e-456e-b3d5-8fe472477a86",
  		"letter": "A",
  		"text_colour": [255,255,0],
            "back_colour": [0,0,0]
	}

	display_message
	{
  		"@type": "did:sov:BzCbsNYhMrjHiqZDTUASHg;spec/raspberrypi_interactions/1.0/display_message",
  		"@id": "518be002-de8e-456e-b3d5-8fe472477a86",
  		"content": "Hello world!",
  		"text_colour": [255,255,0], 
  		"back_colour": [0,0,0],
  		"scroll_speed": 0.1
	}

	set_pixels
	{
  		"@type": "did:sov:BzCbsNYhMrjHiqZDTUASHg;spec/raspberrypi_interactions/1.0/display_message",
  		"@id": "518be002-de8e-456e-b3d5-8fe472477a86",
  		"pixels": [[0,0,0] * 64]
	}
    
    
    
#### Read Sensors

ReadSensor Message act as a "read" request, it consists of a list of sensors under the field "sensors", currently, it supports 9 different types:
* Temperature: returns the current temperature in degrees Celsius from the humidity sensor.
* Pressure: returns the current pressure in Millibars from the pressure sensor.
* Humidity: returns the percentage of relative humidity from the humidity sensor.
* Orientation: returns the current orientation in degrees using the aircraft principal axes of pitch, roll and yaw.
* Accerlerometer: returns the raw x, y and z axis accelerometer data.
* Compass: returns the raw x, y and z axis magnetometer data.
* Gyroscope: returns the raw x, y and z axis gyroscope data.
* stick_event: returns a list of tuples representing all events that have occurred since the last call.
* pixels: returns a list containing 64 smaller lists of [R, G, B] pixels (red, green, blue) representing the currently displayed image.

It is expected that an agent will reply a SensorValue message after received the ReadSensor message

#### Sensor Values

According to the read_sensor request, it may consist of different fields depending on what "read" request message specifically desires.

It has sensor values that "read" request asks for, depending on the contents within "sensors", it has different fields. 

#### Display Letter

It is a "write" request that is going to change the status of the LED matrix on sensehat, for displaying a single letter with the specified text colour and background colour, clearing all previous status on LED matrix. It has following fields:

* letter: A text string which length=1
* text_colour(optional): A list containing the R-G-B (red, green, blue) colour of the letter. Each R-G-B element must be an integer between 0 and 255. Defaults to [255, 255, 255] white.
* back_colour(optional): A list containing the R-G-B (red, green, blue) colour of the background. Each R-G-B element must be an integer between 0 and 255. Defaults to [0, 0, 0] black / off.

As a reply to this message, it is a [RFC 0095 Basic Message](https://github.com/hyperledger/aries-rfcs/tree/master/features/0095-basic-message), stating "DisplayLetter message received".

#### Display Message

It is a "write" request that is going to change the status of the LED matrix on sensehat, for displaying a text message with the specified text colour and background colour, clearing all previous status on LED matrix. It has following fields:

* content: The message that need to display on the LED matrix.
* text_colour(optional): A list containing the R-G-B (red, green, blue) colour of the letter. Each R-G-B element must be an integer between 0 and 255. Defaults to [255, 255, 255] white.
* back_colour(optional): A list containing the R-G-B (red, green, blue) colour of the background. Each R-G-B element must be an integer between 0 and 255. Defaults to [0, 0, 0] black / off.

As a reply to this message, it is a [RFC 0095 Basic Message](https://github.com/hyperledger/aries-rfcs/tree/master/features/0095-basic-message), stating "DisplayMessage message received".

#### Set Pixels

It is a "write" request that updates the entire LED matrix based on a 64 length list of pixel values, clearing all previous status on LED matrix. It has following fields:

* pixels: A list containing 64 smaller lists of [R, G, B] pixels (red, green, blue). Each R-G-B element must be an integer between 0 and 255.

As a reply to this message, it is a [RFC 0095 Basic Message](https://github.com/hyperledger/aries-rfcs/tree/master/features/0095-basic-message), stating "SetPixels message received".

