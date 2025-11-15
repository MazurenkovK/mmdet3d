# mmdet3d
Первый проект по mmdet3d
#!/bin/bash

set -e

python3.10 -m venv .mmdet3d
source .mmdet3d/bin/activate

pip install numpy==1.26.0
pip install torch==2.0.1 torchvision==0.15.2 --index-url https://download.pytorch.org/whl/cu118
pip install mmcv==2.1.0 -f https://download.openmmlab.com/mmcv/dist/cu118/torch2.0/index.html

pip install -U openmim
mim install mmengine
mim install "mmdet>=3.0.0,<3.3.0"

# git clone https://github.com/open-mmlab/mmdetection3d.git
cd mmdetection3d
pip install -v -e .

python -c "import torch, mmcv, mmdet, mmdet3d; \
print(f'Torch {torch.__version__}, MMCV {mmcv.__version__}, MMDet {mmdet.__version__}, MMDet3D {mmdet3d.__version__}')"

mim download mmdet3d \
  --config pointpillars_hv_secfpn_8xb6-160e_kitti-3d-3class \
  --dest checkpoints

python3 demo/pcd_demo.py \
  demo/data/kitti/000008.bin \
  configs/pointpillars/pointpillars_hv_secfpn_8xb6-160e_kitti-3d-car.py \
  checkpoints/hv_pointpillars_secfpn_6x8_160e_kitti-3d-car_20220331_134606-d42d15ed.pth \
  --device cuda:0 \
  --show \
  --out-dir outputs

mim download mmdet3d \
  --config pointnet2_ssg_2xb16-cosine-200e_scannet-seg \
  --dest checkpoints

python3 demo/pcd_seg_demo.py \
  demo/data/scannet/scene0000_00.bin \
  configs/pointnet2/pointnet2_ssg_2xb16-cosine-200e_scannet-seg.py \
  checkpoints/pointnet2_ssg_16x2_cosine_200e_scannet_seg-3d-20class_20210514_143644-ee73704a.pth \
  --device cuda:0 \
  --show \
  --out-dir outputs


source .mmdet3d/bin/activate