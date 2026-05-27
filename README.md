# Your Project Name

| | |
|-|-|
|`Author` | Your full name

## Description

## Motivation

## Architecture

### Block diagram

<!-- Make sure the path to the picture is correct -->
![Block Diagram](schematics/block_diagram.png)

### Schematic

![Schematic](schematics/kicad_schematic.png)

### Components


<!-- This is just an example, fill in with your actual components -->

| Device | Usage | Price |
|--------|--------|-------|
| Activ Buzzer | Buzzer | [1.5 RON](https://www.optimusdigital.ro/ro/audio-buzzere/635-buzzer-activ-de-3-v.html?search_query=buzzer&results=61) |
| Push Button | Button | [1 RON](https://www.optimusdigital.ro/ro/butoane-i-comutatoare/1119-buton-6x6x6.html?search_query=buton&results=222) |
| Jumper Wires | Connecting components | [7 RON](https://www.optimusdigital.ro/ro/fire-fire-mufate/884-set-fire-tata-tata-40p-10-cm.html?search_query=set+fire&results=110) |
| Breadboard | Project board | [10 RON](https://www.optimusdigital.ro/ro/prototipare-breadboard-uri/8-breadboard-830-points.html?search_query=breadboard&results=145) |

### Libraries

<!-- This is just an example, fill in the table with your actual components -->

| Library | Description | Usage |
|---------|-------------|-------|
| [lib-name1](link-to-lib) | official description of the lib | Used for accesing the peripherals of the microcontroller  |
| [lib-name2](link-to-lib) | official description of the lib | Used for accesing the peripherals of the microcontroller  |

## Log

<!-- write every week your progress here -->

### Week 6 - 12 May

### Week 7 - 19 May

### Week 20 - 26 May


## Reference links

<!-- Fill in with appropriate links and link titles -->

[Tutorial 1](https://www.youtube.com/watch?v=wdgULBpRoXk&t=1s&ab_channel=BenEater)

[Article 1](https://www.explainthatstuff.com/induction-motors.html)

[Link title](https://projecthub.arduino.cc/)


COD:
// LEDS
const byte RED_LED   = 12;
const byte GREEN_LED = 11;
const byte BLUE_LED  = 10;
// BUTTONS
const byte RED_BTN   = A1;
const byte GREEN_BTN = A2;
const byte BLUE_BTN  = A3;
// BUZZER
const byte BUZZER = 9;

const int MAX_SEQ = 50;

byte sequence[MAX_SEQ];
int seqLength = 2;

void setup() {

  pinMode(RED_LED, OUTPUT);
  pinMode(GREEN_LED, OUTPUT);
  pinMode(BLUE_LED, OUTPUT);

  pinMode(RED_BTN, INPUT_PULLUP);
  pinMode(GREEN_BTN, INPUT_PULLUP);
  pinMode(BLUE_BTN, INPUT_PULLUP);

  pinMode(BUZZER, OUTPUT);

  randomSeed(analogRead(A1));

  newGame();
}

void loop() {

  showSequence();

  if (!repeatSequence(seqLength)) {
    gameOver();
    newGame();
    return;
  }

  successAnimation();

  // utilizatorul trebuie sa repete din nou
  // secventa si sa adauge o culoare

  if (!repeatSequence(seqLength)) {
    gameOver();
    newGame();
    return;
  }

  byte newColor = getButton();

  sequence[seqLength] = newColor;
  seqLength++;

  flashColor(newColor);

  successAnimation();

  delay(1000);
}

void newGame() {

  seqLength = 2;

  sequence[0] = random(3);
  sequence[1] = random(3);

  allOff();
}

bool repeatSequence(int length) {

  for (int i = 0; i < length; i++) {

    byte pressed = getButton();

    flashColor(pressed);

    if (pressed != sequence[i]) {
      return false;
    }
  }

  return true;
}

byte getButton() {

  while (true) {

    if (digitalRead(RED_BTN) == LOW) {
      delay(30);
      while (digitalRead(RED_BTN) == LOW);
      delay(30);
      return 0;
    }

    if (digitalRead(GREEN_BTN) == LOW) {
      delay(30);
      while (digitalRead(GREEN_BTN) == LOW);
      delay(30);
      return 1;
    }

    if (digitalRead(BLUE_BTN) == LOW) {
      delay(30);
      while (digitalRead(BLUE_BTN) == LOW);
      delay(30);
      return 2;
    }
  }
}

void showSequence() {

  delay(500);

  for (int i = 0; i < seqLength; i++) {

    flashColor(sequence[i]);

    delay(200);
  }
}

void flashColor(byte color) {

  allOff();

  switch (color) {

    case 0:
      digitalWrite(RED_LED, HIGH);
      tone(BUZZER, 440);
      break;

    case 1:
      digitalWrite(GREEN_LED, HIGH);
      tone(BUZZER, 660);
      break;

    case 2:
      digitalWrite(BLUE_LED, HIGH);
      tone(BUZZER, 880);
      break;
  }

  delay(350);

  allOff();
  noTone(BUZZER);
}

void successAnimation() {

  digitalWrite(RED_LED, HIGH);
  digitalWrite(GREEN_LED, HIGH);
  digitalWrite(BLUE_LED, HIGH);

  tone(BUZZER, 1200);

  delay(250);

  allOff();
  noTone(BUZZER);
}

void gameOver() {

  for (int i = 0; i < 5; i++) {

    digitalWrite(RED_LED, HIGH);
    digitalWrite(GREEN_LED, HIGH);
    digitalWrite(BLUE_LED, HIGH);

    tone(BUZZER, 180);

    delay(200);

    allOff();
    noTone(BUZZER);

    delay(200);
  }
}

void allOff() {

  digitalWrite(RED_LED, LOW);
  digitalWrite(GREEN_LED, LOW);
  digitalWrite(BLUE_LED, LOW);
}
