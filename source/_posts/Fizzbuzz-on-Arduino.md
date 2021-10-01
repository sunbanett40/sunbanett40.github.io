---
title: Fizzbuzz on Arduino
date: 2019-10-24 12:56:00
tags:
    - Arduino
    - "C++ Programming"
    - Electronics
    - Microcontroller
---

### Introduction
Inspired by Tom Scott's YouTube Video on the <a href="https://www.youtube.com/watch?v=QPZ0pIK_wsc&list=PL5ZXG50fTM7oZBZmFhJygA2419Hsg6v8H">FizzBuzz interview question</a> for programmers, which was proposed by the <a href="https://imranontech.com/2007/01/24/using-fizzbuzz-to-find-developers-who-grok-coding/">‘Imran on Tech’ blog</a>. For this project, I used the Arduino IDE again, using an Arduino Nano and a 10 segment LED display as a physical solution to Imran’s interview question. The project helped to consolidate my programme planning and Arduino coding skills that I learned during my EPQ.

### The Hardware
As I was planning on using an Arduino for my solution to the Fizzbuzz game I needed to create a circuit to surport it. As an input it had a button, and a low pass filter for debouncing, connected to a digital pin. As an output it had an LED display connected to digital pins in parrallel. The powerrails were poweered from the power and ground pins.
<p style="text-align:center;"><img src="/images/fizzbuzz/Fizzbuzz2.jpg" alt="Arduino board" style="width: 33%; height: 33%">

### The Program
In the main loop my program would constantly poll the input pin. Once the button had been pressed it would then increase the count, and loop back to 0 if it reached 100. Then the output would check if the count was a multiple of fizz or buzz and then output the flag. If it was neither then the value would be outputted as a 7 bit binary number. I decided to output it as flags and binary as these would be the most useful way to interact with larger systems if required.

```cpp
int count = 0;                                  //Defining Variables
int buttonState = 0;                            //Defining buttonState as low
int ouMin = 3, ouMax = 12;                      //Lowest & highest output pin
int fizz_val = 3, buzz_val = 5;                 //Defining values for fizz & buzz
const int Buzz = 11, Fizz = 12, buttonPin = 13; //Setting i/o pins

void setup()
{
    // put your setup code here, to run once:
    pinMode(buttonPin, INPUT);           //Set button as input
    for (int i = ouMin; i <= ouMax; i++) //Setting output pins
    {
        pinMode(i, OUTPUT);
    }
    Serial.begin(9600);
}

void loop()
{
    // put your main code here, to run repeatedly:
    buttonState = digitalRead(buttonPin); //Read the state of the button value
    if (buttonState == HIGH)
    {
        //Check if the button is pressed
        for (int i = ouMin; i <= ouMax; i++) //If so reset all output pins to 0
        {
            digitalWrite(i, LOW);
        }

        //increment count
        count++;

        //Reset count when it exceeds 100
        if (count > 100)
        {
            count = 0;
        }
    }

    //Check if value is a multiple of fizz
    if (count % fizz_val == 0)
    {
        digitalWrite(Fizz, HIGH);
    }

    //Check if value is a multiple of buzz
    if (count % buzz_val == 0)
    {
        digitalWrite(Buzz, HIGH);
    }

    //check if it is neither a multiple of fizz or buzz
    if (!((count % fizz_val == 0) || (count % buzz_val == 0)))
    {
        int out = count; //Set local out equal to global count
        int j;

        //convert to binary
        for (j = 6; j >= 0; j--)
        {
            if (out >= pow(2, j))
            {
                digitalWrite(j + 3, HIGH);
                out -= pow(2, j);
            }
        }
    }

    delay(200);
}
```

### Results
Overall I was pleased with the results of this project, the program worked and the hardware was enough to be able to interpret what was happening. 

If I were to improve the hardware I would use multiplexed seven segment displays so the output could be more easily understand. If I were to improve the program I would improve the scalability of the code so that it could be used for a larger set of values.

<p style="text-align:center;"><video style="width: 33%; height: 33%" autoplay muted>
  <source src="/images/fizzbuzz/Fizzbuzz.mp4" type="video/mp4">
This browser does not support the video tag.
</video>