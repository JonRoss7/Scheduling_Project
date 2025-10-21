# Scheduling_Project

This repository contains a set of simple CPU scheduling algorithm implementations and a small task scheduling simulator written in C++ for educational purposes. The code demonstrates several classical scheduling algorithms, measurements (waiting time, turnaround time, CPU utilization, throughput), and a small command-line simulator that can run different schedulers on input process lists.

## Contents

Top-level layout:

- `Exercises/` — individual example implementations and small executables for many scheduling algorithms (FCFS, SJF, SRTF, Round Robin, Priority, MLFQ skeletons, Lottery skeletons, CFS/EDF skeletons, and others).
- `taskSchedulingSimulator/` — a small, more configurable simulator (`taskSchedulingSim.cpp`) that accepts CLI flags to choose scheduler, input, output, quantum, and random generation.

Files of note

- `Exercises/FCFS.cpp` — First-Come First-Served example (single-run executable style).
- `Exercises/SJF.cpp` — Shortest Job First (non-preemptive) example.
- `Exercises/SRTF.cpp` — Shortest Remaining Time First (preemptive) example.
- `Exercises/roundRobin.cpp` — Round Robin example with fixed quantum.
- `Exercises/priorityScheduler.cpp` — Priority scheduling example.
- `Exercises/MLFQ.cpp`, `Exercises/multiQueue.cpp` — Multi-level feedback queue examples / skeletons.
- `Exercises/lotteryScheduler.cpp`, `Exercises/lotterySchelduler` — Lottery scheduler example and binary (if present).
- `Exercises/CFS.cpp`, `Exercises/EDF.cpp` — Completely Fair Scheduler and Earliest Deadline First skeletons (some are marked not implemented in simulator).
- `taskSchedulingSimulator/taskSchedulingSim.cpp` — Configurable simulator that implements FCFS, SJF, SRTF, Priority, Round Robin (RR) and contains stubs for MLFQ/MLQ, Lottery, CFS, EDF.
- `taskSchedulingSimulator/test_cases.txt` — example input format for process lists.

## Build

These are small, standalone C++ programs. You can compile them with g++ (C++11 or later). Example commands (from repository root):

```bash
# Compile the simulator
g++ -std=c++11 -O2 -o taskSchedulingSimulator/simulator taskSchedulingSimulator/taskSchedulingSim.cpp

# Compile an example exercise (creates an executable in Exercises/)
g++ -std=c++11 -O2 -o Exercises/fcfs Exercises/FCFS.cpp
g++ -std=c++11 -O2 -o Exercises/rr Exercises/roundRobin.cpp
g++ -std=c++11 -O2 -o Exercises/sjf Exercises/SJF.cpp
```

If you prefer to build all the small examples, run the above for each `.cpp` file in `Exercises/`.

## Usage

1) Using the configurable simulator

The simulator supports a few command-line flags:

- `--scheduler <type>` — the scheduler to run. Supported values in this version: `rr` (round-robin), `fcfs`, `sjf`, `srtf`, `priority`.
- `--input <file>` — path to a text file containing lines with: `ID arrival_time burst_time priority` (see `taskSchedulingSimulator/test_cases.txt`).
- `--output <file>` — write results to a file instead of stdout.
- `--quantum <n>` — quantum value for round-robin (default 4).
- `--random --num <n>` — generate `n` random processes instead of reading input.

Examples:

```bash
# Run the simulator with a test file and FCFS
./taskSchedulingSimulator/simulator --scheduler fcfs --input taskSchedulingSimulator/test_cases.txt

# Run Round Robin with quantum 3 and write output
./taskSchedulingSimulator/simulator --scheduler rr --quantum 3 --input taskSchedulingSimulator/test_cases.txt --output rr_output.txt

# Generate 12 random processes and run SRTF
./taskSchedulingSimulator/simulator --scheduler srtf --random --num 12
```

2) Running individual example programs

Each example in `Exercises/` can usually be run without arguments; they use built-in example process lists. For example:

```bash
./Exercises/fcfs
./Exercises/rr
./Exercises/sjf
```

Output

Programs print average waiting time, average turnaround time, CPU utilization, throughput and a simple Gantt chart to stdout (or to a file if `--output` is used by the simulator).

## Input format (for `--input`)

Each line should contain: ID arrival_time burst_time priority
Example (as in `taskSchedulingSimulator/test_cases.txt`):

P1 0 8 2
P2 1 4 1
P3 2 9 3
P4 3 5 4

The simulator sorts processes by arrival time before scheduling.

## Notes & Implementation status

- The `taskSchedulingSimulator` implements FCFS, SJF (non-preemptive), SRTF (preemptive), Priority (non-preemptive), and Round Robin (preemptive) in a modular `Scheduler` class hierarchy. Several advanced schedulers (MLFQ, Lottery, CFS, EDF) are present as class stubs but their `schedule` methods are not implemented in this version.
- Several compiled binaries may exist in the `Exercises/` folder (files without `.cpp`) — these appear to be prebuilt executables. If you prefer to rebuild from source, compile the corresponding `.cpp` files as shown in the Build section.
- The code is written for readability and teaching. It is not optimized for production use.

## Quick verification (optional)

To verify the simulator builds and runs on your machine:

```bash
g++ -std=c++11 -O2 -o taskSchedulingSimulator/simulator taskSchedulingSimulator/taskSchedulingSim.cpp
./taskSchedulingSimulator/simulator --scheduler rr --quantum 4
```

If compilation fails due to an older compiler, try `g++ -std=c++17` or install a newer g++.
