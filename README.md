# 🛡️ Real-Time Host Anomaly Detection System

An unsupervised machine learning Intrusion Detection System (IDS) designed to monitor host-machine resources and detect Zero-Day attacks using an **Isolation Forest** algorithm. 

This project simulates real-time system monitoring (CPU, Memory, Disk, Network, and Processes) and uses a Sliding Window approach to filter out background noise, resulting in highly stable and accurate threat detection.

## ✨ Key Features
* **Optimized 5-Feature Vector:** Extracted and mapped the most dominant numerical features from the NSL-KDD dataset to represent physical hardware load.
* **Real-Time Sliding Window:** Utilizes a `deque` (size 10) moving average to evaluate sustained system behavior, eliminating false positives caused by transient CPU spikes.
* **Logarithmic Data Transformation:** Applies `np.log1p` to network and process metrics to normalize massive data variance (0 to 1 Billion+), ensuring the model evaluates all dimensions equally.
* **High-Precision Inference:** Tuned contamination thresholds (`0.1`) and expanded tree ensembles (`300 trees`) to achieve a **90% Precision Rate** on malicious anomalies.

## 📊 Model Performance
The model was trained strictly on 'Normal' traffic to establish a clean baseline and tested against 22,552 records (mixed normal and attack data).

| Metric | Score | Detail |
| :--- | :--- | :--- |
| **Overall Accuracy** | **76.51%** | Robust unsupervised classification. |
| **Anomaly Precision** | **0.90** | When an alert triggers, there is a 90% chance it is a verified attack. |
| **Normal Recall** | **0.90** | The system correctly ignores 90% of safe, background system noise. |

## 🗄️ Dataset Details
This model utilizes the **NSL-KDD** dataset. To adapt network-level data for host-based monitoring, the following feature mapping was used:
1. `count` ➡️ **CPU Usage** (Connection intensity)
2. `srv_count` ➡️ **Memory Usage** (Service-level load)
3. `dst_host_count` ➡️ **Disk Usage** (Destination reach)
4. `src_bytes` ➡️ **Network Traffic** (Data exfiltration)
5. `dst_bytes` ➡️ **Process Count** (Payload intensity/Fork bombs)


### 🚀 Installation & Usage

1. **Clone the repository**
   ```bash
   git clone [https://github.com/namita12666/Anomaly-Detection-System.git](https://github.com/namita12666/Anomaly-Detection-System.git)
   cd Anomaly-Detection-System
   
2. **Install dependencies**
   ```bash
   pip install -r requirements.txt

3. **Train the Model**
   - Ensure `KDDTest.arff.csv` is in the root directory.
   - Run the training script to generate the optimized `model.pkl` and `scaler.pkl` files.

4. **Run the Real-Time Simulation**
Execute the detection engine to see the sliding window in action.

## 🎯 Simulation Scenarios
The simulate_stream.py script includes four live-testing modes. Each attack begins with a 10-step "Healthy" lead-in to demonstrate the Sliding Window transition.

normal: Standard system idle behavior.

cpu_spike: Simulates a Denial of Service (DoS) attack maxing out compute logic.

process_bomb: Simulates a Fork Bomb multiplying active process IDs.

network_attack: Simulates massive data exfiltration or a DDoS attack.
