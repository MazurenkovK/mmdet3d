# mmdet3d
```
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
```
### для mac os 
mim download mmdet3d --config pointnet2_ssg_2xb16-cosine-200e_scannet-seg --dest /workspaces/mmdet3d/mmdetection3d/checkpoints 2>&1 | grep -A 5 "downloading\|successfully"  

python3 demo/pcd_seg_demo.py \
  demo/data/scannet/scene0000_00.bin \
  configs/pointnet2/pointnet2_ssg_2xb16-cosine-200e_scannet-seg.py \
  checkpoints/pointnet2_ssg_16x2_cosine_200e_scannet_seg-3d-20class_20210514_143644-ee73704a.pth \
  --device cpu \
  --out-dir outputs

## мои изменения для mac os 

1й вариант предпочтительнее:
python3.10 -m pip install "torch==2.0.1+cpu" "torchvision==0.15.2+cpu" -f https://download.pytorch.org/whl/torch_stable.html

или
python3.10 -m pip install "torch==2.0.1" "torchvision==0.15.2" -f https://download.pytorch.org/whl/torch_stable.html

### загружаем конфиг для kitti с проверкой
mim download mmdet3d --config pointpillars_hv_secfpn_8xb6-160e_kitti-3d-car --dest /workspaces/mmdet3d/mmdetection3d/checkpoints 2>&1 | grep -A 5 "downloading\|successfully"

### создан новый demo_run.py для классификации в kitti

python demo_run.py \
  demo/data/kitti/000008.bin \
  configs/pointpillars/pointpillars_hv_secfpn_8xb6-160e_kitti-3d-car.py \
  checkpoints/hv_pointpillars_secfpn_6x8_160e_kitti-3d-car_20220331_134606-d42d15ed.pth \
  --device cpu \
  --out-dir outputs

mim download mmdet3d --config pointnet2_ssg_2xb16-cosine-200e_scannet-seg --dest /workspaces/mmdet3d/mmdetection3d/checkpoints 2>&1 | grep -A 5 "downloading\|successfully" 

## команды и вывод терминала для отчета по проекту

@MazurenkovK ➜ /workspaces/mmdet3d (main) $ python3.10 -m venv .mmdet3d
@MazurenkovK ➜ /workspaces/mmdet3d (main) $ source .mmdet3d/bin/activate
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip install numpy==1.26.0
Collecting numpy==1.26.0
  Using cached numpy-1.26.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.2 MB)
Installing collected packages: numpy
Successfully installed numpy-1.26.0

[notice] A new release of pip is available: 23.0.1 -> 25.3
[notice] To update, run: pip install --upgrade pip
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip install --upgrade pip
Requirement already satisfied: pip in ./.mmdet3d/lib/python3.10/site-packages (23.0.1)
Collecting pip
  Using cached pip-25.3-py3-none-any.whl (1.8 MB)
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 23.0.1
    Uninstalling pip-23.0.1:
      Successfully uninstalled pip-23.0.1
Successfully installed pip-25.3
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip install torch==2.0.1 torchvision==0.15.2 --index-url https://download.pytorch.org/whl/cu118
Looking in indexes: https://download.pytorch.org/whl/cu118
Collecting torch==2.0.1
  Downloading https://download.pytorch.org/whl/cu118/torch-2.0.1%2Bcu118-cp310-cp310-linux_x86_64.whl (2267.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.3/2.3 GB 9.3 MB/s  0:00:35
Collecting torchvision==0.15.2
  Downloading https://download.pytorch.org/whl/cu118/torchvision-0.15.2%2Bcu118-cp310-cp310-linux_x86_64.whl (6.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.1/6.1 MB 52.2 MB/s  0:00:00
Collecting filelock (from torch==2.0.1)
  Downloading https://download.pytorch.org/whl/filelock-3.19.1-py3-none-any.whl.metadata (2.1 kB)
Collecting typing-extensions (from torch==2.0.1)
  Downloading https://download.pytorch.org/whl/typing_extensions-4.15.0-py3-none-any.whl.metadata (3.3 kB)
Collecting sympy (from torch==2.0.1)
  Downloading https://download.pytorch.org/whl/sympy-1.14.0-py3-none-any.whl.metadata (12 kB)
Collecting networkx (from torch==2.0.1)
  Downloading https://download.pytorch.org/whl/networkx-3.5-py3-none-any.whl.metadata (6.3 kB)
Collecting jinja2 (from torch==2.0.1)
  Downloading https://download.pytorch.org/whl/jinja2-3.1.6-py3-none-any.whl.metadata (2.9 kB)
Collecting triton==2.0.0 (from torch==2.0.1)
  Downloading https://download.pytorch.org/whl/triton-2.0.0-1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (63.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 63.3/63.3 MB 61.7 MB/s  0:00:01
Requirement already satisfied: numpy in ./.mmdet3d/lib/python3.10/site-packages (from torchvision==0.15.2) (1.26.0)
Collecting requests (from torchvision==0.15.2)
  Downloading https://download.pytorch.org/whl/requests-2.28.1-py3-none-any.whl (62 kB)
Collecting pillow!=8.3.*,>=5.3.0 (from torchvision==0.15.2)
  Downloading https://download.pytorch.org/whl/pillow-11.3.0-cp310-cp310-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl.metadata (9.0 kB)
Collecting cmake (from triton==2.0.0->torch==2.0.1)
  Downloading https://download.pytorch.org/whl/cmake-3.25.0-py2.py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (23.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 23.7/23.7 MB 58.7 MB/s  0:00:00
Collecting lit (from triton==2.0.0->torch==2.0.1)
  Downloading https://download.pytorch.org/whl/lit-15.0.7.tar.gz (132 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting MarkupSafe>=2.0 (from jinja2->torch==2.0.1)
  Downloading https://download.pytorch.org/whl/MarkupSafe-2.1.5-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
INFO: pip is looking at multiple versions of networkx to determine which version is compatible with other requirements. This could take a while.
Collecting networkx (from torch==2.0.1)
  Downloading https://download.pytorch.org/whl/networkx-3.3-py3-none-any.whl.metadata (5.1 kB)
Collecting charset-normalizer<3,>=2 (from requests->torchvision==0.15.2)
  Downloading https://download.pytorch.org/whl/charset_normalizer-2.1.1-py3-none-any.whl (39 kB)
Collecting idna<4,>=2.5 (from requests->torchvision==0.15.2)
  Downloading https://download.pytorch.org/whl/idna-3.4-py3-none-any.whl (61 kB)
Collecting urllib3<1.27,>=1.21.1 (from requests->torchvision==0.15.2)
  Downloading https://download.pytorch.org/whl/urllib3-1.26.13-py2.py3-none-any.whl (140 kB)
