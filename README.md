# Thief detecteion system (mbed 1769 + Raspberry Pi)

### Project idea

This project aims to implment a thief detection system for a room/house. The idea is to install a camera and an ultrasonic sensor on the room you want to secure. The ultrasonic points in one angle and the camera on a different one (ideally we would have multiple ultrasonic sensors and cameras, but for simplicity we just have one of each).

The ultrasonic detects a thief if he is close enough to the sensor. The camera is connected to a Raspberry Pi which is running an object detection model using tensorflow and opencv, so it detects the thief using this model. 

When a thief is detected, the alarm will sound on the speaker, a red blinking light will be turn on (a led for simplicity) and the door will be automatically closed (a motor in our case).

### Diagram
<br></br>
![This is an image](finalece.drawio.svg)
### Wiring
| **MBED1768** | **Component** | **Pin Name** | **Misc.** |
|--------------|--------------|--------------|--------------|
| p5 | uSD File System	  | SPI MOSI  | -  |
| p6  | uSD File System	  | SPI MISO  | -  |
| p7  | uSD File System	  | SPI SCLK  | - |
| p8  | uSD File System	  | CS  | -  |
| Vout  | uSD File System	  | VCC  | -  |
| Gnd  | uSD File System	  | Gnd  | -  |
| p18  | Class D Amplifier	  | in+  | - |
| -  | Class D Amplifier	  | pwr+  | 5V supply |
| GND  | Class D Amplifier	  | in-  | - |
| GND  | Class D Amplifier	  | pwr-  | - |
| -  | Class D Amplifier	  | out+  | speaker+ |
| -  | Class D Amplifier	  | out-  | speaker- |
| Vu(5V)  | HC-SR04  | Vcc  | - |
| Gnd  | HC-SR04  | Gnd  | - |
| p12  | HC-SR04  | trig  | - |
| p13  | HC-SR04  | echo  | - |
| -  | motor  | Vcc  | 5V supply (transistor) |
| Gnd  | motor  | Gnd  | - |
| p24  | LED  | Vcc  | - |
| Gnd  | LED  | Gnd  | - |
| p21  | Transistor  | base  | - |
| -  | Transistor  | emitter  | - |
| -  | Transistor  | collector  | - |

Additionally the camera is connected to Raspberry pi using the CSI interface and the mbed 1768 is connected to the Raspberry pi.

### Code
#### MBED code

The most important thing of the MBED code is the use of threads for each component

```
#include "Mutex.h"
#include "Thread.h"
#include "mbed.h"
#include "rtos.h"
#include "SDFileSystem.h"
#include "wave_player.h"
#include "ultrasonic.h"
#include <cstdio>
#include "Mutex.h"

Mutex thieve_m;
Serial  pi(USBTX, USBRX);
DigitalOut motor(p21);
DigitalOut red_led(p24);
bool thieves = 0;
DigitalOut led1(LED1);
SDFileSystem sd(p5, p6, p7, p8, "sd"); 
AnalogOut DACout(p18);
wave_player waver(&DACout);

void dist(int distance){
    
    if (distance < 700 && thieves == 0){
        thieve_m.lock();
        thieves = 1;
        Thread::wait(6000);
        thieves = 0;
        thieve_m.unlock();
    }
}
ultrasonic mu(p12, p13, .1, 1, &dist);    

void thread_Music(void const *args){
    while(true){
        if (thieves == 1){
        
            Thread::wait(500);
            FILE *wave_file;
            wave_file=fopen("/sd/music/sirena11.wav","r");
            if(wave_file==NULL) {
                printf("file open error!\n\n\r");
            }
            waver.play(wave_file);
            fclose(wave_file);  
            Thread::wait(200);   
        }
    }
}

void thread_ultrasonic(void const * args){

    mu.startUpdates();
    while(1){
        mu.checkDistance();    
        Thread::wait(50);
    }

}

void thread_motor(void const * args){
    while(1)
    {
        if (thieves == 1){
            motor = 1;
        }
        else{
            motor = 0;
        }
        Thread::wait(50);
    }
}

void thread_color(void const *args)
{
    while(true) {       
        if (thieves == 0){
            red_led = 0;      
        }
        else{
            red_led = !red_led;
        }
        Thread::wait(700);
    }
}

int main()
{
    Thread t2(thread_Music);
    Thread t3(thread_color);
    Thread t4(thread_ultrasonic);
    Thread t5(thread_motor);
    while(1){
        char val = 0;
        while(pi.readable()) {
            val = pi.getc();
            if(val == 'T' && thieves == 0){
                    thieve_m.lock();
                    thieves = 1;
                    Thread::wait(6000);
                    thieves = 0;
                    thieve_m.unlock();            
            }
        }
        Thread::wait(500);
    }
}
```

#### Rasperry pi code
As we can see the ra

### Set-up instructions

- Wire the circuit as in the images
- Connect camera to the Pi, make sure it works
- Load the code into mbed
- Create python script with the code provided for the raspberry
- Connect Pi to mbed
- Execute python script on the raspberry

### Images

<img src="pro3.jpeg" width="682" height="512">
<img src="pro1.jpeg" width="682" height="512">

### Video

https://www.youtube.com/watch?v=e2ACNT02rpE

### Conclusions + future work

In this project we have showcased the capabilities of the mbed at the time of managing multple compenents using threads and connecting to a raspeberry pi for recieving data. Adittionally we have showed how to run object detection models on the raspberri PI.

For future enhancements it would be interesting to install a camera that could rotate in all angles. Installing more ultrasonic sensors could also result in a more capable system.
Finally it would also be important to integrate all the wiring within one single box.

### Team members
Alejandro Cañada Hinojosa


