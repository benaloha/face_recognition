# Org README
see org README [here](README1.md)

# Pictures
zet je foto's in data/known_people en data/images

# Howto CPU
docker build -t face_recognition:cpu .

docker run -it --rm -v /home/ben/projects/face_recognition/data:/data face_recognition:cpu face_recognition /data/known_people /data/images/ --show-distance True --tolerance 0.5 --cpus -1

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

## Resultaat
M'n i7 CPU is vele malen sneller dan m'n oude GPU! Helaas..

