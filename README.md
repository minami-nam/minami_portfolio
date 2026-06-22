# 작업물 정리용 Portfolio : About RTL Design, LLM/ML, Computer Vision, and Personal server infrastructure

> Electronic & Electrical Engineering undergraduate portfolio

해당 리포지토리는 제가 작업하거나, 혹은 작업에 참여한 작업물을 개재하고, 간단한 설명을 덧붙여 이해를 돕기 위하여 개설하였습니다.
2026-06-22 기준 상시 업데이트 중입니다.

---

## Overview

현재 크게 4개의 주제를 가지고 작업물을 생성하고 있으며, 마지막 scratch의 경우 잡다한 작업물들을 개재하고 있습니다.

| Category          | Description                                                                                                   |
| ----------------- | ---------------------------------------------------------------------------------------------------------     |
| [verilog_RTL](./projects/verilog_RTL.md)         | Verilog/SystemVerilog RTL design, digital IP blocks 설계 역량 증진을 목표로 여러 모듈을 설계하고 있습니다.        |
| [llm_ml](./projects/llm_ml.md)                   | 한국어 경량 LLM 모델에 대해 관심을 가지고 여러 작업을 진행하는 중입니다. 현재 민원 안내 챗봇을 개발하였습니다.        | 
| [computer_vision](./projects/computer_vision.md) | CNN Based Image Enhancement에 대해 주로 다루며, 경량화 모델을 개발하고 있습니다.                                  |
| [server_logs](./projects/server_logs.md)         | 현재 개발을 진행중인 서버의 설정과, Open WebUI를 통한 로컬 LLM 구동 설정 등을 여기에 기록할 계획입니다.              |
| [scratch](./projects/scratch.md)                 | 프로젝트로 분리하긴 애매한 작업들을 여기서 처리할 계획입니다.                                                      |

---

## Repository Structure

```text
engineering-portfolio/
├── README.md
├── verilog_RTL/
│   ├── README.md
│   ├── axi4_controller.md
│   ├── axi4_memory_subsystem.md  (계획중)
│   ├── sram_wrapper.md
│   ├── riscv_pipeline.md
│   └── uvm_verification.md       (계획중)
│
├── llm_ml/
│   ├── README.md
│   ├── local_llm_stack.md        (계획중)
│   ├── civil_service_chatbot.md
│   ├── llm_dataset_builder.md    (계획중)
│   ├── small_transformer.md
│   └── orchestrator_harness.md   (진행중)
│
├── computer_vision/
│   ├── README.md
│   ├── image_enhancement_cnn.md
│   └── flat_region_conv_reuse.md  (계획중)  
│
├── server_logs/
│   ├── README.md
│   ├── proxmox_notes.md
│   ├── local_ai_server.md
│   └── troubleshooting.md
│
└── scratch/
    ├── README.md
    ├── experiments.md
    └── archived_notes.md

```

---

## Featured Projects

### 1. Verilog / RTL / Digital IP

| Project                 | Description                                                     | Main Topics                                | Status      |
| ----------------------- | --------------------------------------------------------------- | ------------------------------------------ | ----------- |
| AXI4 Controller         | AXI4 read/write transaction controller with scheduling logic    | AMBA AXI4, Burst, Valid/Ready, FSM         | Completed   |
| AXI4 Memory Subsystem   | Multi-master/multi-slave memory subsystem concept based on AXI4 | Address Decoder, Interconnect, Arbitration | Planned     |
| SRAM Wrapper            | FPGA-oriented SRAM wrapper with registered output structure     | BRAM Inference, STA, Timing Closure        | Completed   |
| RISC-V 5-stage Pipeline | Basic 5-stage pipelined processor implementation                | IF/ID/EX/MEM/WB, Hazard, Forwarding        | Completed   |
| UVM AXI4 Verification   | UVM-based verification environment for AXI4 modules             | UVM Agent, Scoreboard, Coverage            | Planned     |

---

### 2. LLM / ML

| Project                | Description                                                                | Main Topics                                  | Status             |
| ---------------------- | -------------------------------------------------------------------------- | -------------------------------------------- | ------------------ |
| Local LLM Stack        | Local inference environment using Ollama, Open WebUI, LiteLLM, and SearXNG | Local LLM, Docker, Routing, Search           | Planned            |
| Civil Service Chatbot  | Domain-specific chatbot for public civil service guidance                  | Dataset Design, RAG, Fine-tuning             | In Progress        |
| LLM Dataset Builder    | JSONL/CSV dataset construction workflow for instruction tuning             | Data Generation, Preprocessing, Evaluation   | Planned            |
| Small Transformer      | Small decoder-only transformer implementation for learning model internals | Tokenization, Attention, CE Loss, PPL        | Prototype Ready    |
| Orchestrator / Harness | Model/tool routing system to reduce unnecessary tool calls                 | Router, Tool Calling, Quality-Cost Trade-off | In Progress        |

---

### 3. Computer Vision

| Project                       | Description                                                                         | Main Topics                                 | Status              |
| ----------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------- | ------------------- |
| Image Enhancement CNN         | CNN-based image enhancement experiment using tone/curve-based processing            | CNN, Image Enhancement, PyTorch             | Completed           |
| Flat-Region Convolution Reuse | Reducing redundant convolution operations in flat regions of high-resolution images | CNN Acceleration, MAC Reduction, PSNR, SSIM | Research Idea       |

---

### 4. Server Logs / Infrastructure

| Project              | Description                                                | Main Topics                         | Status |
| -------------------- | ---------------------------------------------------------- | ----------------------------------- | ------ |
| Local AI Server      | Personal AI development server setup and operation notes   | Ubuntu Server, NVIDIA GPU, Docker   | Active |
| Proxmox Notes        | Virtualization environment setup and troubleshooting       | Proxmox, VM, PCIe Passthrough       | Active |
| Troubleshooting Logs | Error logs and solutions collected during server operation | Networking, Ports, CUDA, Containers | Active |

---

### 5. Scratch / Archive

| Section         | Description                                               |
| --------------- | --------------------------------------------------------- |
| Experiments     | Small technical experiments and early-stage ideas         |
| Archived Notes  | Old project notes that are no longer actively maintained  |


---

## 2027년 후반기까지 습득하고자 하는 Skills

* Digital IC Design
* RTL Design using Verilog/SystemVerilog
* UVM-based verification
* AI accelerator architecture design
* CNN optimization and image enhancement
* Local LLM inference stack
* Fine-tuning dataset construction

---

## Skills (2026.6월 기준)

### Hardware / RTL

* Verilog
* SystemVerilog
* FSM design
* Memory interface design
* Testbench construction
* Waveform debugging

### AI / ML

* Python
* PyTorch
* CNN-based image processing
* LLM fine-tuning workflow
* Dataset preprocessing
* Evaluation metric design
* Local LLM inference setup

### Systems / Infrastructure

* Ubuntu Server
* Proxmox
* NVIDIA GPU setup
* SSH-based remote development
* Local AI server settings


---

## Notes
해당 리포지토리는 지속적으로 업데이트가 있을 예정입니다.    
2026년 6월 기준 크게 4가지의 주제를 가지고 작업물을 업데이트할 예정이나, 상황에 따라 다양한 주제가 추가될 수 있습니다.
