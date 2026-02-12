#include <Servo.h>

//change wait time prescaler here
const long prescaler = 60000; //in miliseconds
//1000 * 60 * 1 = 60000
//for 1-15 minutes interval (1, 2, 4, 8 min)
//1000 * 60 * 15 = 900000
//for 15 min ~ 3.75 hours interval (15, 30, 60, 120 min)
//1000 * 60 * 5 = 300000
//for 5 min ~ 75 min interval (5, 10, 20, 40 min)

//activate servo control
Servo myservo;

//declare input pin for timeSet
byte timeSetPin = 12;

//declare output pin for timeMark (widely configurable)
byte _1minMark = 8;
byte _2minMark = 9;
byte _4minMark = 10;
byte _8minMark = 11;
byte timeSetInput = 1;

//to avoid button command executed multiple time
byte buttonActivated = false;

//timestamp marking
long now = 0;
long rollTimeMark = 0;

//timeToRoll counter (wait time)
long timeToRoll = 0;

void setup()
{
  //declare timeMark pin as OUTPUT
  pinMode(_1minMark, OUTPUT);
  pinMode(_2minMark, OUTPUT);
  pinMode(_4minMark, OUTPUT);
  pinMode(_8minMark, OUTPUT);
  
  digitalWrite(_1minMark, HIGH);
  
  timeToRoll = (long)timeSetInput * prescaler;

  //operation indicator LED (OUTPUT)
  pinMode(13, OUTPUT);
  
  //prepare for timeSet pushbutton as INPUT
  pinMode(timeSetPin, INPUT);
  
  //attach pin3 for servo control
  myservo.attach(3);
  myservo.write(0);
  
  rollTimeMark = millis();
  
  Serial.begin(9600);
  
  Serial.print("timeToRoll: ");Serial.println(timeToRoll);
  Serial.print("Prescaler: ");Serial.println(prescaler);
}

void loop()
{
  //always check timeset input first
  if (buttonActivated && now % 400 <= 5)
  {
    buttonActivated = false;
  }
  
  if (digitalRead(timeSetPin) && !buttonActivated)
  {
    timeSetInput = timeSetInput + 1;
    //limit timeSetInput max value to 15
    //if you use more than 4 time indicator, this value
    //gonna need to increased
    if (timeSetInput > 15) timeSetInput = 1;
    
    digitalWrite(_1minMark, LOW);
    digitalWrite(_2minMark, LOW);
    digitalWrite(_4minMark, LOW);
    digitalWrite(_8minMark, LOW);
    
    bool _bit0, _bit1, _bit2, _bit3;
    //check LSB to MSB value out of timeSetinput variable
    //we only need first 4 bits 
    //because we only have 4 LED :)
    _bit0 = bitRead(timeSetInput, 0);
    _bit1 = bitRead(timeSetInput, 1);
    _bit2 = bitRead(timeSetInput, 2);
    _bit3 = bitRead(timeSetInput, 3);
    
    if (_bit0) digitalWrite(_1minMark, HIGH);
    if (_bit1) digitalWrite(_2minMark, HIGH);
    if (_bit2) digitalWrite(_4minMark, HIGH);
    if (_bit3) digitalWrite(_8minMark, HIGH);
    
    //Calculate wait time for next roll
    timeToRoll = (long)timeSetInput * prescaler;
    
    buttonActivated = true;
    
    //for debug purpose
    Serial.println(timeSetInput);
    Serial.print("timeToRoll: ");Serial.println(timeToRoll);
  }
    
  //roll the servo
  if (now - rollTimeMark > timeToRoll)
  {
    myservo.write(180);
    delay(2000);
    myservo.write(0);
    
    //save current roll time for next roll
    rollTimeMark = millis();
    
    //for debug purpose
    Serial.println("roll!");
    Serial.println(timeToRoll);
  }
  
  //update current time
  now = millis();
  
  //blinking onboard LED - for show off purpose, not important
  if (now % 1000 <= 5 && !digitalRead(13))
  {
    digitalWrite(13, HIGH);
  }
  else if (digitalRead(13) && now % 1000 >= 150)
  {
    digitalWrite(13, LOW);
  } // */
}
