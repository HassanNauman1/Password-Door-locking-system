# Password-Door-locking-system
The Password-Based Door Lock System is a security project that uses an  Arduino microcontroller to control access through a 4x4 keypad. Users must  enter a correct password to unlock a servo motor that simulates a locking  mechanism. If the password is incorrect, access is denied, and a buzzer alerts  the user. The A key manually locks the door. 

#include <Servo.h> 
#include <Keypad.h> 
Servo doorLock; 
const int buzzerPin = 11; 
const String password = "1234";  // Set your password here 
String input = ""; 
bool isUnlocked = false; 
// Keypad setup 
const byte ROWS = 4; 
const byte COLS = 4; 
// ? Standard Keypad Matrix 
char keys[4][4] = { 
{'D','C','B','A'}, 
{'#','9','6','3'}, 
{'0','8','5','2'}, 
{'*','7','4','1'} 
}; 
// ? Your tested and working pin wiring 
byte rowPins[ROWS] = {5, 4, 3, 2};     // Keypad rows 
byte colPins[COLS] = {6, 9, 8, 7};     // Keypad columns 
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 
ROWS, COLS); 
void setup() { 
Serial.begin(9600); 
doorLock.attach(10);            
// Servo on pin 10 
pinMode(buzzerPin, OUTPUT);     // Buzzer on pin 11 
doorLock.write(0); // Start locked 
Serial.println("Enter password and press #"); 
} 
void loop() { 
char key = keypad.getKey(); 
if (key) { 
Serial.print(key); 
beepBuzzer(50); 
if (key == '#') { 
if (input == password) { 
if (!isUnlocked) { 
doorLock.write(90);  // Unlock 
isUnlocked = true; 
Serial.println("\nAccess Granted. Door Unlocked."); 
beepBuzzer(200); 
} else { 
Serial.println("\nAlready Unlocked."); 
} 
} else { 
Serial.println("\nIncorrect Password."); 
beepBuzzer(100); 
delay(100); 
beepBuzzer(100); 
} 
input = ""; // Reset input 
} 
else if (key == '*') { 
input = ""; // Clear input 
Serial.println("\nInput Cleared."); 
} 
else if (key == '0') { 
doorLock.write(0); // Manual lock 
isUnlocked = false; 
Serial.println("\nDoor Locked Manually."); 
beepBuzzer(200); 
} 
else { 
input += key; // Append key 
} 
} 
} 
void beepBuzzer(int duration) { 
digitalWrite(buzzerPin, HIGH); 
delay(duration); 
digitalWrite(buzzerPin, LOW);
}
