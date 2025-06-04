# Rocking G‑code Generator

A small, self‑contained Python utility that produces **mixing G‑code** for FDM 3‑D printers:

* Heats the build‑plate to a user‑defined temperature.
* Raises the nozzle (default **200 mm**—adjustable) so it never dips into liquids on the bed.
* Gently rocks the bed back‑and‑forth for a specified duration, amplitude and feed‑rate.

---

## ✨ Features

| Feature          | Description                                                                       |
| ---------------- | --------------------------------------------------------------------------------- |
| Flexible CLI     | Temperature, feed‑rate, duration, amplitude and Z‑lift are command‑line switches. |
| Printer‑agnostic | Only standard Marlin/RepRap G‑codes (`M140`, `M190`, `G91`, `G1`, `M400`, `G90`). |
| Safety first     | Nozzle stays safely elevated; relative moves avoid end‑stop collisions.           |
| Zero deps        | Pure Python ≥3.8 — no external libraries required.                                |

---

## 📦 Installation

```bash
# Clone the repo
$ git clone https://github.com/jimmyw/gcode_rocker.git
$ cd rocking‑gcode

# (Optional) create a virtual environment
$ python -m venv .venv && source .venv/bin/activate
```

No dependencies beyond the Python standard library.

---

## 🚀 Quick Start

```bash
python generate_rocking_gcode.py \
    --temp 50 \          # °C bed temperature
    --feedrate 600 \     # mm / min rocking speed
    --duration 30 \      # minutes
    --amplitude 10 \     # ± mm each half‑stroke
    --yoffset 100 \      # mm offset where shaking will commence
    mix_30min.gcode
```

This creates **`mix_30min.gcode`** that:

1. Heats the bed to 50 °C and waits.
2. Raises the nozzle 200 mm.
3. Rocks ±10 mm in Y at 600 mm/min for 30 minutes (≈ **N cycles** calculated automatically).

Send the file to your printer like any normal print job.

---

## 🛠 CLI Reference

| Argument      | Type  | Default    | Description                        |
| ------------- | ----- | ---------- | ---------------------------------- |
| `--temp`      | float | **(req.)** | Bed temperature in °C.             |
| `--feedrate`  | int   | **(req.)** | Rocking feed‑rate (mm/min).        |
| `--duration`  | float | `30`       | Duration in minutes.               |
| `--amplitude` | float | `10`       | Half‑stroke distance in mm.        |
| `--yoffset`   | float | `100`      | Posision of shake.                 |
| `outfile`     | str   | *(stdout)* | Path to save the generated G‑code. |

---

## 📖 How it works

* **Relative positioning** (`G91`) keeps moves simple and avoids printer‑specific origins.
* One full rock travels `4 × amplitude` mm (`+A −2A +A`).
* The script computes how many cycles best fit the requested duration:
  `cycles = round(duration_min / (4 × amp / feedrate))`.
* Because the nozzle is never returned to its starting Z, crashes with surface liquids are impossible—provided `--zlift` is within your machine’s travel range.

---

## 🧩 File list

```
│  generate_rocking_gcode.py   # main script
│  README.md                   # this file
└─ examples/
   └─ mix_30min.gcode          # sample output (git‑ignored by default)
```

---

## 🤝 Contributing

Issues and pull requests are welcome!  Feel free to:

* Add looping macros for Klipper/RepRapFirmware.
* Provide presets for common printers.
* Improve docs or tests.

---

## 📜 License

Released under the **MIT License** — see `LICENSE` for details.

---

## 🗨 Support / Questions

Open an issue or start a discussion on the repo.  Happy mixing!
