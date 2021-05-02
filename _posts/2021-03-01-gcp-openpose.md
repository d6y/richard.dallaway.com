---
title: |
  Server-side OpenPose with cloud GPUs
author: 
layout: post
---

[OpenPose](https://cmu-perceptual-computing-lab.github.io/openpose/web/html/doc/) software detects bodies in video. It runs best on GPU hardware, including on servers. An easy way to set that up is with [Google’s Compute Platform](https://cloud.google.com/), which is a metered cloud platform that can be configured with GPU hardware. There’s a couple of tricks to know, but it does give significant speed-ups over non-GPU hardware.

<!-- break -->

## Launch a deep learning instance

Creating the GCP compute instance is straightforward enough, but for an easy life you want to select the specific Linux distribution: “[Deep learning on Linux](https://cloud.google.com/ai-platform/deep-learning-vm/docs/introduction)”. The default is (at the time of writing) Debian without any of the pre-configured GPU dependencies.

A type instance might be:
- us-central-1
- Series: N1
- GPU: [V100](https://www.nvidia.com/en-us/data-center/v100/)
- Deep learning on Linux (not the default Debian); 50 GB disk

Launch that (which will cost you money), and first time you log in you’ll be promoted to install the NVIDIA drivers.

## Install OpenPose

The second trick, which is specific for compatibility with OpenPose, is to remove Conda. Now, [Conda is a great tool for managing Python environments](https://docs.conda.io/en/latest/) but for OpenPose it includes [an incompatible version of Protobuf](https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/installation/1_prerequisites.md#ubuntu-prerequisites). 

To fix that, edit `/etc/profile.d/env.sh` and comment out:

	. "/opt/conda/etc/profile.d/conda.sh"
	conda activate base

…and restart your shell. For belt-and-braces I also took the nuclear option: `sudo mv /opt/conda /opt/conda.deactivated`.

The rest of the installation for OpenPose is as per the OpenPose docs. E.g.,

	git clone https://github.com/CMU-Perceptual-Computing-Lab/openpose.git
	cd openpose/
	git submodule update --init --recursive --remote
	
	
	sudo apt update
	sudo apt-get install -y libgoogle-glog-dev libboost-all-dev libatlas-base-dev libopencv-dev protobuf-compiler
	
	mkdir build && cd build
	cmake ..
	make -j`nproc`

## The result

Is it worth that to be able to run on a GPU? Yes: for me, it was a 20x speed up compared to my spicy (for Intel) Mac desktop machine. 

I find phrases like “20x speed up” a little too abstract to appreciate. In practice, it means something that took 30 minutes now takes 90 seconds.
