---
title: Fizzbuzz on Arduino
date: 2019-10-24 12:56:00
tags:
    - Arduino
    - Electonics
    - Microcontroller
---


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