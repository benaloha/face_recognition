# Pictures
zet je foto's in data/known_people en data/images

# Howto CPU

# Howto GPU

## On host

-Nvidia cuda driver:  

https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#driver-installation  
sudo apt-get install cuda-drivers  

-Nvidia container toolkit  

sudo apt-get install -y nvidia-container-toolkitsudo  
systemctl restart docker  

## Docker

docker build -t face_recognition:gpu -f Dockerfile.gpu .

docker run -it --gpus all --rm -v /home/ben/projects/face_recognition/data:/data face_recognition:gpu face_recognition /data/known_people /data/images/ --show-distance True --tolerance 0.5

Dan foutmelding:  
RuntimeError: Click will abort further execution because Python 3 was configured to use ASCII as encoding for the environment. Consult https://click.palletsprojects.com/python3/ for mitigation steps.

This system supports the C.UTF-8 locale which is recommended. You might be able to resolve your issue by exporting the following environment variables:

    export LC_ALL=C.UTF-8
    export LANG=C.UTF-8

docker run -it --gpus all --rm -v /home/ben/projects/face_recognition/data:/data face_recognition:gpu bash

Dan in container en die exports doen:
face_recognition /data/known_people /data/images/ --show-distance True --tolerance 0.5

Met dit cmd zie je dan dat ie echt GPU gebruikt ipv CPU  
$nvidia-smi 

+-----------------------------------------------------------------------------+  
| NVIDIA-SMI 460.32.03    Driver Version: 460.32.03    CUDA Version: 11.2     |  
|-------------------------------+----------------------+----------------------+  
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |  
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |  
|                               |                      |               MIG M. |  
|===============================+======================+======================|  
|   0  GeForce GTX 750 Ti  On   | 00000000:01:00.0  On |                  N/A |  
| 30%   33C    P8     1W /  38W |    193MiB /  2000MiB |      2%      Default |  
|                               |                      |                  N/A |  
+-------------------------------+----------------------+----------------------+  
                                                                                 
+-----------------------------------------------------------------------------+  
| Processes:                                                                  |  
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |  
|        ID   ID                                                   Usage      |  
|=============================================================================|  
|    0   N/A  N/A      1466      G   /usr/lib/xorg/Xorg                 29MiB |  
|    0   N/A  N/A      2724      G   /usr/lib/xorg/Xorg                103MiB |  
|    0   N/A  N/A      2845      G   /usr/bin/gnome-shell               46MiB |  
|    0   N/A  N/A      7762      G   /usr/lib/firefox/firefox            1MiB |  


## Resultaat
M'n i7 CPU is vele malen sneller dan m'n oude GPU! Helaas..

