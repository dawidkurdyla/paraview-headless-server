# ParaView Headless Server – Dockerized

This repository provides Minimal Docker setup for for running ParaView (`pvserver`) in headless mode:

1. **CPU version (OSMesa)** – uses software rendering on the CPU (no GPU required).  
2. **CUDA version (GPU + EGL)** – uses an NVIDIA GPU for accelerated rendering (EGL) and VTK-m CUDA support.

## Table of Contents
- [ParaView Headless Server – Dockerized](#paraview-headless-server--dockerized)
  - [Table of Contents](#table-of-contents)
  - [Requirements](#requirements)
    - [General](#general)
    - [Additional requirements for GPU](#additional-requirements-for-gpu)
  - [Repository Structure](#repository-structure)
  - [How to Build and Run (CPU)](#how-to-build-and-run-cpu)
    - [Building the CPU Image](#building-the-cpu-image)
    - [Running pvserver (CPU)](#running-pvserver-cpu)
  - [How to Build and Run (CUDA)](#how-to-build-and-run-cuda)
    - [Building the CUDA Image](#building-the-cuda-image)
    - [Running pvserver (CUDA)](#running-pvserver-cuda)
  - [Connecting to ParaView (Client-Server)](#connecting-to-paraview-client-server)
  - [License](#license)

---

## Requirements

### General
1. **Docker** – Docker Desktop on Windows 10/11 or a native Docker installation on Linux/macOS.
2. **Network connectivity** – so the ParaView GUI client can connect to the `pvserver` inside the container.
3. **Port 11111 open** – the default port for ParaView client-server communication.

### Additional requirements for GPU
1. **NVIDIA GPU** with proper drivers on the host.
2. **NVIDIA Container Toolkit** – required for Docker to access your GPU (`--gpus all`).
3. Use:
   ```bash
   docker run --gpus all ...
   ```

---

## Repository Structure
- `dockerfiles/`
  - `Dockerfile.cpu` – CPU (OSMesa) variant.
  - `Dockerfile.cuda` – CUDA + EGL (NVIDIA) variant.
- `README.md` – this file with instructions.

---

## How to Build and Run (CPU)

### Building the CPU Image
1. Go to the directory containing `Dockerfile.cpu`.
2. Execute:
   ```bash
   docker build -f Dockerfile.cpu -t paraview-cpu .
   ```

### Running pvserver (CPU)
To run the container with software-based OSMesa rendering:

```bash
docker run -it --rm -p 11111:11111 paraview-cpu)
```

- `-p 11111:11111` maps container port 11111 to the host.
- You should see `Waiting for client...`, indicating the server is ready.

---

## How to Build and Run (CUDA)

### Building the CUDA Image
1. Go to the directory containing `Dockerfile.cuda`.
2. Execute:
   ```bash
   docker build -f Dockerfile.cuda -t paraview-cuda .
   ```

### Running pvserver (CUDA)
1. Verify you have **NVIDIA Container Toolkit** and GPU drivers installed.
2. Run:
   ```bash
   docker run --gpus all -it --rm -p 11111:11111 paraview-cuda
   ```

- `--gpus all` passes all available NVIDIA GPUs to the container.
- The server will start on port 11111 with GPU acceleration (EGL + VTK-m CUDA).

---

## Connecting to ParaView (Client-Server)
1. **Install ParaView (GUI)** on your local machine (Windows, macOS, or Linux).
2. Open **File → Connect...**:
   - Click **Add Server**.
   - Use the **Host** IP of the machine running Docker.
   - **Port**: `11111`.
   - Click **Connect**.
3. Now you can load data and perform remote rendering on the container.


---

## License
All Dockerfiles and scripts in this repository are distributed under the [MIT License](https://opensource.org/licenses/MIT). ParaView itself is licensed under a BSD-style license by Kitware. For NVIDIA CUDA images, refer to NVIDIA’s license terms. 
