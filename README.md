# Defusing a Binary Bomb 🔬💣

This repository contains an educational “binary bomb” challenge originally adapted from CMU’s Computer Systems **Bomb Lab**.  The goal is to reverse‑engineer the program `bomb` (or build it from `bomb.c`) and enter six correct phase strings to stop the countdown—illustrating practical skills in disassembly, debugging, and x86‑64 logic.

> **WARNING:** Running the bomb with wrong inputs will exit with a *BOOM!* message. No real damage occurs, but prepare for many crashes as you work.

---

## 🗂 File Overview

| Path | Purpose |
|------|---------|
| `bomb` | Pre‑compiled 64‑bit ELF binary (Linux) |
| `bomb.c` | Obfuscated C source for reference / rebuilding |
| `CSE2138_Project2.zip` | Original assignment bundle (PDF spec, sample transcript) |
| `150121025.txt` | Placeholder for your final defusal input (one line per phase) |

---

## 🔧 Build (optional)
The provided binary is ready to run on **glibc 2.31+** systems.  If you’d like to compile:
```bash
gcc -std=c11 -O0 -g -fno-stack-protector -z execstack -o bomb bomb.c
```
Flags disable modern mitigations so you see classic behaviour (no PIE, no stack canary).

---

## 🚀 Running the Bomb
```bash
chmod +x bomb
./bomb               # prompts for phase 1 input
```
Type a guess and press **Enter**.  If incorrect, the bomb “explodes” (prints BOOM and exits `SIGINT`).  Restart and keep iterating.

You can feed all six lines at once:
```bash
./bomb < my_answers.txt
```

---

## 🧩 Phase Structure
Although each cohort’s bomb differs, common patterns are:
1. **String compare** – exact password.
2. **Numeric sequence** – arithmetic pattern.
3. **Switch / binary tree** – pick correct path.
4. **Recursive function** – provide integer that satisfies base cases.
5. **Linked‑list reorder** – submit indices in sort order.
6. **Secret phase** – often hidden behind `phase_defused` counter.

Use the supplied `bomb.c` to locate the `phase_1` … `phase_6` functions, then analyze control flow.

---

## 🛠 Reverse Engineering Tips
1. **GDB**: `gdb ./bomb`, set breakpoints (`b phase_1`), run, step (`si`, `ni`), inspect registers.
2. **Objdump**: `objdump -d -M intel bomb | less` for annotated assembly.
3. **Strings**: `strings -n 5 bomb` sometimes leaks hints.
4. **Draw the call graph**: `gdb -batch -ex "file bomb" -ex "info functions"`.
5. **Replay stdin** with redirection so you don’t re‑type past phases.

---

## 📚 References
* Computer Systems: A Programmer’s Perspective – Bryant & O’Hallaron §3 & §4
* CMU 15‑213 Lab 3: Bomb Lab ([public write‑ups](https://bomb.education))
* `gdb` Cheat Sheet – <https://darkdust.net/files/GDB%20Cheat%20Sheet.pdf>

---

## License
Original bomb framework © 2011 *Dr. Evil Inc.* Provided under fair‑use for academic practice.

