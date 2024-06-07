## Updating GPU Drivers

1. Install latest toolkit from nvidia's official site (installation instructions can be found there itself by selecting the OS and other machine specifications)
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.5.0/local_installers/cuda-repo-ubuntu2204-12-5-local_12.5.0-555.42.02-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2204-12-5-local_12.5.0-555.42.02-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2204-12-5-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-5
```

2. Install / Upgrade the Nvidia Drivers
```sudo apt-get install -y cuda-drivers```

3. Export 2 important environment variables:
```
export PATH=/usr/local/cuda-12.5/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-12.5/lib\
                         ${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

Note: `LD_LIBRARY_PATH` variable needs to be exported in a different way for 32 bit systems.

4. Reboot system to reflect changes in `nvcc`
```sudo reboot```

5. While building a few applications some errors were encountered related to the absence of environment variables. Given below are the additional environment variables which should (recommended) be added to the `~/.bashrc` in order to ensure proper inclusion of binaries and essential libraries.
```
# adding cuda binaries to environment
export PATH=/usr/local/cuda-12.5/bin:/usr/lib/nvidia-cuda-toolkit/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-12.5/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

# adding cuda related C and C++ libraries to environment
export CPLUS_INCLUDE_PATH=/usr/local/cuda-12.5/targets/x86_64-linux/include
export C_INCLUDE_PATH=/usr/local/cuda-12.5/targets/x86_64-linux/include

# adding known architecture versions to avoid errors
export TORCH_CUDA_ARCH_LIST="8.0 8.6 8.9 9.0"
```

**References**
- https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=deb_local
- https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#post-installation-actions
