# SadTalker

基于此开源模型（ai数字人）做cuda到sdaa侧的迁移，并支持此模型在sdaa设备上运行
## 1. 模型概述
- 仓库链接：[[OpenTalker/SadTalker: [CVPR 2023\] SadTalker：Learning Realistic 3D Motion Coefficients for Stylized Audio-Driven Single Image Talking Face Animation](https://github.com/OpenTalker/SadTalker)](https://github.com/state-spaces/mamba)
- 论文链接：[[[2211.12194\] SadTalker: Learning Realistic 3D Motion Coefficients for Stylized Audio-Driven Single Image Talking Face Animation](https://arxiv.org/abs/2211.12194)](https://openreview.net/forum?id=AL1fq05o7H)
- 详细信息参考**README_en.md**

## 2. 快速开始
使用本模型执行训练的主要流程如下：
### 2.1 基础环境安装

```
--------------+------------------------------------------------
 Host IP      | 20.21.22.13
 PyTorch      | 2.4.0a0+git4451b0e
 Torch-SDAA   | 2.2.0
--------------+------------------------------------------------
 SDAA Driver  | 2.2.0 (N/A)
 SDAA Runtime | 2.2.0 (/opt/tecoai/lib64/libsdaart.so)
 SDPTI        | 1.5.0 (/opt/tecoai/lib64/libsdpti.so)
 TecoDNN      | 2.2.0 (/opt/tecoai/lib64/libtecodnn.so)
 TecoBLAS     | 2.2.0 (/opt/tecoai/lib64/libtecoblas.so)
 CustomDNN    | 1.20.0a0 (/opt/tecoai/lib64/libtecodnn_ext.so)
 TecoRAND     | 2.0.1 (/opt/tecoai/lib64/libtecorand.so)
 TCCL         | 2.2.0 (/opt/tecoai/lib64/libtccl.so)
--------------+------------------------------------------------
```

1.  启动相应虚拟环境并确定torch，torch_sdaa可用。
    ```
    conda env list
    conda activate torch_env
    ```

2.  下载源码。
    ```
    git clone https://github.com/OpenTalker/SadTalker.git
    cd SadTalker
    ```

3.  安装三方依赖库。

   ```shell
   conda install ffmpeg
   
   # pip install -r requirements.txt (建议依次手动安装/SadTalker/requirements.txt中依赖库)
   pip install numpy==1.23.4
   pip install face_alignment==1.3.5
   pip install imageio==2.19.3
   ...
   ...
   pip install av
   pip install safetensors
   
   # 安装结束后确认numpy版本是否被覆盖，2.X.X则重新安装
   ```

4. 下载预训练模型

    ```
    bash scripts/download_models.sh
    ```

### 2.2 命令行启动

1. 开启环境迁移变量切换sdaa backend。

```
source /opt/tecoai/setvars.sh
```

2. 修改源码

```
# SadTalker/ 路径下

# 备份复制源文件
cp src/facerender/modules/dense_motion.py src/facerender/modules/dense_motion.py.bak

# 替换 .view( 为 .reshape(
sed -i 's/\.view(/.reshape(/g' src/facerender/modules/dense_motion.py
```

3. 开始推理

```
# 准备wav音频片段以及图片（视频）
python inference.py --driven_audio <audio.wav> \
                    --source_image <video.mp4 or picture.png> \
```


## 3.windows使用

见README_en.md


