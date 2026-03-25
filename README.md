# 🧠 MasterMind — Raspberry Pi Systems Programming

![C](https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white)
![ARM Assembly](https://img.shields.io/badge/ARM_Assembly-0091BD?style=for-the-badge&logo=arm&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry_Pi-A22846?style=for-the-badge&logo=raspberrypi&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

A low-level implementation of the classic **MasterMind** board game, built in **C and inline ARM Assembly** and deployed on a **Raspberry Pi 3**. Player input is handled entirely through a single button, with a two-LED system and an LCD display providing all visual feedback — no keyboard or screen required during gameplay.

---

## 🎮 How the Game Works

The Raspberry Pi acts as the **codekeeper**, generating a secret 3-digit sequence (using digits 1–3). The player is the **codebreaker**, entering guesses using only a button. The game gives **3 attempts** to crack the code.

**Input — button presses:**
- Press the button N times to enter the digit N
- A timeout separates each digit input

**Output — LED feedback:**

| Signal | Meaning |
|--------|---------|
| 🔴 Red blinks 3× | New round starting |
| 🔴 Red blinks 1× | Digit input received |
| 🟢 Green blinks N× | Echo of digit entered (N = value) |
| 🔴 Red blinks 2× | All digits entered, input complete |
| 🟢 Green blinks N× | Number of **exact** matches |
| 🔴 Red blinks 1× | Separator |
| 🟢 Green blinks N× | Number of **approximate** matches |
| 🟢 Green blinks 3× + 🔴 Red ON | Code cracked — you win! |

**LCD Display:** Shows exact and approximate match counts each round, and prints `SUCCESS` + number of attempts on a win.

---

## ⚙️ Hardware

| Our Hardware Structure |
(Hardware.png)

### Components

| Component | Role |
|-----------|------|
| Raspberry Pi 3 | Main processor / codekeeper |
| Green LED | Data output (GPIO pin 13) |
| Red LED | Control output (GPIO pin 5) |
| Push Button | Player input (GPIO pin 19) |
| 16×2 LCD Display | Text output |
| Potentiometer | LCD contrast control (LCD pin 3) |
| Breadboard | Wiring platform |

### GPIO Wiring — LCD

| LCD Pin | GPIO / Connection |
|---------|-------------------|
| 1 (GND) | GND |
| 2 (3v Power) | 3.3V |
| 3 (Potentiometer) | Middle pin of potentiometer |
| 4 (RS) | GPIO 25 |
| 5 (RW) | GND |
| 6 (EN) | GPIO 24 |
| 11 (DATA4) | GPIO 23 |
| 12 (DATA5) | GPIO 10 |
| 13 (DATA6) | GPIO 27 |
| 14 (DATA7) | GPIO 22 |
| 15 (LED+) | 3.3V |
| 16 (LED-) | GND |

> ⚠️ Use the **3.3V** power pin, not 5V.

---

## 🗂️ Code Structure

```
├── master-mind.c       # Core game logic (C)
├── mm-matches.s        # Matching function in ARM Assembly
├── Makefile            # Build and test automation
└── README.md
```

### Key Functions

**ARM Assembly (low-level GPIO control):**

| Function | Description |
|----------|-------------|
| `digitalWrite()` | Write HIGH/LOW to a GPIO pin |
| `pinMode()` | Set a GPIO pin to INPUT or OUTPUT mode |
| `readButton()` | Read HIGH/LOW from a GPIO pin |
| `waitForButton()` | Block until button press detected |
| `blinkN()` | Blink an LED N times |

**C (game logic):**

| Function | Description |
|----------|-------------|
| `initSeq()` | Generate a random 3-digit secret sequence |
| `readSeq()` | Decode an integer into an array |
| `showSeq()` | Print the secret sequence to terminal |
| `showSeqG()` | Print the player's guess to terminal |
| `countMatches()` | Count exact and approximate matches (ARM) |
| `showMatches()` | Display match results after `countMatches()` |
| `buttonInput()` | Read player input via button presses with timeout |

---

## 🚀 Build & Run

### Prerequisites
- Raspberry Pi 2 or 3 running **Raspbian (32-bit)**
- GNU toolchain: `gcc`, `as`, `ld`
- Hardware wired as specified above

### Compile

```bash
gcc master-mind.c -o cw2
```

Or using the Makefile:

```bash
make
```

### Run

```bash
# Normal gameplay
sudo ./cw2

# Debug mode — shows secret sequence
sudo ./cw2 -d

# Debug with a fixed secret sequence
sudo ./cw2 -s 123 -d

# Unit test — compare two sequences
./cw2 -u 123 321
```

---

## ⌨️ CLI Options

| Option | Description |
|--------|-------------|
| *(none)* | Normal gameplay with hardware I/O |
| `-d` | Debug mode — prints secret sequence and answers |
| `-s <seq>` | Set a specific secret sequence (e.g. `-s 123`) |
| `-u <seq1> <seq2>` | Unit test — print exact and approximate matches |

### Example

```bash
$ ./cw2 -u 123 321
1 exact match(es)
2 approximate match(es)
```

---

## 🧪 Sample Execution (Debug Mode)

```
///////////////// The Master Mind Game is Starting, Get Ready! /////////////////
5 attempts can you crack the code? , good luck!

The value of sequence was: 111.
Secret Code Length: 3.

Round 1
Guess 1 — Value stored: 1
Guess 2 — Value stored: 2
Guess 3 — Value stored: 3
The value of sequence was: 123.
1 exact match(es)
0 approximate match(es)
End of the round 1

...

Round 4
The value of sequence was: 111.
3 exact match(es)
0 approximate match(es)
Code has been cracked! Codebreaker has WON!
```

---

## 🛠️ Tech Stack

![C](https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white)
![ARM Assembly](https://img.shields.io/badge/ARM_Assembly-0091BD?style=for-the-badge&logo=arm&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry_Pi-A22846?style=for-the-badge&logo=raspberrypi&logoColor=white)
![Raspbian](https://img.shields.io/badge/Raspbian-A22846?style=for-the-badge&logo=raspberrypi&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