Collecting certifi>=2017.4.17 (from requests->torchvision==0.15.2)
  Downloading https://download.pytorch.org/whl/certifi-2022.12.7-py3-none-any.whl (155 kB)
Collecting mpmath<1.4,>=1.1.0 (from sympy->torch==2.0.1)
  Downloading https://download.pytorch.org/whl/mpmath-1.3.0-py3-none-any.whl (536 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 536.2/536.2 kB 22.3 MB/s  0:00:00
Downloading https://download.pytorch.org/whl/pillow-11.3.0-cp310-cp310-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl (6.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.6/6.6 MB 53.0 MB/s  0:00:00
Downloading https://download.pytorch.org/whl/filelock-3.19.1-py3-none-any.whl (15 kB)
Downloading https://download.pytorch.org/whl/jinja2-3.1.6-py3-none-any.whl (134 kB)
Downloading https://download.pytorch.org/whl/networkx-3.3-py3-none-any.whl (1.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.7/1.7 MB 50.5 MB/s  0:00:00
Downloading https://download.pytorch.org/whl/sympy-1.14.0-py3-none-any.whl (6.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.3/6.3 MB 59.6 MB/s  0:00:00
Downloading https://download.pytorch.org/whl/typing_extensions-4.15.0-py3-none-any.whl (44 kB)
Building wheels for collected packages: lit
  Building wheel for lit (pyproject.toml) ... done
  Created wheel for lit: filename=lit-15.0.7-py3-none-any.whl size=89991 sha256=40587098d9a8429d1d6e9fa8a5442aba62fa623191701335ffaa1297c64ccbc0
  Stored in directory: /home/codespace/.cache/pip/wheels/27/2c/b6/3ed2983b1b44fe0dea1bb35234b09f2c22fb8ebb308679c922
Successfully built lit
Installing collected packages: mpmath, lit, cmake, urllib3, typing-extensions, sympy, pillow, networkx, MarkupSafe, idna, filelock, charset-normalizer, certifi, requests, jinja2, triton, torch, torchvision
Successfully installed MarkupSafe-2.1.5 certifi-2022.12.7 charset-normalizer-2.1.1 cmake-3.25.0 filelock-3.19.1 idna-3.4 jinja2-3.1.6 lit-15.0.7 mpmath-1.3.0 networkx-3.3 pillow-11.3.0 requests-2.28.1 sympy-1.14.0 torch-2.0.1+cu118 torchvision-0.15.2+cu118 triton-2.0.0 typing-extensions-4.15.0 urllib3-1.26.13
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip install mmcv==2.1.0 -f https://download.openmmlab.com/mmcv/dist/cu118/torch2.0/index.html
Looking in links: https://download.openmmlab.com/mmcv/dist/cu118/torch2.0/index.html
Collecting mmcv==2.1.0
  Downloading https://download.openmmlab.com/mmcv/dist/cu118/torch2.0.0/mmcv-2.1.0-cp310-cp310-manylinux1_x86_64.whl (98.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 98.6/98.6 MB 44.5 MB/s  0:00:02
Collecting addict (from mmcv==2.1.0)
  Downloading addict-2.4.0-py3-none-any.whl.metadata (1.0 kB)
Collecting mmengine>=0.3.0 (from mmcv==2.1.0)
  Downloading mmengine-0.10.7-py3-none-any.whl.metadata (20 kB)
Requirement already satisfied: numpy in ./.mmdet3d/lib/python3.10/site-packages (from mmcv==2.1.0) (1.26.0)
Collecting packaging (from mmcv==2.1.0)
  Downloading packaging-25.0-py3-none-any.whl.metadata (3.3 kB)
Requirement already satisfied: Pillow in ./.mmdet3d/lib/python3.10/site-packages (from mmcv==2.1.0) (11.3.0)
Collecting pyyaml (from mmcv==2.1.0)
  Downloading pyyaml-6.0.3-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl.metadata (2.4 kB)
Collecting yapf (from mmcv==2.1.0)
  Downloading yapf-0.43.0-py3-none-any.whl.metadata (46 kB)
Collecting opencv-python>=3 (from mmcv==2.1.0)
  Downloading opencv_python-4.12.0.88-cp37-abi3-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (19 kB)
Collecting matplotlib (from mmengine>=0.3.0->mmcv==2.1.0)
  Downloading matplotlib-3.10.7-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (11 kB)
Collecting rich (from mmengine>=0.3.0->mmcv==2.1.0)
  Downloading rich-14.2.0-py3-none-any.whl.metadata (18 kB)
Collecting termcolor (from mmengine>=0.3.0->mmcv==2.1.0)
  Downloading termcolor-3.2.0-py3-none-any.whl.metadata (6.4 kB)
Collecting numpy (from mmcv==2.1.0)
  Downloading numpy-2.2.6-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (62 kB)
