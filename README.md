# Thief detecteion system (mbed 1769 + Raspberry Pi)

### Project idea

This project aims to implment a thief detection system for a room/house. The idea is to install a camera and an ultrasonic sensor on the room you want to secure. The ultrasonic points in one angle and the camera on a different one (ideally we would have multiple ultrasonic sensors and cameras, but for simplicity we just have one of each).

The ultrasonic detects a thief if he is close enough to the sensor. The camera is connected to a Raspberry Pi which is running an object detection model using tensorflow and opencv, so it detects the thief using this model. 

When a thief is detected, the alarm will sound on the speaker, a red blinking light will be turn on (a led for simplicity) and the door will be automatically closed (a motor in our case).

### Diagram
<br></br>
![This is an image](finalece.drawio.svg)
### Wiring
Lorem ipsum dolor sit amet consectetur adipisicing elit. Maxime mollitia,
molestiae quas vel sint commodi repudiandae consequuntur voluptatum laborum
numquam blanditiis harum quisquam eius sed odit fugiat iusto fuga praesentium
optio, eaque rerum! Provident similique accusantium nemo autem. Veritatis
obcaecati tenetur iure eius earum ut molestias architecto voluptate aliquam
nihil, eveniet aliquid culpa officia aut! Impedit sit sunt quaerat, odit,
tenetur error, harum nesciunt ipsum debitis quas aliquid. Reprehenderit,
quia. Quo neque error repudiandae fuga? Ipsa laudantium molestias eos 
sapiente officiis modi at sunt excepturi expedita sint? Sed quibusdam
recusandae alias error harum maxime adipisci amet laborum. Perspiciatis
### Code
Lorem ipsum dolor sit amet consectetur adipisicing elit. Maxime mollitia,
molestiae quas vel sint commodi repudiandae consequuntur voluptatum laborum
numquam blanditiis harum quisquam eius sed odit fugiat iusto fuga praesentium
optio, eaque rerum! Provident similique accusantium nemo autem. Veritatis
obcaecati tenetur iure eius earum ut molestias architecto voluptate aliquam
nihil, eveniet aliquid culpa officia aut! Impedit sit sunt quaerat, odit,
tenetur error, harum nesciunt ipsum debitis quas aliquid. Reprehenderit,
quia. Quo neque error repudiandae fuga? Ipsa laudantium molestias eos 
sapiente officiis modi at sunt excepturi expedita sint? Sed quibusdam
recusandae alias error harum maxime adipisci amet laborum. Perspiciatis
### Instructions
### HW setup
### Images

<img src="pro3.jpeg" width="682" height="512">
<img src="pro1.jpeg" width="682" height="512">

### Video

https://www.youtube.com/watch?v=e2ACNT02rpE

# this is atest

hello
