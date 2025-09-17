## WiseCloud — Energy-Aware VM Scheduling Simulator

WiseCloud is a Python simulator for energy-efficient virtual machine (VM) scheduling in cloud-like clusters. It models power usage, consolidates workloads, and evaluates scheduling strategies with live or synthetic datasets.

### Why it matters

-   Data centers are power-hungry; inefficient VM placement wastes energy.
-   WiseCloud quantifies the trade-offs between consolidation, performance, and energy.

### Highlights

-   **Schedulers**: Best‑Fit Decreasing, Minimum‑Utilization, Random (baseline)
-   **Energy model**: Idle/active curves, consolidation impact, basic cooling/PUE
-   **Live data**: Optional AWS EC2 + CloudWatch ingestion via `boto3`
-   **Visualization**: Real-time utilization and power charts (Tkinter + Matplotlib)
-   **Reproducible**: Deterministic simulation engine and sample dataset

---

### Quickstart

```bash
git clone <repo-url>
cd WiseCloud
python -m venv venv
venv\Scripts\activate  # on Windows; use source venv/bin/activate on macOS/Linux
pip install -r requirements.txt
python main.py
```

### CLI (headless)

Run a simulation without the GUI:

```bash
python simulation.py --hosts 10 --vms 200 --policy best_fit --duration 3600
```

---

### Repository structure

-   `main.py`: Tkinter GUI orchestrating simulations and plots
-   `scheduler.py`: Scheduling policies and placement logic
-   `simulation.py`: Discrete-time simulation engine
-   `energy_model.py`: Power/energy calculations and PUE helpers
-   `models.py`: Core entities (`Host`, `VM`)
-   `datasets.py`: Sample/local/AWS-backed workload generation
-   `aws_data_fetcher.py`: EC2/CloudWatch data ingestion
-   `sample_dataset.csv`: Example workload

### Architecture

-   **UI layer** (`main.py`): controls runs, renders charts
-   **Simulation core** (`simulation.py`): time loop, events, metrics
-   **Scheduling** (`scheduler.py`): placement, migration hooks
-   **Energy** (`energy_model.py`): per-host/cluster power modeling
-   **Data** (`datasets.py`, `aws_data_fetcher.py`): synthetic + AWS feeds

---

### Key capabilities

-   **VM placement**: consolidation, load-balancing, migration-friendly hooks
-   **Metrics**: energy (kWh), PUE, per-VM/host utilization, migrations
-   **Visualization**: live CPU/RAM/power charts; export results
-   **Extensible**: add policies by implementing a scheduler interface

### Configuration surface

-   Hosts and VM templates: see `models.py`, `datasets.py`
-   Energy parameters: see `energy_model.py`
-   Policy and runtime: via GUI controls or CLI flags

---

### AWS integration (local-only)

-   Runs entirely on your machine; connects to AWS over the internet.
-   Required IAM permissions: `ec2:DescribeInstances`, `cloudwatch:GetMetricStatistics`, `cloudwatch:ListMetrics`.
-   Minimal flow:
    1. `python main.py`
    2. Open “AWS Integration” tab → enter Access Key/Secret + region
    3. In “Dataset Configuration”, enable “Use Live AWS Data” and provide EC2 Instance IDs
    4. Run simulation

Security tips: never share credentials; use least-privilege IAM; credentials are used only for direct calls to AWS via `boto3`.

---

### Testing

```bash
python -m pytest -q
```

Includes threading and AWS widget tests: `test_aws_integration.py`, `test_aws_threading.py`, `test_aws_widgets.py`.

### Requirements

-   Python 3.8+
-   See `requirements.txt` for pinned dependencies

### Roadmap

-   ML-guided scheduling and predictive autoscaling
-   Multi-objective optimization (energy, SLA, cost)
-   Faster backends (parallel/distributed, optional GPU)
-   Rich dashboards and export formats

---

### Contributing

Issues and PRs are welcome. Please include reproducible steps and before/after metrics where applicable.
