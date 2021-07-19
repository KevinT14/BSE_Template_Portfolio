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
My second milestone was adding a battery, vibration motor, and button. Due to the addition of a battery, the system is now more portable and a user would not have to carry around a laptop in order for the sensor to work. Because of the addition of a vibration motor and button, a user now has the ability to click the button and choose whether they want the information to be given to them through vibrations or sound. This is because many people that are visually impaired may also be hearing impaired, so a buzzer would not be helpful to them. In order to set this up, one pin of the battery is connected to Gnd on the arduino via a breadboard that was added for the button, with the other pin connected to the Vin pin o nthe arduino. Next, one leg of the vibration motor was also connected to Gnd with its other attached to digital pin 7. The button was attached to the breadboard then via resistors and more wires, it is all connected to the 3.3V pin on the arduino, as well as Gnd and digital pin 2. When it comes to coding, the arddition of the vibration motor was somewhat simple, as it used similar logic to what I had established before with the millis() function. Then with the button it was difficult to make different actions occur after clicking as opposed to when holding the button down or not. This section of code needed many embedded if and else statements to make sure each function of my system worked perfectly. Eventually it was all completed accurately with this code:

```
My code
const int trig = 9;
const int echo = 8;

const int button = 2;
int status = false;

const int vibrateMotor = 7;

const int buzzer = 10;
int buzzerState = 0;

unsigned long lastPeriodStart;
unsigned long vibrateStart;

int duration = 0;
int distance = 0;

void setup() {
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);

  pinMode(buzzer, OUTPUT);
  pinMode(vibrateMotor, OUTPUT);

  pinMode(button, INPUT_PULLUP);

  Serial.begin(9600);
}

void loop() {
  digitalWrite(trig, HIGH);
  delayMicroseconds(1000);
  digitalWrite(trig, LOW);

  duration = pulseIn(echo, HIGH);
  distance = (duration / 2) / 28.5;
  Serial.println(distance);

  if (distance < 160 && distance >= 140) {
    if (millis() - lastPeriodStart >= 5000) {
      lastPeriodStart += 5000;
    }
  }

  if (distance < 140 && distance >= 120) {
    if (millis() - lastPeriodStart >= 4400) {
      lastPeriodStart += 4400;
    }
  }

  if (distance < 120 && distance >= 140) {
    if (millis() - lastPeriodStart >= 3800) {
      lastPeriodStart += 3800;
    }
  }

  if (distance < 100 && distance >= 80) {
    if (millis() - lastPeriodStart >= 3200) {
      lastPeriodStart += 3200;
    }
  }

  if (distance < 100 && distance >= 80) {
    if (millis() - lastPeriodStart >= 2600) {
      lastPeriodStart += 2600;
    }
  }

  if (distance < 80 && distance >= 60) {
    if (millis() - lastPeriodStart >= 2000) {
      lastPeriodStart += 2000;
    }
  }

  if (distance < 60 && distance >= 40) {
    if (millis() - lastPeriodStart >= 1400) {
      lastPeriodStart += 1400;
    }
  }

  if (distance < 40 && distance >= 20) {
    if (millis() - lastPeriodStart >= 800) {
      lastPeriodStart += 800;
    }
  }

  if (distance > 160) {
    noTone(buzzer);
    digitalWrite(vibrateMotor, LOW);
  }

  if (digitalRead(button) == HIGH) {
    delay(50);
    status = !status;
  }
  if (status == true) {
    noTone(buzzer);
    if (millis() - lastPeriodStart <= 500 && distance >= 20) {
      digitalWrite(vibrateMotor, HIGH);
    }
    else if (distance < 20) {
      digitalWrite(vibrateMotor, HIGH);
    }
    else {
      digitalWrite(vibrateMotor, LOW);
    }
  }
  else if (status == false) {
    digitalWrite(vibrateMotor, LOW);
    if (millis() - lastPeriodStart <= 500 && distance >= 20) {
      tone(buzzer, 500);
    }
    else if (distance < 20) {
      tone(buzzer, 500);
    }
    else {
      noTone(buzzer);
    }
  } while (digitalRead(button) == HIGH); {
    delay(50);
  }
}
```

![Second_Milestone](http://www.jbhwheelchair.com/wp-content/uploads/2016/05/novideo.png)
# First Milestone
  

My first milestone was creating a system that beeps at a different rate depending on the distance of an object from the ultrasonic sensor. To do this, all that was needed was an ultrasonic sensor, an uno R3, a buzzer, and some jumper wires. Additionally, the Arduino IDE software was used for coding. In order to set up the materials, the VCC pin on the ultrasonic sensor had to be connected to 5V on the Uno, the Trig pin was connected to 9 on the Uno, the Echo pin was connected to 8 on the Uno, and finally, the Gnd pin was connected to Gnd on the Uno. To connect the buzzer, its positive pin was connected to another Gnd on the Uno, and the negative pin was assigned to number 10. Then for the code I declared the components as variables depending on what Uno pin they were attached to, declared different parts as inputs and outputs, and calculated distance depending on the time it takes for the ultrasonic ray to bounce off of an object and return. Then with the more logic-based code in which we actually make the buzzer beep depending on distance, I ran into some problems. The biggest issue was using the delay() function to create pauses between beeps because this caused the sensor's intake to delay as well. Instead I had to redo the code using the millis() function which keeps track of how much time the program has been running. Then with some new variables and if statements I was able to make it so that the time between beeps repeated at the accurate rate.

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

[![First Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1626441431/video_to_markdown/images/youtube--oqjgrrg4sWs-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=oqjgrrg4sWs "First Milestone"){:target="_blank" rel="noopener"}
