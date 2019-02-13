# TPush
Arduino debounce system + time management library

# How To Use

- Add TPush to your [Arduino IDE libraries](https://www.arduino.cc/en/Guide/Libraries)
- Include the library `#include <TPush.h>`
- Initialize a component `TPush ButtonX;`


## Click
```C++
#include <TPush.h>
TPush Button1;

void setup() {
    // Button1 is active LOW
    Button1.setUp(2, LOW);
    
    Serial.begin(9600);
}

void loop() {

    // minimum active time of 150ms. 
    if (Button1.Click(150)) {
        /** this is triggered once till release **/
        Serial.println("Button1 Pressed!");
    }

    // second param means another index
    if (Button1.Click(500, 2)) {
        Serial.println("Button1 Action 2 Pressed!");
    }
}
```
Default wait-time is 10 milliseconds, enough for **membranes**, use 150ms for **lamella** buttons.

Click is activated on the activation fron, if you need the release frond see the Keep function, which has a double use.

## Double Click

```C++
void loop() {
    if (Button1.DoubleClick()) {
        Serial.println("Button1 Double Click!");
    }
}
```

## Keep

```C++
int t = 0;
void loop() {
    if (t = Button1.KeepHold(1000)) { // if ((t = Button1.KeepHold(1000)) > x)
        Serial.print("Button1 pressed for ");
        Serial.print(t);
        Serial.println("ms");
    }
}
```

Here ```t``` is set each frame
```C++
if (t = Button1.KeepHold(100, true) > 1000) {
```

## Timing

```C++
#include <TPush.h>
TPush TIMER;

void setup() {
    // Nothing to do here
}

void loop() {
    if (TIMER.Wait(150)) {
        Serial.println("Once every 150ms");
    }

    if (TIMER.Wait(500, 2)) {
        Serial.println("Use the second parameter for multiple separate timers, on the same object");
    }
}

```
NB: to avoid waste of memory you can use Wait() on Buttons already declared instead of declaring timer objects. Use the second parameter as arrays index.


Avoid floating state useing a [pull resistor](https://en.wikipedia.org/wiki/Pull-up_resistor).

If the buttons is ACTIVE LOW you can use internal pull-up resistance (not on pin 13). [See documentation](https://www.arduino.cc/en/Tutorial/DigitalPins).
