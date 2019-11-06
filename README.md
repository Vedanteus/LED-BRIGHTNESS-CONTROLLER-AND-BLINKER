# LED-BRIGHTNESS-CONTROLLER-AND-BLINKER
Writing Node.js program for Blinking LED:

To write a Node.js program open any text editor (Notepad, Notepad++, etc.) and paste the ‘blink_led’ code attached at the end of this tutorial and save it with an extension of ‘.js’ i.e. (blink_led.js) in the “LED_Control” folder created before. We will discuss important steps in blink_led.js code file.

Initially define the Pin of microcontroller where led is connected. In this example, the LED is connected to Pin 5 of Arduino UNO. The ‘var’ in Node.js represents variable declaration.

var led_pin=5;
 
The johnny-five module needs to be included and the board needs to be selected. The modules in Node.js are libraries. The function ‘require()’ will access the module.

var johnny_five=require("johnny-five"); 
var arduino_board=new johnny_five.Board(); 

The console.log statement is similar to print statement and it will print message. And the LED pin is set to output mode and the defined delay is given to blink led.

console.log("LED has Started Blinking!");  
var led = new johnny_five.Led(led_pin);  
led.blink(100); 

Writing Node.js program to control Brightness of LED:

Next step would be writing the “brightness_control.js” sketch using Node.js and executing the code. The execution of the code will be bit similar to the Blinking an LED with Node.js.

To write a Node.js program open any text editor (Notepad, Notepad++, etc.) and paste the ‘brightness_control” code attached at the end of this tutorial and save it with an extension of ‘.js’ i.e. (brightness_control.js) in the “LED_Control” folder created before. You can also download the code from here.

Similar to Blink Led Node.js program, this section will also use modules (library). Include the ‘express’, ‘http’ and ‘serial port’ module.

var express = require('express');
app = express();
server = require('http').createServer(app);
io = require('socket.io').listen(server);
var SerialPort = require("serialport")//.SerialPort
 
Now set the COM port and baudrate. Note that in windows, it will always be COM with extension of number (COM6, COM4, COM24 etc.), so set below as required after ‘/’. Also set buadrate.

var serialPort = new SerialPort("/COM4", { baudRate: 9600 });
 
Start to listen the server at port 8080.

server.listen(8080);
 
Set the brightness at 0 initially. Then latch the brightness data to IO with sockets module, which is a websocket module. The data will receive by Web Interface using socket protocol.

io.sockets.on('connection', function (socket) {
socket.on('led', function (data) {
brightness = data.value;
var buf = new Buffer(1);
buf.writeUInt8(brightness, 0);
serialPort.write(buf);
 
Now emit the LED brightness value got from socket to LED pin.

io.sockets.emit('led', {value: brightness});   
});
socket.emit('led', {value: brightness});
});
 
Now just to debug, add a print statement at the end. Console.log is a print statement in Node.js.

console.log("Web Server Started go to 'http://localhost:8080' in your Browser.");

Setting up Arduino UNO to control Brightness of LED: 

To set up the Arduino UNO, simply upload the sketch “arduino_control.ino” into Arduino UNO board and that’s it. This will set up the Arduino Board. You can download the complete code with HTML files from here. The “arduino_control.ino” code has following important steps involved.

Initially the baud rate is set at 9600.

Serial.begin(9600);
 
The serial port always looks for incoming byte and the byte is written to Pin 5 which is a PWM Pin.

while(!Serial.available());
analogWrite(5, Serial.read());
 
That’s it. This will set the Arduino to latch the byte to PWM pin and will change the brightness of LED.


Setting up Web Interface: 

To control the brightness of led using web interface, one small piece of HTML code is written to have an interface in the Browser. To have interface follow simple steps below:
Create a new folder named “public” inside the “LED_Control” folder created before.

Now download the “index.html” and “style.css” files and move both files inside the “public” folder created in first step above. Files can be downloaded from here.

This will create a slider on webpage to control the brightness of LED using Node.js and Arduino.

Controlling an LED from webpage is interesting to learn and it can done with other microcontroller by creating a webserver and host the webpage on webserver. Check all the webserver related projects here.