Collecting contourpy>=1.0.1 (from matplotlib->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading contourpy-1.3.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (5.5 kB)
Collecting cycler>=0.10 (from matplotlib->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading cycler-0.12.1-py3-none-any.whl.metadata (3.8 kB)
Collecting fonttools>=4.22.0 (from matplotlib->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading fonttools-4.60.1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (112 kB)
Collecting kiwisolver>=1.3.1 (from matplotlib->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading kiwisolver-1.4.9-cp310-cp310-manylinux_2_12_x86_64.manylinux2010_x86_64.whl.metadata (6.3 kB)
Collecting pyparsing>=3 (from matplotlib->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading pyparsing-3.2.5-py3-none-any.whl.metadata (5.0 kB)
Collecting python-dateutil>=2.7 (from matplotlib->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading python_dateutil-2.9.0.post0-py2.py3-none-any.whl.metadata (8.4 kB)
Collecting six>=1.5 (from python-dateutil>=2.7->matplotlib->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading six-1.17.0-py2.py3-none-any.whl.metadata (1.7 kB)
Collecting markdown-it-py>=2.2.0 (from rich->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading markdown_it_py-4.0.0-py3-none-any.whl.metadata (7.3 kB)
Collecting pygments<3.0.0,>=2.13.0 (from rich->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading pygments-2.19.2-py3-none-any.whl.metadata (2.5 kB)
Collecting mdurl~=0.1 (from markdown-it-py>=2.2.0->rich->mmengine>=0.3.0->mmcv==2.1.0)
  Downloading mdurl-0.1.2-py3-none-any.whl.metadata (1.6 kB)
Collecting platformdirs>=3.5.1 (from yapf->mmcv==2.1.0)
  Downloading platformdirs-4.5.0-py3-none-any.whl.metadata (12 kB)
Collecting tomli>=2.0.1 (from yapf->mmcv==2.1.0)
  Downloading tomli-2.3.0-py3-none-any.whl.metadata (10 kB)
Downloading mmengine-0.10.7-py3-none-any.whl (452 kB)
Downloading opencv_python-4.12.0.88-cp37-abi3-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (67.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 67.0/67.0 MB 47.9 MB/s  0:00:01
Downloading numpy-2.2.6-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (16.8 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 16.8/16.8 MB 34.7 MB/s  0:00:00
Downloading addict-2.4.0-py3-none-any.whl (3.8 kB)
Downloading matplotlib-3.10.7-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (8.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.7/8.7 MB 44.9 MB/s  0:00:00
Downloading contourpy-1.3.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (325 kB)
Downloading cycler-0.12.1-py3-none-any.whl (8.3 kB)
Downloading fonttools-4.60.1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (4.8 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.8/4.8 MB 39.5 MB/s  0:00:00
Downloading kiwisolver-1.4.9-cp310-cp310-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (1.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.6/1.6 MB 33.1 MB/s  0:00:00
Downloading packaging-25.0-py3-none-any.whl (66 kB)
Downloading pyparsing-3.2.5-py3-none-any.whl (113 kB)
Downloading python_dateutil-2.9.0.post0-py2.py3-none-any.whl (229 kB)
Downloading six-1.17.0-py2.py3-none-any.whl (11 kB)
Downloading pyyaml-6.0.3-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl (770 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 770.3/770.3 kB 29.6 MB/s  0:00:00
Downloading rich-14.2.0-py3-none-any.whl (243 kB)
Downloading pygments-2.19.2-py3-none-any.whl (1.2 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.2/1.2 MB 42.2 MB/s  0:00:00
Downloading markdown_it_py-4.0.0-py3-none-any.whl (87 kB)
Downloading mdurl-0.1.2-py3-none-any.whl (10.0 kB)
Downloading termcolor-3.2.0-py3-none-any.whl (7.7 kB)
Downloading yapf-0.43.0-py3-none-any.whl (256 kB)
Downloading platformdirs-4.5.0-py3-none-any.whl (18 kB)
Downloading tomli-2.3.0-py3-none-any.whl (14 kB)
Installing collected packages: addict, tomli, termcolor, six, pyyaml, pyparsing, pygments, platformdirs, packaging, numpy, mdurl, kiwisolver, fonttools, cycler, yapf, python-dateutil, opencv-python, markdown-it-py, contourpy, rich, matplotlib, mmengine, mmcv
  Attempting uninstall: numpy
    Found existing installation: numpy 1.26.0
    Uninstalling numpy-1.26.0:
      Successfully uninstalled numpy-1.26.0
Successfully installed addict-2.4.0 contourpy-1.3.2 cycler-0.12.1 fonttools-4.60.1 kiwisolver-1.4.9 markdown-it-py-4.0.0 matplotlib-3.10.7 mdurl-0.1.2 mmcv-2.1.0 mmengine-0.10.7 numpy-2.2.6 opencv-python-4.12.0.88 packaging-25.0 platformdirs-4.5.0 pygments-2.19.2 pyparsing-3.2.5 python-dateutil-2.9.0.post0 pyyaml-6.0.3 rich-14.2.0 six-1.17.0 termcolor-3.2.0 tomli-2.3.0 yapf-0.43.0
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip list
Package            Version
------------------ ------------
addict             2.4.0
certifi            2022.12.7
charset-normalizer 2.1.1
cmake              3.25.0
contourpy          1.3.2
cycler             0.12.1
filelock           3.19.1
fonttools          4.60.1
idna               3.4
Jinja2             3.1.6
kiwisolver         1.4.9
lit                15.0.7
markdown-it-py     4.0.0
MarkupSafe         2.1.5
matplotlib         3.10.7
mdurl              0.1.2
mmcv               2.1.0
mmengine           0.10.7
mpmath             1.3.0
networkx           3.3
numpy              2.2.6
opencv-python      4.12.0.88
packaging          25.0
pillow             11.3.0
pip                25.3
platformdirs       4.5.0
Pygments           2.19.2
pyparsing          3.2.5
python-dateutil    2.9.0.post0
PyYAML             6.0.3
requests           2.28.1
rich               14.2.0
setuptools         79.0.1
six                1.17.0
sympy              1.14.0
termcolor          3.2.0
tomli              2.3.0
torch              2.0.1+cu118
torchvision        0.15.2+cu118
triton             2.0.0
typing_extensions  4.15.0
urllib3            1.26.13
yapf               0.43.0
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip install numpy==1.26.0
Collecting numpy==1.26.0
  Downloading numpy-1.26.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (58 kB)
Downloading numpy-1.26.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.2 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 18.2/18.2 MB 43.1 MB/s  0:00:00
Installing collected packages: numpy
  Attempting uninstall: numpy
    Found existing installation: numpy 2.2.6
    Uninstalling numpy-2.2.6:
      Successfully uninstalled numpy-2.2.6
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
opencv-python 4.12.0.88 requires numpy<2.3.0,>=2; python_version >= "3.9", but you have numpy 1.26.0 which is incompatible.
Successfully installed numpy-1.26.0
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip install -U openmim
Collecting openmim
  Downloading openmim-0.3.9-py2.py3-none-any.whl.metadata (16 kB)
Collecting Click (from openmim)
  Downloading click-8.3.1-py3-none-any.whl.metadata (2.6 kB)
Collecting colorama (from openmim)
  Downloading colorama-0.4.6-py2.py3-none-any.whl.metadata (17 kB)
Collecting model-index (from openmim)
  Downloading model_index-0.1.11-py3-none-any.whl.metadata (3.9 kB)
Collecting opendatalab (from openmim)
  Downloading opendatalab-0.0.10-py3-none-any.whl.metadata (6.4 kB)
Collecting pandas (from openmim)
  Downloading pandas-2.3.3-cp310-cp310-manylinux_2_24_x86_64.manylinux_2_28_x86_64.whl.metadata (91 kB)
Requirement already satisfied: pip>=19.3 in ./.mmdet3d/lib/python3.10/site-packages (from openmim) (25.3)
Requirement already satisfied: requests in ./.mmdet3d/lib/python3.10/site-packages (from openmim) (2.28.1)
Requirement already satisfied: rich in ./.mmdet3d/lib/python3.10/site-packages (from openmim) (14.2.0)
Collecting tabulate (from openmim)
  Downloading tabulate-0.9.0-py3-none-any.whl.metadata (34 kB)
Requirement already satisfied: pyyaml in ./.mmdet3d/lib/python3.10/site-packages (from model-index->openmim) (6.0.3)
Collecting markdown (from model-index->openmim)
  Downloading markdown-3.10-py3-none-any.whl.metadata (5.1 kB)
Collecting ordered-set (from model-index->openmim)
  Downloading ordered_set-4.1.0-py3-none-any.whl.metadata (5.3 kB)
Collecting pycryptodome (from opendatalab->openmim)
  Downloading pycryptodome-3.23.0-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (3.4 kB)
Collecting tqdm (from opendatalab->openmim)
  Downloading tqdm-4.67.1-py3-none-any.whl.metadata (57 kB)
Collecting openxlab (from opendatalab->openmim)
  Downloading openxlab-0.1.3-py3-none-any.whl.metadata (3.8 kB)
Requirement already satisfied: charset-normalizer<3,>=2 in ./.mmdet3d/lib/python3.10/site-packages (from requests->openmim) (2.1.1)
Requirement already satisfied: idna<4,>=2.5 in ./.mmdet3d/lib/python3.10/site-packages (from requests->openmim) (3.4)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in ./.mmdet3d/lib/python3.10/site-packages (from requests->openmim) (1.26.13)
Requirement already satisfied: certifi>=2017.4.17 in ./.mmdet3d/lib/python3.10/site-packages (from requests->openmim) (2022.12.7)
Collecting filelock~=3.14.0 (from openxlab->opendatalab->openmim)
  Downloading filelock-3.14.0-py3-none-any.whl.metadata (2.8 kB)
Collecting oss2~=2.17.0 (from openxlab->opendatalab->openmim)
  Downloading oss2-2.17.0.tar.gz (259 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting packaging~=24.0 (from openxlab->opendatalab->openmim)
  Downloading packaging-24.2-py3-none-any.whl.metadata (3.2 kB)
Collecting pytz~=2023.3 (from openxlab->opendatalab->openmim)
  Downloading pytz-2023.4-py2.py3-none-any.whl.metadata (22 kB)
Collecting requests (from openmim)
  Downloading requests-2.28.2-py3-none-any.whl.metadata (4.6 kB)
Collecting rich (from openmim)
  Downloading rich-13.4.2-py3-none-any.whl.metadata (18 kB)
Collecting setuptools~=60.2.0 (from openxlab->opendatalab->openmim)
  Downloading setuptools-60.2.0-py3-none-any.whl.metadata (5.1 kB)
Collecting tqdm (from opendatalab->openmim)
  Downloading tqdm-4.65.2-py3-none-any.whl.metadata (56 kB)
Collecting crcmod>=1.7 (from oss2~=2.17.0->openxlab->opendatalab->openmim)
  Downloading crcmod-1.7.tar.gz (89 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting aliyun-python-sdk-kms>=2.4.1 (from oss2~=2.17.0->openxlab->opendatalab->openmim)
  Downloading aliyun_python_sdk_kms-2.16.5-py2.py3-none-any.whl.metadata (1.5 kB)
Collecting aliyun-python-sdk-core>=2.13.12 (from oss2~=2.17.0->openxlab->opendatalab->openmim)
  Downloading aliyun-python-sdk-core-2.16.0.tar.gz (449 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Requirement already satisfied: six in ./.mmdet3d/lib/python3.10/site-packages (from oss2~=2.17.0->openxlab->opendatalab->openmim) (1.17.0)
Requirement already satisfied: markdown-it-py>=2.2.0 in ./.mmdet3d/lib/python3.10/site-packages (from rich->openmim) (4.0.0)
Requirement already satisfied: pygments<3.0.0,>=2.13.0 in ./.mmdet3d/lib/python3.10/site-packages (from rich->openmim) (2.19.2)
Collecting jmespath<1.0.0,>=0.9.3 (from aliyun-python-sdk-core>=2.13.12->oss2~=2.17.0->openxlab->opendatalab->openmim)
  Downloading jmespath-0.10.0-py2.py3-none-any.whl.metadata (8.0 kB)
Collecting cryptography>=3.0.0 (from aliyun-python-sdk-core>=2.13.12->oss2~=2.17.0->openxlab->opendatalab->openmim)
  Downloading cryptography-46.0.3-cp38-abi3-manylinux_2_34_x86_64.whl.metadata (5.7 kB)
Collecting cffi>=2.0.0 (from cryptography>=3.0.0->aliyun-python-sdk-core>=2.13.12->oss2~=2.17.0->openxlab->opendatalab->openmim)
  Downloading cffi-2.0.0-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (2.6 kB)
Requirement already satisfied: typing-extensions>=4.13.2 in ./.mmdet3d/lib/python3.10/site-packages (from cryptography>=3.0.0->aliyun-python-sdk-core>=2.13.12->oss2~=2.17.0->openxlab->opendatalab->openmim) (4.15.0)
Collecting pycparser (from cffi>=2.0.0->cryptography>=3.0.0->aliyun-python-sdk-core>=2.13.12->oss2~=2.17.0->openxlab->opendatalab->openmim)
  Downloading pycparser-2.23-py3-none-any.whl.metadata (993 bytes)
Requirement already satisfied: mdurl~=0.1 in ./.mmdet3d/lib/python3.10/site-packages (from markdown-it-py>=2.2.0->rich->openmim) (0.1.2)
Requirement already satisfied: numpy>=1.22.4 in ./.mmdet3d/lib/python3.10/site-packages (from pandas->openmim) (1.26.0)
Requirement already satisfied: python-dateutil>=2.8.2 in ./.mmdet3d/lib/python3.10/site-packages (from pandas->openmim) (2.9.0.post0)
Collecting tzdata>=2022.7 (from pandas->openmim)
  Downloading tzdata-2025.2-py2.py3-none-any.whl.metadata (1.4 kB)
Downloading openmim-0.3.9-py2.py3-none-any.whl (52 kB)
Downloading click-8.3.1-py3-none-any.whl (108 kB)
Downloading colorama-0.4.6-py2.py3-none-any.whl (25 kB)
Downloading model_index-0.1.11-py3-none-any.whl (34 kB)
Downloading markdown-3.10-py3-none-any.whl (107 kB)
Downloading opendatalab-0.0.10-py3-none-any.whl (29 kB)
Downloading openxlab-0.1.3-py3-none-any.whl (314 kB)
Downloading filelock-3.14.0-py3-none-any.whl (12 kB)
Downloading packaging-24.2-py3-none-any.whl (65 kB)
Downloading pytz-2023.4-py2.py3-none-any.whl (506 kB)
Downloading requests-2.28.2-py3-none-any.whl (62 kB)
Downloading rich-13.4.2-py3-none-any.whl (239 kB)
Downloading setuptools-60.2.0-py3-none-any.whl (953 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 953.1/953.1 kB 19.0 MB/s  0:00:00
Downloading tqdm-4.65.2-py3-none-any.whl (77 kB)
Downloading jmespath-0.10.0-py2.py3-none-any.whl (24 kB)
Downloading aliyun_python_sdk_kms-2.16.5-py2.py3-none-any.whl (99 kB)
Downloading cryptography-46.0.3-cp38-abi3-manylinux_2_34_x86_64.whl (4.5 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.5/4.5 MB 46.7 MB/s  0:00:00
Downloading cffi-2.0.0-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (216 kB)
Downloading pycryptodome-3.23.0-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.3/2.3 MB 45.8 MB/s  0:00:00
Downloading ordered_set-4.1.0-py3-none-any.whl (7.6 kB)
Downloading pandas-2.3.3-cp310-cp310-manylinux_2_24_x86_64.manylinux_2_28_x86_64.whl (12.8 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 12.8/12.8 MB 54.7 MB/s  0:00:00
Downloading tzdata-2025.2-py2.py3-none-any.whl (347 kB)
Downloading pycparser-2.23-py3-none-any.whl (118 kB)
Downloading tabulate-0.9.0-py3-none-any.whl (35 kB)
Building wheels for collected packages: oss2, aliyun-python-sdk-core, crcmod
  Building wheel for oss2 (pyproject.toml) ... done
  Created wheel for oss2: filename=oss2-2.17.0-py3-none-any.whl size=112465 sha256=97417e14b863cbf60b781fb9bc242d18a6734a53c743cbc80c20ca4ea345ac1d
  Stored in directory: /home/codespace/.cache/pip/wheels/87/04/7b/7e61b8157fdf211c5131375240d0d86ca82e2a88ead9672c88
  Building wheel for aliyun-python-sdk-core (pyproject.toml) ... done
  Created wheel for aliyun-python-sdk-core: filename=aliyun_python_sdk_core-2.16.0-py3-none-any.whl size=535417 sha256=53bbf691b8360ed80cc2941e4c63fece0cf168a75be4e3e51860c257468ad9c3
  Stored in directory: /home/codespace/.cache/pip/wheels/35/11/5e/08e7cb4e03a3e83b4862edd12d1143c8d3936a3dd57a3ee46d
  Building wheel for crcmod (pyproject.toml) ... done
  Created wheel for crcmod: filename=crcmod-1.7-py3-none-any.whl size=18910 sha256=b3fa2953aa2ce93b8c2d34615665766ecddfffbb50941162305043c9050d8b24
  Stored in directory: /home/codespace/.cache/pip/wheels/85/4c/07/72215c529bd59d67e3dac29711d7aba1b692f543c808ba9e86
Successfully built oss2 aliyun-python-sdk-core crcmod
Installing collected packages: pytz, crcmod, tzdata, tqdm, tabulate, setuptools, requests, pycryptodome, pycparser, packaging, ordered-set, markdown, jmespath, filelock, colorama, Click, rich, pandas, model-index, cffi, cryptography, aliyun-python-sdk-core, aliyun-python-sdk-kms, oss2, openxlab, opendatalab, openmim
  Attempting uninstall: setuptools
    Found existing installation: setuptools 79.0.1
    Uninstalling setuptools-79.0.1:
      Successfully uninstalled setuptools-79.0.1
  Attempting uninstall: requests
    Found existing installation: requests 2.28.1
    Uninstalling requests-2.28.1:
      Successfully uninstalled requests-2.28.1
  Attempting uninstall: packaging
    Found existing installation: packaging 25.0
    Uninstalling packaging-25.0:
      Successfully uninstalled packaging-25.0
  Attempting uninstall: filelock
    Found existing installation: filelock 3.19.1
    Uninstalling filelock-3.19.1:
      Successfully uninstalled filelock-3.19.1
  Attempting uninstall: rich
    Found existing installation: rich 14.2.0
    Uninstalling rich-14.2.0:
      Successfully uninstalled rich-14.2.0
Successfully installed Click-8.3.1 aliyun-python-sdk-core-2.16.0 aliyun-python-sdk-kms-2.16.5 cffi-2.0.0 colorama-0.4.6 crcmod-1.7 cryptography-46.0.3 filelock-3.14.0 jmespath-0.10.0 markdown-3.10 model-index-0.1.11 opendatalab-0.0.10 openmim-0.3.9 openxlab-0.1.3 ordered-set-4.1.0 oss2-2.17.0 packaging-24.2 pandas-2.3.3 pycparser-2.23 pycryptodome-3.23.0 pytz-2023.4 requests-2.28.2 rich-13.4.2 setuptools-60.2.0 tabulate-0.9.0 tqdm-4.65.2 tzdata-2025.2
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ mim install mmengine
Looking in links: https://download.openmmlab.com/mmcv/dist/cu118/torch2.0.0/index.html
Requirement already satisfied: mmengine in ./.mmdet3d/lib/python3.10/site-packages (0.10.7)
Requirement already satisfied: addict in ./.mmdet3d/lib/python3.10/site-packages (from mmengine) (2.4.0)
Requirement already satisfied: matplotlib in ./.mmdet3d/lib/python3.10/site-packages (from mmengine) (3.10.7)
Requirement already satisfied: numpy in ./.mmdet3d/lib/python3.10/site-packages (from mmengine) (1.26.0)
Requirement already satisfied: pyyaml in ./.mmdet3d/lib/python3.10/site-packages (from mmengine) (6.0.3)
Requirement already satisfied: rich in ./.mmdet3d/lib/python3.10/site-packages (from mmengine) (13.4.2)
Requirement already satisfied: termcolor in ./.mmdet3d/lib/python3.10/site-packages (from mmengine) (3.2.0)
Requirement already satisfied: yapf in ./.mmdet3d/lib/python3.10/site-packages (from mmengine) (0.43.0)
Requirement already satisfied: opencv-python>=3 in ./.mmdet3d/lib/python3.10/site-packages (from mmengine) (4.12.0.88)
Collecting numpy (from mmengine)
  Using cached numpy-2.2.6-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (62 kB)
Requirement already satisfied: contourpy>=1.0.1 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmengine) (1.3.2)
Requirement already satisfied: cycler>=0.10 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmengine) (0.12.1)
Requirement already satisfied: fonttools>=4.22.0 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmengine) (4.60.1)
Requirement already satisfied: kiwisolver>=1.3.1 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmengine) (1.4.9)
Requirement already satisfied: packaging>=20.0 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmengine) (24.2)
Requirement already satisfied: pillow>=8 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmengine) (11.3.0)
Requirement already satisfied: pyparsing>=3 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmengine) (3.2.5)
Requirement already satisfied: python-dateutil>=2.7 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmengine) (2.9.0.post0)
Requirement already satisfied: six>=1.5 in ./.mmdet3d/lib/python3.10/site-packages (from python-dateutil>=2.7->matplotlib->mmengine) (1.17.0)
Requirement already satisfied: markdown-it-py>=2.2.0 in ./.mmdet3d/lib/python3.10/site-packages (from rich->mmengine) (4.0.0)
Requirement already satisfied: pygments<3.0.0,>=2.13.0 in ./.mmdet3d/lib/python3.10/site-packages (from rich->mmengine) (2.19.2)
Requirement already satisfied: mdurl~=0.1 in ./.mmdet3d/lib/python3.10/site-packages (from markdown-it-py>=2.2.0->rich->mmengine) (0.1.2)
Requirement already satisfied: platformdirs>=3.5.1 in ./.mmdet3d/lib/python3.10/site-packages (from yapf->mmengine) (4.5.0)
Requirement already satisfied: tomli>=2.0.1 in ./.mmdet3d/lib/python3.10/site-packages (from yapf->mmengine) (2.3.0)
Using cached numpy-2.2.6-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (16.8 MB)
Installing collected packages: numpy
  Attempting uninstall: numpy
    Found existing installation: numpy 1.26.0
    Uninstalling numpy-1.26.0:
      Successfully uninstalled numpy-1.26.0
Successfully installed numpy-2.2.6
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip list
Package                Version
---------------------- ------------
addict                 2.4.0
aliyun-python-sdk-core 2.16.0
aliyun-python-sdk-kms  2.16.5
certifi                2022.12.7
cffi                   2.0.0
charset-normalizer     2.1.1
click                  8.3.1
cmake                  3.25.0
colorama               0.4.6
contourpy              1.3.2
crcmod                 1.7
cryptography           46.0.3
cycler                 0.12.1
filelock               3.14.0
fonttools              4.60.1
idna                   3.4
Jinja2                 3.1.6
jmespath               0.10.0
kiwisolver             1.4.9
lit                    15.0.7
Markdown               3.10
markdown-it-py         4.0.0
MarkupSafe             2.1.5
matplotlib             3.10.7
mdurl                  0.1.2
mmcv                   2.1.0
mmengine               0.10.7
model-index            0.1.11
mpmath                 1.3.0
networkx               3.3
numpy                  2.2.6
opencv-python          4.12.0.88
opendatalab            0.0.10
openmim                0.3.9
openxlab               0.1.3
ordered-set            4.1.0
oss2                   2.17.0
packaging              24.2
pandas                 2.3.3
pillow                 11.3.0
pip                    25.3
platformdirs           4.5.0
pycparser              2.23
pycryptodome           3.23.0
Pygments               2.19.2
pyparsing              3.2.5
python-dateutil        2.9.0.post0
pytz                   2023.4
PyYAML                 6.0.3
requests               2.28.2
rich                   13.4.2
setuptools             60.2.0
six                    1.17.0
sympy                  1.14.0
tabulate               0.9.0
termcolor              3.2.0
tomli                  2.3.0
torch                  2.0.1+cu118
torchvision            0.15.2+cu118
tqdm                   4.65.2
triton                 2.0.0
typing_extensions      4.15.0
tzdata                 2025.2
urllib3                1.26.13
yapf                   0.43.0
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip install numpy==1.26.0
Collecting numpy==1.26.0
  Using cached numpy-1.26.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (58 kB)
Using cached numpy-1.26.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.2 MB)
Installing collected packages: numpy
  Attempting uninstall: numpy
    Found existing installation: numpy 2.2.6
    Uninstalling numpy-2.2.6:
      Successfully uninstalled numpy-2.2.6
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
opencv-python 4.12.0.88 requires numpy<2.3.0,>=2; python_version >= "3.9", but you have numpy 1.26.0 which is incompatible.
Successfully installed numpy-1.26.0
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ mim install "mmdet>=3.0.0,<3.3.0"
Looking in links: https://download.openmmlab.com/mmcv/dist/cu118/torch2.0.0/index.html
Collecting mmdet<3.3.0,>=3.0.0
  Downloading mmdet-3.2.0-py3-none-any.whl.metadata (32 kB)
Requirement already satisfied: matplotlib in ./.mmdet3d/lib/python3.10/site-packages (from mmdet<3.3.0,>=3.0.0) (3.10.7)
Requirement already satisfied: numpy in ./.mmdet3d/lib/python3.10/site-packages (from mmdet<3.3.0,>=3.0.0) (1.26.0)
Collecting pycocotools (from mmdet<3.3.0,>=3.0.0)
  Downloading pycocotools-2.0.10-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (1.3 kB)
Collecting scipy (from mmdet<3.3.0,>=3.0.0)
  Downloading scipy-1.15.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (61 kB)
Collecting shapely (from mmdet<3.3.0,>=3.0.0)
  Downloading shapely-2.1.2-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (6.8 kB)
Requirement already satisfied: six in ./.mmdet3d/lib/python3.10/site-packages (from mmdet<3.3.0,>=3.0.0) (1.17.0)
Collecting terminaltables (from mmdet<3.3.0,>=3.0.0)
  Downloading terminaltables-3.1.10-py2.py3-none-any.whl.metadata (3.5 kB)
Requirement already satisfied: tqdm in ./.mmdet3d/lib/python3.10/site-packages (from mmdet<3.3.0,>=3.0.0) (4.65.2)
Ignoring mmcv: markers 'extra == "mim"' don't match your environment
Ignoring mmengine: markers 'extra == "mim"' don't match your environment
Requirement already satisfied: contourpy>=1.0.1 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmdet<3.3.0,>=3.0.0) (1.3.2)
Requirement already satisfied: cycler>=0.10 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmdet<3.3.0,>=3.0.0) (0.12.1)
Requirement already satisfied: fonttools>=4.22.0 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmdet<3.3.0,>=3.0.0) (4.60.1)
Requirement already satisfied: kiwisolver>=1.3.1 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmdet<3.3.0,>=3.0.0) (1.4.9)
Requirement already satisfied: packaging>=20.0 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmdet<3.3.0,>=3.0.0) (24.2)
Requirement already satisfied: pillow>=8 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmdet<3.3.0,>=3.0.0) (11.3.0)
Requirement already satisfied: pyparsing>=3 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmdet<3.3.0,>=3.0.0) (3.2.5)
Requirement already satisfied: python-dateutil>=2.7 in ./.mmdet3d/lib/python3.10/site-packages (from matplotlib->mmdet<3.3.0,>=3.0.0) (2.9.0.post0)
Downloading mmdet-3.2.0-py3-none-any.whl (2.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 2.6 MB/s  0:00:00
Downloading pycocotools-2.0.10-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (455 kB)
Downloading scipy-1.15.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (37.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 37.7/37.7 MB 57.0 MB/s  0:00:00
Downloading shapely-2.1.2-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (3.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.1/3.1 MB 51.5 MB/s  0:00:00
Downloading terminaltables-3.1.10-py2.py3-none-any.whl (15 kB)
Installing collected packages: terminaltables, shapely, scipy, pycocotools, mmdet
Successfully installed mmdet-3.2.0 pycocotools-2.0.10 scipy-1.15.3 shapely-2.1.2 terminaltables-3.1.10
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ git clone https://github.com/open-mmlab/mmdetection3d.git
Cloning into 'mmdetection3d'...
remote: Enumerating objects: 21045, done.
remote: Total 21045 (delta 0), reused 0 (delta 0), pack-reused 21045 (from 1)
Receiving objects: 100% (21045/21045), 20.53 MiB | 34.58 MiB/s, done.
Resolving deltas: 100% (14852/14852), done.
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ python -c "import torch, mmcv, mmdet, mmdet3d; \
print(f'Torch {torch.__version__}, MMCV {mmcv.__version__}, MMDet {mmdet.__version__}, MMDet3D {mmdet3d.__version__}')"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/mmcv/__init__.py", line 4, in <module>
    from .image import *
  File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/mmcv/image/__init__.py", line 2, in <module>
    from .colorspace import (bgr2gray, bgr2hls, bgr2hsv, bgr2rgb, bgr2ycbcr,
  File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/mmcv/image/colorspace.py", line 4, in <module>
    import cv2
  File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/cv2/__init__.py", line 181, in <module>
    bootstrap()
  File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/cv2/__init__.py", line 153, in bootstrap
    native_module = importlib.import_module("cv2")
  File "/usr/lib/python3.10/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
ImportError: libGL.so.1: cannot open shared object file: No such file or directory
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ cd mmdetection3d
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d/mmdetection3d (main) $ pip install -v -e .
Using pip 25.3 from /workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip (python 3.10)
Obtaining file:///workspaces/mmdet3d/mmdetection3d
  Running command installing build dependencies
  Using pip 25.3 from /workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip (python 3.10)
  Collecting setuptools>=40.8.0
    Obtaining dependency information for setuptools>=40.8.0 from https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
    Using cached setuptools-80.9.0-py3-none-any.whl.metadata (6.6 kB)
  Using cached setuptools-80.9.0-py3-none-any.whl (1.2 MB)
  Installing collected packages: setuptools
  ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
  openxlab 0.1.3 requires setuptools~=60.2.0, but you have setuptools 80.9.0 which is incompatible.
  Successfully installed setuptools-80.9.0
  Installing build dependencies ... done
  Running command Checking if build backend supports build_editable
  Checking if build backend supports build_editable ... done
  Running command Getting requirements to build editable
  Traceback (most recent call last):
    File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 389, in <module>
      main()
    File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 373, in main
      json_out["return_val"] = hook(**hook_input["kwargs"])
    File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 157, in get_requires_for_build_editable
      return hook(config_settings)
    File "/tmp/pip-build-env-i4i3dql6/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 473, in get_requires_for_build_editable
      return self.get_requires_for_build_wheel(config_settings)
    File "/tmp/pip-build-env-i4i3dql6/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 331, in get_requires_for_build_wheel
      return self._get_build_requires(config_settings, requirements=[])
    File "/tmp/pip-build-env-i4i3dql6/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 301, in _get_build_requires
      self.run_setup()
    File "/tmp/pip-build-env-i4i3dql6/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 512, in run_setup
      super().run_setup(setup_script=setup_script)
    File "/tmp/pip-build-env-i4i3dql6/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 317, in run_setup
      exec(code, locals())
    File "<string>", line 10, in <module>
  ModuleNotFoundError: No module named 'torch'
  error: subprocess-exited-with-error
  
  × Getting requirements to build editable did not run successfully.
  │ exit code: 1
  ╰─> No available output.
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
  full command: /workspaces/mmdet3d/.mmdet3d/bin/python3.10 /workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py get_requires_for_build_editable /tmp/tmpo60eyw55
  cwd: /workspaces/mmdet3d/mmdetection3d
  Getting requirements to build editable ... error
ERROR: Failed to build 'file:///workspaces/mmdet3d/mmdetection3d' when getting requirements to build editable
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d/mmdetection3d (main) $ cd ..
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip install 'setuptools~=60.2.0'
Requirement already satisfied: setuptools~=60.2.0 in ./.mmdet3d/lib/python3.10/site-packages (60.2.0)
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ pip list
Package                Version
---------------------- ------------
addict                 2.4.0
aliyun-python-sdk-core 2.16.0
aliyun-python-sdk-kms  2.16.5
certifi                2022.12.7
cffi                   2.0.0
charset-normalizer     2.1.1
click                  8.3.1
cmake                  3.25.0
colorama               0.4.6
contourpy              1.3.2
crcmod                 1.7
cryptography           46.0.3
cycler                 0.12.1
filelock               3.14.0
fonttools              4.60.1
idna                   3.4
Jinja2                 3.1.6
jmespath               0.10.0
kiwisolver             1.4.9
lit                    15.0.7
Markdown               3.10
markdown-it-py         4.0.0
MarkupSafe             2.1.5
matplotlib             3.10.7
mdurl                  0.1.2
mmcv                   2.1.0
mmdet                  3.2.0
mmengine               0.10.7
model-index            0.1.11
mpmath                 1.3.0
networkx               3.3
numpy                  1.26.0
opencv-python          4.12.0.88
opendatalab            0.0.10
openmim                0.3.9
openxlab               0.1.3
ordered-set            4.1.0
oss2                   2.17.0
packaging              24.2
pandas                 2.3.3
pillow                 11.3.0
pip                    25.3
platformdirs           4.5.0
pycocotools            2.0.10
pycparser              2.23
pycryptodome           3.23.0
Pygments               2.19.2
pyparsing              3.2.5
python-dateutil        2.9.0.post0
pytz                   2023.4
PyYAML                 6.0.3
requests               2.28.2
rich                   13.4.2
scipy                  1.15.3
setuptools             60.2.0
shapely                2.1.2
six                    1.17.0
sympy                  1.14.0
tabulate               0.9.0
termcolor              3.2.0
terminaltables         3.1.10
tomli                  2.3.0
torch                  2.0.1+cu118
torchvision            0.15.2+cu118
tqdm                   4.65.2
triton                 2.0.0
typing_extensions      4.15.0
tzdata                 2025.2
urllib3                1.26.13
yapf                   0.43.0
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d (main) $ cd mmdetection3d
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d/mmdetection3d (main) $ pip install -v -e .
Using pip 25.3 from /workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip (python 3.10)
Obtaining file:///workspaces/mmdet3d/mmdetection3d
  Running command installing build dependencies
  Using pip 25.3 from /workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip (python 3.10)
  Collecting setuptools>=40.8.0
    Obtaining dependency information for setuptools>=40.8.0 from https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
    Using cached setuptools-80.9.0-py3-none-any.whl.metadata (6.6 kB)
  Using cached setuptools-80.9.0-py3-none-any.whl (1.2 MB)
  Installing collected packages: setuptools
  ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
  openxlab 0.1.3 requires setuptools~=60.2.0, but you have setuptools 80.9.0 which is incompatible.
  Successfully installed setuptools-80.9.0
  Installing build dependencies ... done
  Running command Checking if build backend supports build_editable
  Checking if build backend supports build_editable ... done
  Running command Getting requirements to build editable
  Traceback (most recent call last):
    File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 389, in <module>
      main()
    File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 373, in main
      json_out["return_val"] = hook(**hook_input["kwargs"])
    File "/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 157, in get_requires_for_build_editable
      return hook(config_settings)
    File "/tmp/pip-build-env-dmeajzly/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 473, in get_requires_for_build_editable
      return self.get_requires_for_build_wheel(config_settings)
    File "/tmp/pip-build-env-dmeajzly/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 331, in get_requires_for_build_wheel
      return self._get_build_requires(config_settings, requirements=[])
    File "/tmp/pip-build-env-dmeajzly/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 301, in _get_build_requires
      self.run_setup()
    File "/tmp/pip-build-env-dmeajzly/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 512, in run_setup
      super().run_setup(setup_script=setup_script)
    File "/tmp/pip-build-env-dmeajzly/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 317, in run_setup
      exec(code, locals())
    File "<string>", line 10, in <module>
  ModuleNotFoundError: No module named 'torch'
  error: subprocess-exited-with-error
  
  × Getting requirements to build editable did not run successfully.
  │ exit code: 1
  ╰─> No available output.
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
  full command: /workspaces/mmdet3d/.mmdet3d/bin/python3.10 /workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py get_requires_for_build_editable /tmp/tmpjmw48x0r
  cwd: /workspaces/mmdet3d/mmdetection3d
  Getting requirements to build editable ... error
ERROR: Failed to build 'file:///workspaces/mmdet3d/mmdetection3d' when getting requirements to build editable

## получен инференс без демонстрации

python demo/pcd_demo.py \
  demo/data/kitti/000008.bin \
  configs/pointpillars/pointpillars_hv_secfpn_8xb6-160e_kitti-3d-car.py \
  checkpoints/hv_pointpillars_secfpn_6x8_160e_kitti-3d-car_20220331_134606-d42d15ed.pth \
  --device cpu \
  --out-dir outputs
/workspaces/mmdet3d/mmdetection3d/mmdet3d/models/dense_heads/anchor3d_head.py:94: UserWarning: dir_offset and dir_limit_offset will be depressed and be incorporated into box coder in the future
  warnings.warn(
Loads checkpoint by local backend from path: checkpoints/hv_pointpillars_secfpn_6x8_160e_kitti-3d-car_20220331_134606-d42d15ed.pth
11/16 16:51:54 - mmengine - WARNING - Failed to search registry with scope "mmdet3d" in the "function" registry tree. As a workaround, the current "function" registry in "mmengine" is used to build instance. This may cause unexpected failure when running the built modules. Please check whether "mmdet3d" is a correct scope, or whether the registry is initialized.
/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/mmengine/visualization/visualizer.py:196: UserWarning: Failed to add <class 'mmengine.visualization.vis_backend.LocalVisBackend'>, please provide the `save_dir` argument.
  warnings.warn(f'Failed to add {vis_backend.__class__}, '
/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/torch/functional.py:504: UserWarning: 
torch.meshgrid: in an upcoming release, it will be required to pass the indexing argument. (Triggered
internally at ../aten/src/ATen/native/TensorShape.cpp:3483.)
  return _VF.meshgrid(tensors, **kwargs)  # type: ignore[attr-defined]
Inference ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   
11/16 16:51:56 - mmengine - INFO - results have been saved at outputs
(.mmdet3d) @MazurenkovK ➜ /workspaces/mmdet3d/mmdetection3d (main) $ cd /workspaces/mmdet3d/mmdetection3d
source /workspaces/mmdet3d/.mmdet3d/bin/activate
python demo_run.py \
  demo/data/kitti/000008.bin \
  configs/pointpillars/pointpillars_hv_secfpn_8xb6-160e_kitti-3d-car.py \
  checkpoints/hv_pointpillars_secfpn_6x8_160e_kitti-3d-car_20220331_134606-d42d15ed.pth \
  --device cpu \
  --out-dir outputs \
  --show
Traceback (most recent call last):
  File "/workspaces/mmdet3d/mmdetection3d/demo_run.py", line 15, in <module>
    from mmdet3d.structures import points_to_tensor
ImportError: cannot import name 'points_to_tensor' from 'mmdet3d.structures' (/workspaces/mmdet3d/mmdetection3d/mmdet3d/structures/__init__.py)


## выводит инференс, но без визуализации

python3 demo/pcd_demo.py \
  demo/data/kitti/000008.bin \
  configs/pointpillars/pointpillars_hv_secfpn_8xb6-160e_kitti-3d-car.py \
  checkpoints/hv_pointpillars_secfpn_6x8_160e_kitti-3d-car_20220331_134606-d42d15ed.pth \
  --device cpu \
  --show \
  --out-dir outputs
11/16 17:01:22 - mmengine - WARNING - Display device not found. `--show` is forced to False
/workspaces/mmdet3d/mmdetection3d/mmdet3d/models/dense_heads/anchor3d_head.py:94: UserWarning: dir_offset and dir_limit_offset will be depressed and be incorporated into box coder in the future
  warnings.warn(
Loads checkpoint by local backend from path: checkpoints/hv_pointpillars_secfpn_6x8_160e_kitti-3d-car_20220331_134606-d42d15ed.pth
11/16 17:01:22 - mmengine - WARNING - Failed to search registry with scope "mmdet3d" in the "function" registry tree. As a workaround, the current "function" registry in "mmengine" is used to build instance. This may cause unexpected failure when running the built modules. Please check whether "mmdet3d" is a correct scope, or whether the registry is initialized.
/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/mmengine/visualization/visualizer.py:196: UserWarning: Failed to add <class 'mmengine.visualization.vis_backend.LocalVisBackend'>, please provide the `save_dir` argument.
  warnings.warn(f'Failed to add {vis_backend.__class__}, '
/workspaces/mmdet3d/.mmdet3d/lib/python3.10/site-packages/torch/functional.py:504: UserWarning: 
torch.meshgrid: in an upcoming release, it will be required to pass the indexing argument. (Triggered
internally at ../aten/src/ATen/native/TensorShape.cpp:3483.)
  return _VF.meshgrid(tensors, **kwargs)  # type: ignore[attr-defined]
Inference ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   
11/16 17:01:23 - mmengine - INFO - results have been saved at outputs
