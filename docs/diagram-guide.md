### **Part 2: 🏛️ Diagram Construction Guide**

Use the "Draw.io Integration" in VS Code to create `architecture.drawio` and follow these detailed blueprints. Export each completed diagram to a `.svg` file with the specified name in the `/docs` folder.

#### **Diagram 1: `architecture-overview.svg`**

**Blueprint:**
1.  **Central Component:** Place a large, prominent rectangle in the center. Label it "**Hyperion Control Plane**" with a sub-label "**Planetary-Scale AI Scheduler**". Use a distinct color like a medium blue.
2.  **Cloud Providers (Left):**
    * Create a bounding box labeled "**Public Clouds**".
    * Inside, place three smaller rectangles with logos for **AWS**, **GCP**, and **Azure**.
3.  **Edge Network (Right):**
    * Create a bounding box labeled "**Distributed Edge Network**".
    * Inside, place a few smaller icons representing edge devices (e.g., a small server or a chip icon). Label them "NVIDIA Jetson", "Local GPU Server", etc.
4.  **User (Top):**
    * Place a "User" icon at the top.
    * Create a rectangle representing a YAML file next to the user, labeled "`HyperionWorkload`".
5.  **Connections (Arrows):**
    * Draw a dashed arrow from the **User's YAML file** to the **Hyperion Control Plane**, labeled "**1. Define Workload & Policy**".
    * Draw solid, bi-directional arrows from the **Cloud Providers** and **Edge Network** to the **Control Plane**. Label these arrows "**Telemetry & Control (gRPC/QUIC)**".
    * Draw dashed arrows from the **Control Plane** out to the **Cloud Providers** and **Edge Network**. Label these "**2. Autonomous Placement Decisions**".

#### **Diagram 2: `architecture-control-plane.svg`**

**Blueprint:**
1.  **Main Bounding Box:** Create a large, light-blue rounded rectangle labeled "**Hyperion Operator (Go Process)**". This will contain the core components.
2.  **External Systems:**
    * Below the main box, place a cylinder shape labeled "**etcd**" and a rectangle next to it labeled "**Kubernetes API Server**". Draw a box around both labeled "**Shared State**".
    * To the right of the main box, place a rectangle labeled "**RL Model (Python Service)**".
    * To the left, place a rectangle labeled "**Prometheus**".
3.  **Internal Components (inside the main box):**
    * Place a rectangle on the left labeled "**Workload Reconciler**".
    * Place a rectangle in the middle labeled "**RL Scheduler Interface**".
    * Place a rectangle on the right labeled "**Telemetry Collector**".
4.  **Connections (Numbered Arrows for Flow):**
    * **Arrow 1:** From outside the diagram, point to the **Kubernetes API Server**, labeled "**1. `kubectl apply -f workload.yaml`**".
    * **Arrow 2:** From the **Kubernetes API Server** to the **Workload Reconciler**, labeled "**2. Watch Event**".
    * **Arrow 3:** From the **Workload Reconciler** to the **RL Scheduler Interface**, labeled "**3. Request Placement**".
    * **Arrow 4:** A bi-directional arrow between the **RL Scheduler Interface** and **Prometheus**, labeled "**4. Get System State**".
    * **Arrow 5:** A bi-directional arrow between the **RL Scheduler Interface** and the **RL Model**, labeled "**5. Get Action**".
    * **Arrow 6:** From the **RL Scheduler Interface** back to the **Kubernetes API Server**, labeled "**6. Create Pod Spec with Node Assignment**".
    * **Arrow 7:** From the **Telemetry Collector** to **Prometheus**, labeled "**Ingest Agent Metrics**".

#### **Diagram 3: `architecture-node.svg`**

**Blueprint:**
1.  **Main Bounding Box:** Create a large rectangle representing the "**Node (e.g., EC2 Instance / Edge Device)**".
2.  **OS Kernel:** At the very bottom, have a layer labeled "**Linux Kernel & GPU Driver**".
3.  **Core Components (inside the Node):**
    * Place a box for the standard "**Kubelet**".
    * Place a prominent, distinctly colored (e.g., green) box for our "**Hyperion Agent (Rust)**".
    * Show a box for "**Container Runtime (containerd)**".
4.  **Hyperion Agent Internals (inside the agent's box):**
    * `gRPC Server`: Listens for commands.
    * `Telemetry Probes`: Small components that connect to the OS Kernel layer to gather metrics (GPU, CPU, Network).
    * `Workload Executor`: The component responsible for managing container lifecycles.
5.  **Connections (Arrows):**
    * Draw a solid arrow from the **Kubelet** to the **Container Runtime**, labeled "**Manage Pods**".
    * Draw a dashed arrow from the **Hyperion Agent's Workload Executor** to the **Container Runtime**, showing it can also manage containers.
    * Draw a prominent, thick arrow originating from the **Hyperion Agent's Telemetry Probes**, going out of the main Node box, and pointing upwards. Label this arrow "**Telemetry Stream to Control Plane (gRPC over QUIC)**".