# "Third Eye" for the Visually Impaired
This will serve as a brief description of your project. Limit this to three sentences because it can become overly long at that point. This copy should draw the user in and make she/him want to read more.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
|Kevin Taylor|Paul D. Schreiber High School|Mechanical Engineering|Incoming Senior|

![Headstone_Image](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ac/No_image_available.svg/1024px-No_image_available.svg.png)
  
# Final Milestone
Work in progress 

![Final_Milestone](http://www.jbhwheelchair.com/wp-content/uploads/2016/05/novideo.png)

# Second Milestone
Work in progress

![Third_Milestone](http://www.jbhwheelchair.com/wp-content/uploads/2016/05/novideo.png)
# First Milestone
  

My first milestone was creating a system that beeps at a different rate depending on the distance of an object from the ultrasonic sensor. To do this, all that was needed was an ultrasonic sensor, an uno R3, a buzzer, and some jumper wires. Additionally, the Arduino IDE software was used for coding. In order to set up the materials, the VCC pin on the ultrasonic sensor had to be connected to 5V on the Uno, the Trig pin was connected to 9 on the Uno, the Echo pin was connected to 8 on the Uno, and finally, the Gnd pin was connected to Gnd on the Uno. To connect the buzzer, its positive pin was connected to another Gnd on the Uno, and the negative pin was assigned to number 10. Then for the code I declared the components as variables depending on what Uno pin they were attached to, declared different parts as inputs and outputs, and calculated distance depending on the time it takes for the ultrasonic ray to bounce off of an object and return. Then with the more logic-based code in which we actually make the buzzer beep depending on distance, I ran into some problems. The biggest issue was using the delay() function to create pauses between beeps because this caused the sensor's intake to delay as well. Instead I had to redo the code using the millis() function which keeps track of how much time the program has been running.  Then with some new variables and if statements I was able to make it so that the time between beeps repeated at the accurate rate.

```
My code
const int trig = 9;
const int echo = 8;

const int buzzer = 10;
const int onDuration = 500;

unsigned long lastPeriodStart;

//const int vibrationMotor = 13;

int duration = 0;
int distance = 0;

void setup() {
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);

  pinMode(buzzer, OUTPUT);
  //pinMode(vibrationMotor, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  digitalWrite(trig, HIGH); 
  delayMicroseconds(1000);
  digitalWrite(trig, LOW);
  
  duration = pulseIn(echo,HIGH);
  distance = (duration/2) / 28.5;
  Serial.println(distance);

  if (distance < 160 && distance >= 140){
    if (millis() - lastPeriodStart >= 5000){
      lastPeriodStart += 5000;
      tone(buzzer, 500, onDuration);
    }
  }

  if (distance < 140 && distance >= 120){
    if (millis() - lastPeriodStart >= 4400){
      lastPeriodStart += 4400;
      tone(buzzer, 500, onDuration);
    }
  }

  if (distance < 120 && distance >= 140){
    if (millis() - lastPeriodStart >= 3800){
      lastPeriodStart += 3800;
      tone(buzzer, 500, onDuration);
    }
  }

  if (distance < 100 && distance >= 80){
    if (millis() - lastPeriodStart >= 3200){
      lastPeriodStart += 3200;
      tone(buzzer, 500, onDuration);
    }
  }

  if (distance < 100 && distance >= 80){
    if (millis() - lastPeriodStart >= 2600){
      lastPeriodStart += 2600;
      tone(buzzer, 500, onDuration);
    }
  }

  if (distance < 80 && distance >= 60){
    if (millis() - lastPeriodStart >= 2000){
      lastPeriodStart += 2000;
      tone(buzzer, 500, onDuration);
    }
  }

  if (distance < 60 && distance >= 40){
    if (millis() - lastPeriodStart >= 1400){
      lastPeriodStart += 1400;
      tone(buzzer, 500, onDuration);
    }
  }

  if (distance < 40 && distance >= 20){
    if (millis() - lastPeriodStart >= 800){
      lastPeriodStart += 800;
      tone(buzzer, 500, onDuration);
    }
  }

  if (distance < 20){
    tone(buzzer, 500);
  }

  if (distance > 160){
    noTone(buzzer);
  }
}
```

![First_Milestone](http://www.jbhwheelchair.com/wp-content/uploads/2016/05/novideo.png)
