# NeRF 基于 NeRF 的物体重建和新视图合成

## 环境安装与模型下载

### 1. 环境安装

```sh
conda create --name nerf -y python=3.8
conda activate nerf
```
### 2. 下载 NeRF 模型代码并配置环境

```sh
cd ~/nerf-pytorch
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 3. 数据转换代码及环境

```sh
conda create --name llff -y python=3.8
conda activate llff
cd ~/LLFF
pip install -r requirements.txt
```

## 代码组织结构

```bash
├── LLFF     # 将相机参数转化为 LLFF 格式
├── nerf     # NeRF 模型重建
         ├── config     # 训练参数的设置文件
         ├── data        
                  ├── nerf_llff_data
                           ├── llfftest5  # 用于重建的图像及相机参数
```

## 实验步骤


### 1. 相机参数估计与稀疏重建

使用 COLMAP 软件对`llfftest5/images`中的图像进行参数重建，得到的结果文件放入`llfftest5/sparse/0`文件夹。

### 2. 数据格式转化

使用LLFF中的`img2pose.py`文件将参数转化为LLFF格式。

```sh
python img2pose.py --images_path llfftest5/images --sparse_path llfftest5/sparse/0 --output_path llfftest5/poses
```

### 3. NeRF 模型训练

在`nerf/configs`文件夹中新建`new_data.txt`文件，输入训练参数、图像地址等信息。

运行以下命令进行训练：

```sh
python run_nerf.py --config configs/new_data.txt
```

### 4. 使用训练好的模型进行参数重建

运行以下命令进行渲染：

```sh
python run_nerf.py --config configs/new_data.txt --render_only
```
