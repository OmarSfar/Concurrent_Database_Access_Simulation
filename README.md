# Concurrent Database Access Simulation (OMNeT++)

A discrete-event simulation project developed in **OMNeT++** to analyze concurrency, queuing delays, and capacity limits in a distributed multi-client database access system.

Developed for the *Computer Engineering / Artificial Intelligence and Data Engineering* Master's program at the **University of Pisa** (A.Y. 2025/2026).

---

## 📌 Project Overview

The project models $N$ concurrent users generating read/write requests to $M$ database tables under a **Readers/Writers concurrency model** with **FCFS (First-Come, First-Served)** queueing and mutual exclusion:
- **Shared Reads:** Multiple readers can access the same table concurrently.
- **Exclusive Writes:** Write operations require exclusive table access and block subsequent requests.
- **Traffic Patterns:** User request generation follows a Poisson process ($\lambda$), with access targets distributed either **Uniformly** or via **Lognormal** distribution (hotspot modeling).

---

## 🏗️ System Architecture & Model

- **`User` Module (`User.ned / .cc / .h`):** Generates read (prob. $p$) and write (prob. $1-p$) requests with exponential inter-arrival times.
- **`Table` Module (`Table.ned / .cc / .h`):** Manages local FCFS queues, reader/writer locks, and service time processing ($S$).
- **`DatabaseNetwork` (`DatabaseNetwork.ned`):** Fully-connected mesh topology interconnecting $N$ users and $M$ tables.
- **Statistics & Warm-Up:** Built-in OMNeT++ `@signal` / `@statistic` tracking with a verified **500s warm-up period** to eliminate transient initialization bias.

---

## 🔬 Experimental Verification & Analysis

The simulation model was validated through rigorous testing and factorial design (360 total simulation runs):
1. **Degeneracy & Continuity Tests:** Verified boundary conditions ($N=0, M=1, p=0, p=1$) and linear response to proportional parameter scaling.
2. **Theoretical Queueing Comparison:** Compared empirical utilization and throughput against Open Queueing Network theoretical models ($U = \frac{N \cdot \lambda \cdot S}{M}$).
3. **Statistical Validation:** Residual normality (QQ-plot, Shapiro-Wilk) and homoscedasticity evaluation across diverse load scenarios.

---

## 📊 Key Findings & Capacity Limits

*(Baseline benchmark: M = 20 tables, λ = 0.05 req/s, S = 0.1 s)*

- **Load Dominance:** The number of active users ($N$) accounts for **~72.5%** of throughput variability.
- **The Hotspot Bottleneck:** Non-uniform access patterns (Lognormal) reduce usable system capacity by **50%–60%** compared to Uniform distribution due to localized table contention.
- **Read Ratio Impact:** Read-heavy workloads ($p = 0.8$) significantly expand stable operating headroom.

| Workload Configuration | Access Pattern | Stable Capacity ($W < 200	ext{ ms}$) | Stall Boundary ($W > 1000	ext{ ms}$) |
| :--- | :--- | :---: | :---: |
| **Write-heavy ($p = 0.3$)** | Uniform | up to 2,500 users | $N > 4,000$ |
| **Write-heavy ($p = 0.3$)** | Lognormal (Hotspots) | up to 1,200 users | $N \ge 1,500$ |
| **Balanced ($p = 0.5$)** | Uniform | up to 3,000 users | Degraded at $N = 5,000$ |
| **Balanced ($p = 0.5$)** | Lognormal (Hotspots) | up to 1,200 users | $N \ge 2,000$ |
| **Read-heavy ($p = 0.8$)** | Uniform | **5,000+ users** | Stable ($W  pprox 154	ext{ ms}$) |
| **Read-heavy ($p = 0.8$)** | Lognormal (Hotspots) | up to 2,500 users | $N \ge 4,000$ |

---

## 🛠️ Requirements & How to Run

1. **Prerequisites:** [OMNeT++](https://omnetpp.org/) (v5.x / v6.x).
2. **Build:** Import the project directory into the OMNeT++ IDE or build from CLI via `opp_makemake && make`.
3. **Execute Simulations:** Run experiments configured in `omnetpp.ini`.

---

## 👥 Authors
- **Luca Rotelli**
- **Omar Tomas Sfar**
- **Greta Gashi**

*Master's Degree in Computer Engineering / AI & Data Engineering — University of Pisa*
