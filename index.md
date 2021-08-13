# "Third Eye" for the Visually Impaired with Image Recognition
This piece of technology for the visually impaired has object avoidance capabilites, allowing a user to switch between vibrations and sound to indicate the distance from an object ahead.  Along with the object avoidance, image recognition software will then classify the specific object or image, creating a greater understanding of the user's surroundings.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
|Kevin Taylor|Paul D. Schreiber High School|Mechanical Engineering|Incoming Senior|

![Headstone_Image](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ac/No_image_available.svg/1024px-No_image_available.svg.png)

# Materials: (EDIT)
- Elegoo Uno R3 (1)
- Ultrasonic Sensor (1)
- Buzzer (1)
- Push Button (1)
- Vibration Motor (1)
- Battery pack for 4 AA batteries (1)
- Perfboard (1)
- Resistor (1)
- Male to Female Jumper Wires (8)
- Male to Male Jumper Wires (4)
- Soldering Iron Kit (1)

# Final Milestone (EDIT)
Work in progress 

![Final_Milestone](http://www.jbhwheelchair.com/wp-content/uploads/2016/05/novideo.png)

# Fourth Milestone (EDIT)
Work in progress 

<iframe width="560" height="315" src="https://www.youtube.com/embed/Vv6weVz1yds" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Third Milestone (EDIT)
My third milestone is the first step in developing the machine learning for object detection. I used the cifar-100 dataset which contains 100 labels stemming from dinosaurs to chairs to cars to people. Because this project is dedicated to be helpful to the blind community, some of the pictues and labels involved in this dataset would not be very useful, such as dinosaurs as mentioned before. Thus, I preprocessed the dataset and made a new one incorporating only 24 labels out of the 100 label dataset. This includes only the necessary objects for blind travel safety, as well as increasing the accuracy by decreasing the amount of different labels.

<iframe width="560" height="315" src="https://www.youtube.com/embed/NXVRN5Wpwk0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Second Milestone
My second milestone was adding a battery, vibration motor, and button. Due to the addition of a battery, the system is now more portable and a user would not have to carry around a laptop in order for the sensor to work. Because of the addition of a vibration motor and button, a user now has the ability to click the button and choose whether they want the information to be given to them through vibrations or sound. This is because many people that are visually impaired may also be hearing impaired, so a buzzer would not be helpful to them. In order to set this up, one pin of the battery is connected to GND on the arduino via a breadboard that was added for the button, with the other pin connected to the Vin pin on the arduino. Next, one leg of the vibration motor was also connected to GND with its other attached to digital pin 7. The button was then attached to the breadboard via a  resistor and more wires, and it is all connected to the 3.3V pin on the arduino, as well as GND and digital pin 2. When it comes to coding, the addition of the vibration motor was somewhat simple, as it used similar logic to what I had established before with the millis() function. Then with the button it was difficult to make different actions occur after clicking as opposed to when holding the button down or not. This section of code needed many embedded if and else statements to make sure each function of my system worked perfectly. Eventually it was all completed accurately with this setup and code:

![Tinkercad_Image](https://github.com/KevinT14/KevinTaylor_BSE_Portfolio/raw/gh-pages/Screen%20Shot%202021-07-20%20at%2010.45.54%20AM.png)

# Beneath is my full Arduino code setup

Instantiating:
```c++
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
```

Setup:
```c++
void setup() {
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);

  pinMode(buzzer, OUTPUT);
  pinMode(vibrateMotor, OUTPUT);

  pinMode(button, INPUT_PULLUP);

  Serial.begin(9600);
}
```

Logic and carrying out the desired functions:
```c++
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
```
Added methods for the button:
Works by declaring a status of the button as high or low. Then by clicking the button, the status changes and it switches from using the buzzer to creating feedback with the vibration motor.
```c++
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

<iframe width="560" height="315" src="https://www.youtube.com/embed/f0e14o7PyWI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
# First Milestone


My first milestone was creating a system that beeps at a different rate depending on the distance of an object from the ultrasonic sensor. To do this, all that was needed was an ultrasonic sensor, an uno R3, a buzzer, and some jumper wires. Additionally, the Arduino IDE software was used for coding. In order to set up the materials, the VCC pin on the ultrasonic sensor had to be connected to 5V on the Uno, the Trig pin was connected to 9 on the Uno, the Echo pin was connected to 8 on the Uno, and finally, the GND pin was connected to GND on the Uno. To connect the buzzer, its positive pin was connected to another GND on the Uno, and the negative pin was assigned to number 10. Then for the code I declared the components as variables depending on what Uno pin they were attached to, declared different parts as inputs and outputs, and calculated distance depending on the time it takes for the ultrasonic ray to bounce off of an object and return. 

```c++
const int trig = 9;
const int echo = 8;

const int buzzer = 10;
const int onDuration = 500;

unsigned long lastPeriodStart;

const int vibrationMotor = 13;

int duration = 0;
int distance = 0;

void setup() {
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);

  pinMode(buzzer, OUTPUT);
  //pinMode(vibrationMotor, OUTPUT);

  Serial.begin(9600);
}
```
```c++
void loop() {
  digitalWrite(trig, HIGH); 
  delayMicroseconds(1000);
  digitalWrite(trig, LOW);
  
  duration = pulseIn(echo,HIGH);
  distance = (duration/2) / 28.5;
  Serial.println(distance);
}
```

Then with the more logic-based code in which we actually make the buzzer beep depending on distance, I ran into some problems. The biggest issue was using the delay() function to create pauses between beeps because this caused the sensor's intake to delay as well. Instead I had to redo the code using the millis() function which keeps track of how much time the program has been running. Then with some new variables and if statements I was able to make it so that the time between beeps repeated at the accurate rate.

```c++
void loop() {
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

<iframe width="560" height="315" src="https://www.youtube.com/embed/oqjgrrg4sWs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
