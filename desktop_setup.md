### Instructions for Remote Desktop over SSH
1. Setup environment
- ```$ ssh karl@161.253.72.9```
- Install python3.8, as well as ``$ sudo apt install python3.8-distutils`` (using deadsnake ppa).
- ``$ create venv with $ virtualenv predator_env --python=python3.8``
- from ~/OverlapPredator, activate with ``$ source predator_env/bin/activate``

2. Dependencies
- Follow README.md instructions
- ``$ pip install -r requirements.txt``
- `` $ sh scripts/download_data_weight.sh``

3. ExternalViewer Code Changes:
- demo.py
    - line 105: added external viewer option
    - line 50: read from .pcd instead of .pth
    - line 236: specify which src and tgt point clouds to use (hardcoded currently)

- NOTE: May need to upgrade Open3D from 0.10.0.0 to use ExternalVisualizer: ```$ pip install open3d==0.14.1``` (should auto-update numpy too)
- - __IF USING Open3D >= 0.14.1__, in ./lib/benchmark_utils.py,
  1. add False argument after tgt_feats 
      ```result_ransac = o3d.pipelines.registration.registration_ransac_based_on_feature_matching(src_pcd, tgt_pcd, src_feats, tgt_feats, False, distance_threshold,...```
  2. change ```o3d.registration``` to ```o3d.pipelines.registration```

4. Use External Visualizer
- In Jetson terminal 1 (in venv): ```$ python -m open3d.visualization --external-vis```
- In Jetson terminal 2 (in venv): ```$ ssh -R 51454:localhost:51454 karl@161.253.72.9```. This will ssh you into desktop computer.
- Re-activate the virtualenv you made on the desktop/remote computer.
- Run demo in terminal 2 (over ssh)``$ python scripts/demo.py configs/test/indoor.yaml``. You should have an Open3D window open showing the aligned ship point clouds.
    - This assumes you hardcoded the two ship .pcd files to align in demo.py