#define RXD2 4
#define TXD2 2
#define PIN 5
#define RES 19

void setup() {
  // Initialize Serial and Serial1 ports
  Serial.begin(9600);
  Serial1.begin(9600);
  Serial2.begin(9600, SERIAL_8N1, RXD2, TXD2);

  // Set pin modes
  pinMode(PIN, OUTPUT);
  pinMode(RES, OUTPUT);
  pinMode(35, INPUT); // Digital Input 1
  pinMode(34, INPUT); // Digital Input 2
  pinMode(13, OUTPUT); // RS-485 flow control

  // Initialize digital outputs
  digitalWrite(RES, HIGH);
  digitalWrite(PIN, HIGH);
  digitalWrite(13, HIGH); // RS-485

  Serial.println("Setup completed");
}

void loop() {
  // Read from Serial2 (RXD2), send to Serial (USB)
  while (Serial2.available()) {
    int inByte = Serial2.read();
    Serial.write(inByte);
  }

  // Read from Serial (USB), send to Serial2 (TXD2)
  while (Serial.available()) {
    int inByte = Serial.read();
    Serial2.write(inByte);
  }

  // Read and print digital inputs
  Serial.print("I1: ");
  Serial.println(digitalRead(35));
  Serial.print("I2: ");
  Serial.println(digitalRead(34));

  // Delay and send test message on Serial1 (RS-485)
  delay(800);
  Serial1.println("RS-485 TX TEST");
}
