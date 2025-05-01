
# 🚀 Test YOLOv8 Object Detection Project  (failed)
*First Computer Vision Project | NVIDIA Driver Troubleshooting 

![YOLOv8](https://img.shields.io/badge/YOLOv8-Object%20Detection-blue)  
![NVIDIA](https://img.shields.io/badge/NVIDIA-GPU%20Driver%20535-green)  
![CUDA](https://img.shields.io/badge/CUDA-12.2-important)  

---

## 📁 Project Structure (VS Code)  
```bash
.
├── data/                  # Dataset (images/videos)
├── models/                # YOLOv8 model weights
├── outputs/               # Inference results
├── utils/                 # Helper scripts
├── train.py               # Training script
├── detect.py              # Inference script
└── README.md              # This file
---

## Problem Encountered

The system failed to utilize GPU acceleration due to:

    NVIDIA Driver Load Failure

        Symptoms:

            nvidia-smi not working

            CUDA operations defaulting to CPU

            Error logs indicating NVIDIA kernel module missing

        Root Causes:

            Leftover 550-series driver files conflicting with new installations.

            DKMS (Dynamic Kernel Module Support) failing to rebuild the NVIDIA kernel module.

            Secure Boot preventing unsigned driver loading.

    Version Mismatch

        Incompatibility between:

            NVIDIA driver (550 vs 535)

            CUDA (12.2)

            Linux kernel version

Solution Path
Step 1: Completely Remove Old Drivers
bash

sudo apt-get purge 'nvidia*'  
sudo apt-get autoremove  
sudo apt-get autoclean  
sudo rm -rf /usr/lib/nvidia* /etc/modprobe.d/nvidia*  

Step 2: Install Stable NVIDIA Driver (535)
bash

sudo add-apt-repository ppa:graphics-drivers/ppa  
sudo apt update  
sudo apt install nvidia-driver-535  

Step 3: Rebuild DKMS & Update Initramfs
bash

sudo dkms remove -m nvidia -v 550 --all  # Remove old version if lingering  
sudo dkms install -m nvidia -v 535  
sudo update-initramfs -u  
sudo reboot  

Step 4: Secure Boot Handling

If the driver still fails:

    Option 1: Disable Secure Boot (via BIOS/UEFI)

    Option 2: Enroll MOK (Machine Owner Key)

        Follow on-screen prompts after mokutil --import (if needed).

Step 5: Verify Installation
bash

nvidia-smi         
nvcc --version

## Lessons Learned

✅ Always purge old drivers completely before installing new ones.
✅ DKMS must be rebuilt after driver changes (sudo dkms install -m nvidia -v <version>).
✅ Secure Boot can silently block drivers—disable or enroll MOK if needed.
✅ Driver-CUDA compatibility matters—535 works best with CUDA 12.2.
✅ Log all steps for easier debugging in future projects.
