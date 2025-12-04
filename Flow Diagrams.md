# Flow Diagrams

---

## 🧠 High-Level System Flow

```mermaid
flowchart LR

subgraph INPUTS[User Inputs]
  BTN[Push Button\n(Manual Mode)]
  APP[Smartphone App\n(Bluetooth Mode)]
end

APP --> BT[HC-05 Module\nBluetooth to Serial]
BTN --> ARD[Arduino Firmware\n(State Machine)]

BT --> ARD

ARD --> PWM[PWM Signal\nGeneration]
PWM --> SERVOS[Finger Servos\n(Thumb–Little)]
SERVOS --> HAND[3D-Printed Hand\nTendon-Driven Fingers]

ARD --> LED[Status LEDs\n(Red / Blue)]
ARD --> STATE[Software State\n& Position Tracking]
STATE --> APP:::optional

classDef optional fill:#f5f5f5,stroke:#999,stroke-dasharray: 3 3;



flowchart TD

A1[User sends command\nfrom Smartphone App] --> B1[HC-05 receives\nBluetooth packet]
B1 --> C1[HC-05 forwards data\nas Serial to Arduino RX]
C1 --> D1[Arduino reads\nSerial buffer]
D1 --> E1[Parse command &\nmatch with valid actions]
E1 --> F1[Update state machine:\nSelected finger or gesture]
F1 --> G1[Generate target angles\nfor selected servos]
G1 --> H1[Drive servos via PWM\nSmooth motion profile]
H1 --> I1[Update position\nand state variables]
I1 --> J1[Send status/ACK\nback to Bluetooth module]
J1 --> K1[User sees confirmation\nin mobile app UI]



flowchart TD

A2[User presses push button] --> B2[Arduino detects\nstate change]
B2 --> C2[Debounce and validate input]
C2 --> D2[Check current mode and\nfinger position state]
D2 --> E2[Determine action:\nOpen / Close / Pattern Shift]
E2 --> F2[Compute next servo angles\nbased on logic rules]
F2 --> G2[Move servos gradually\n(PWM stepping)]
G2 --> H2[Update position variables]
H2 --> I2[Return to Idle\n(await next input)]



flowchart TD

%% -------- NODES --------
subgraph PWR[Power Supply]
  PS[6V / 7.4V Battery]
end

subgraph INPUT[User Input Interfaces]
  PB[Push Button\nManual Input]
  BT[HC-05 Module\nBluetooth Interface]
  SW[Slide Switch\nMode Selector]
end

subgraph CTRL[Control System]
  ARD[Arduino Uno\nMain Controller]
  PWM[PWM Output\nGeneration Block]
  DRV[Servo Power /\nDriver Stage]
end

subgraph ACT[Actuation System]
  S1[Servo: Thumb]
  S2[Servo: Index]
  S3[Servo: Middle]
  S4[Servo: Ring]
  S5[Servo: Little Finger]
  HAND[3D-Printed Prosthetic Hand\nTendon Driven Mechanism]
end

subgraph FEEDBACK[Feedback & Indicators]
  LED[Status LEDs\n(Red / Blue)]
end

%% -------- CONNECTIONS --------
PS --> INPUT
PS --> CTRL
PS --> ACT

PB --> SW
BT --> SW
SW --> ARD

ARD --> PWM
PWM --> DRV
ARD --> DRV

DRV --> S1
DRV --> S2
DRV --> S3
DRV --> S4
DRV --> S5

S1 --> HAND
S2 --> HAND
S3 --> HAND
S4 --> HAND
S5 --> HAND

ARD --> LED
HAND --> LED



