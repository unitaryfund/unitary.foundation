---
title: Introducing the Unitary Compiler Collection (UCC)
author: Unitary Foundation Team
day: 4
month: 3
year: 2025
tags: 
  - python
  - compiler
  - qiskit
  - qbraid
  - quantum hardware
---
As quantum hardware advances, so does the complexity of programming it. Increasing qubit counts, diverse gate operations, and varied architectures present new challenges for quantum compilation. If we want quantum computation to be useful and accessible to more people, which we do at Unitary Foundation, we are going to need great open source compilers.

Two major challenges that we see are:

1. Compiler improvements are often isolated in separate libraries or one-off repositories without integration into existing tools.  
2. There are high switching costs between quantum computing frameworks and hardware platforms.

To address these, Unitary Foundation has started building the **Unitary Compiler Collection (UCC)** to foster collaboration and streamline quantum compilation.

### **Why UCC?**

With UCC, we are building a home for **quantum compiler experts** to develop new passes and at the same time reducing friction for **quantum algorithm developers** to transition between simulation and hardware.

By surveying the ecosystem, we have begun using the best of open source: integrating performant and user-friendly tools into UCC, creating a platform-agnostic interface with competitive compile time and gate reduction efficiency. 

### **Key Features**

#### **1\. Best-in-Class Compilation**

We benchmark various compilers and continuously improve UCC’s default compilation (currently **"UCCDefault1"**) by integrating passes from existing compilers and developing custom transpiler passes—**and [we need your help](https://ucc.readthedocs.io/en/latest/contributing.html#proposing-a-new-transpiler-pass)\!**

Right now, UCC leverages a subset of Qiskit’s transpiler passes optimized with Rust for circuit optimization. We regularly benchmark UCC’s default passes against existing compilers and SDKs to ensure we are capturing the best the ecosystem has to offer to reduce compiled gate counts and runtime.

You can [suggest](https://github.com/unitaryfund/ucc/issues/new/choose) adding a new compiler to our benchmarks or explore the workloads and backends we benchmark continuously [here](https://github.com/unitaryfund/ucc/blob/main/benchmarks/scripts/run_benchmarks.sh). 

#### **2\. No Code Changes to Switch Between Frontends**

UCC leverages **qBraid** to support **Qiskit, Cirq, TKET**, and a growing number of other quantum frameworks, so developers can use their preferred tools to define circuits without rewriting code.

```py
from ucc import compile
compiled_qiskit_circuit = compile(my_qiskit_circuit)
compiled_cirq_circuit = compile(my_cirq_circuit)
```

#### **3\. Compatible with Any Backend Supporting OpenQASM**

Not only does UCC accept a wide array of input circuit formats, but it can compile and seamlessly convert between them the `return_format` keyword – no extra imports needed:

```py
tket_circuit = compile(my_qiskit_circuit, return_format="tket")
qasm2_circuit = compile(my_qiskit_circuit, return_format="qasm2")
```

Supported return formats also include OpenQASM 2.0 and 3.0, making UCC compatible with major quantum hardware providers, including: **IBM Quantum, Rigetti, IonQ, Amazon Braket,** and more.

### **The Future of UCC**

We are releasing UCC in its foundational stages because we are committed to building in public and inviting the quantum open source community to shape its development with us. 

For development priorities post-launch, we want to focus on what we think is most impactful for the progress of quantum computing – and where we have unique expertise at Unitary Foundation – leveraging our diverse and dynamic quantum open-source community to push into the regime of thousands of qubits, 10s of thousands of gates, where compilers require novel architectures and abstractions to handle errors.

Key roadmap items include:

* **Quantum Error Mitigation (QEM):** Integration with [**Mitiq**](https://unitary.foundation/posts/2024_mitiq_impact/), our cross-platform QEM library with 212k+ downloads and 100+ citations.  
* **Hardware-Aware Compilation:** Custom routing and scheduling optimized for emerging architectures.  
* **Quantum Error Correction (QEC):** Implementing early fault tolerance protocols in conjunction with error mitigation  
* **Hybrid classical-quantum programming:** Mid-circuit measurements, fast feedback, repeat-until-success, etc. 

We will collaborate with the broader quantum software and hardware communities to develop new abstractions and tooling for the **Early Fault Tolerance** era. We are grateful to partner with collaborators in the [SMART Stack](https://cs.uchicago.edu/news/doe-awards-fred-chong-and-his-national-research-team-7-5m-to-develop-a-smart-software-stack-to-control-quantum-computer-noise/), a DOE-funded project to develop novel approaches in quantum stack design that are Scalable, Modular, cross-platform Adaptable, dynamically Reconfigurable, and error-Targeted. We look forward to collaborating with UF members and the wider Unitary Foundation community.

### **Help out by contributing to UCC**

We’re building UCC as a community-driven project\! You can help by:

* [**Creating Custom Compiler Passes**](https://ucc.readthedocs.io/en/latest/contributing.html#proposing-a-new-transpiler-pass)  
* [**Reporting Bugs & Requesting Features**](https://github.com/unitaryfund/ucc/issues)  
* [**Contributing Code**](https://ucc.readthedocs.io/en/latest/contributing.html#contributing-guide)  
* [**Joining the Discussion**](https://discord.com/channels/764231928676089909/1346546840526524427)

**Get involved and help shape the future of quantum compilation\!**

GitHub: [**unitaryfund/ucc**](https://github.com/unitaryfund/ucc)  
Docs: [**UCC Documentation**](https://ucc.readthedocs.io/)  
Stay Updated: [**Mailing List**](https://bit.ly/uf-signup)
