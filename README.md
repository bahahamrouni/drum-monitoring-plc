# Chemical drum level monitoring with a PLC

Design and simulation of a PLC system that watches the weight of two metal drums feeding a dosing machine, warns before a drum runs empty and stops the machine before it can run dry.

Individual engineering internship project (August 2025, about three weeks). Company and plant details are intentionally left out. The system was designed and validated in simulation only; it was not installed or tested on physical equipment. Documents are in French.

## The problem

Two drums supply liquid components to a machine, and nobody can see how full they are. When a drum empties, the machine draws air and stops. The need was:

- weigh each drum continuously and show the state on a display;
- raise a **warning** at a first weight threshold (S1) and **stop** the machine at a second one (S2);
- give each drum its own buzzer and reset button;
- provide a maintenance mode and a safety stop.

## What is in this repository

| Path | Content |
|---|---|
| `plc/plc_programming.smbp` | Ladder program for a Schneider Modicon M221, written in EcoStruxure Machine Expert - Basic (46 rungs) |
| `plc/screenshots/` | I/O, memory, message and Modbus configuration screenshots |
| `grafcet/grafcet.pdf` | GRAFCET models: main sequence, safety and maintenance |
| `electrical/` | Power schematic (QElectroTech `.qet`) and the wiring sheet (device, pin, PLC address, wire number) |
| `calculations/threshold_calculation.txt` | How S1 and S2 are derived, with a worked example |
| `calculations/calibration_procedure.docx` | Procedure to calibrate a weighing channel on a PLC analog input (offset and scale factor) |
| `simulation/` | Table of simulated test scenarios and the online PLC simulator link |
| `archive/version-1/` | The first version of the program and GRAFCET |

## How it works

- Each weighing cell feeds a PLC analog input (`%IW0.0` for drum 1, `%IW0.1` for drum 2); the raw value is converted to a weight with an offset and a scale factor measured during calibration.
- **Thresholds:** `S2` is the empty-drum weight. `S1 - S2 = order lead time x weekly consumption x share of the component in the mix`, so the warning leaves enough time to reorder. The worked example in `calculations/` uses illustrative numbers.
- Messages are shown on an industrial display over a Modbus RTU link with the PLC as master. Each drum has its own buzzer, with a reset button.
- The GRAFCET models separate the normal sequence, the safety behaviour and a maintenance mode.

## Open the files

Install EcoStruxure Machine Expert - Basic for the `.smbp` file and QElectroTech for the `.qet` file. The PDFs open anywhere.

## Limits

- Simulation only: no hardware was installed or tested, and the scenario table covers simulated weights and states.
- The worked threshold example uses illustrative figures, not plant data.
- No automated tests; the verification is the scenario table run in the simulator.
