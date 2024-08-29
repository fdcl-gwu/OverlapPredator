## Setup on Jetson Orin Nano

NOTE: jetson doesn't have enough RAM to train anything. Can only use pretrained weights on point cloud (i.e. the predator demo).

1. setup pip virtualenv
- create venv with ```$ virtualenv predator_env --python=python3.8```
- from ~/OverlapPredator, activate with ```$ source predator_env/bin/activate```
- ```$ pip install --upgrade pip```
- ```$ git clone https://github.com/overlappredator/OverlapPredator.git```

2. Install requirements
https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048

```
# torch with CUDA
$ wget https://nvidia.box.com/shared/static/p57jwntv436lfrd78inwl7iml6p13fzh.whl -O torch-1.$ 11.0-cp38-cp38-linux_aarch64.whl
$ sudo apt-get install python3-pip libopenblas-base libopenmpi-dev libomp-dev
$ pip3 install 'Cython<3'
$ pip3 install --no-cache torch-1.11.0-cp38-cp38-linux_aarch64.whl

# the new requirements.txt doesn't include torch or torchvision
$ pip install -r requirements.txt 

# torchvision
$ sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libopenblas-dev libavcodec-dev libavformat-dev libswscale-dev
$ git clone --branch v0.12.0 https://github.com/pytorch/vision torchvision
$ cd torchvision
$ export BUILD_VERSION=0.12.0
$ python3 setup.py install
$ cd ../ 

# compile cpp wrappers
$ cd cpp_wrappers
$ sh compile_wrappers.sh
$ cd ..
```
3.  Code modifications
- In ./lib/benchmark_utils.py:
   - add False argument after tgt_feats 
      - ```result_ransac = o3d.pipelines.registration.registration_ransac_based_on_feature_matching(src_pcd, tgt_pcd, src_feats, tgt_feats, False, distance_threshold,...```
   -  change all instances of ```o3d.registration``` to ```o3d.pipelines.registration```

4. run demo (may crash VSCode windows)
```$ python scripts/demo.py configs/test/indoor.yaml```



