# Instaling NVIDIA drivers on Linux

Installing NVIDIA drivers on Linux is a bit of a pain. This is a guide on how to install NVIDIA drivers on Fedora 37.

- [Guide for fedora](https://www.if-not-true-then-false.com/2015/fedora-nvidia-guide/)

- [Blog post](https://discussion.fedoraproject.org/t/fedora-37-nvidia-kernel-module-missing-falling-back-to-nouveau/71372/6)

- [Optimus](http://download.nvidia.com/XFree86/Linux-x86_64/510.73.05/README/optimus.html)

- [Instalig for fedora with optimus](https://docs.fedoraproject.org/en-US/quick-docs/how-to-set-nvidia-as-primary-gpu-on-optimus-based-laptops/)
- [Comprehencive guide for CUDA instalation](https://docs.nvidia.com/cuda/pdf/CUDA_Installation_Guide_Linux.pdf)

## Prequisites

Instal rpmfusion [installing rpmfusion](https://rpmfusion.org/Configuration)

update the system

```bash
sudo dnf upgrade --refresh
sudo dnf remove *nvidia* --noautoremove --exclude=nvidia-gpu-firmware
```


