# 基于 CycleGAN 的无监督图像风格迁移（Photo → Monet）

## 1. 项目简介

本项目基于 CycleGAN 模型，实现了无监督条件下的图像风格迁移任务，
以 **Photo → Monet** 为例，将真实照片转换为具有莫奈绘画风格的图像。

CycleGAN 通过引入循环一致性约束，在无需成对训练数据的情况下，
学习两个不同图像域之间的映射关系，广泛应用于图像风格迁移、
图像域转换等生成式任务中。

本项目采用 **官方公开的 CycleGAN PyTorch 实现及预训练模型**，
重点关注模型推理阶段的应用与结果分析。

---

## 2. 项目环境

- 操作系统：Windows 10 / 11  
- 开发语言：Python 3.10  
- 深度学习框架：PyTorch  
- GPU：NVIDIA GeForce RTX 4060 Laptop GPU  
- 开发工具：PyCharm  
- 虚拟环境：conda（cyclegan）

---

## 3. 项目结构说明

```

DLBigWork/
├── inputs/
│   └── photos/                 # 输入的真实照片（Photo 域）
│
├── outputs/
│   └── results_style_monet/    # 最终生成结果（用于报告展示）
│
├── third_party/
│   └── pytorch-CycleGAN-and-pix2pix/
│       ├── test.py
│       ├── train.py
│       ├── models/
│       ├── scripts/
│       ├── checkpoints/
│       │   └── style_monet_pretrained/
│       │       └── latest_net_G.pth
│       └── results/
│
└── README.md

```

---

## 4. 使用的模型与方法

### 4.1 CycleGAN 模型简介

CycleGAN 由两个生成器和两个判别器组成：

- 生成器 G：实现 Photo → Monet 的映射
- 生成器 F：实现 Monet → Photo 的反向映射
- 判别器：用于对抗训练阶段判断生成图像的真实性

在模型训练完成后，**推理阶段仅需加载目标方向的生成器 G**，
即可完成图像风格迁移任务。判别器与反向生成器主要用于训练阶段，
在推理过程中不再参与计算。

---

### 4.2 本项目的实验设置

- 使用官方公开的 **style_monet** 预训练模型  
- 仅进行推理（Inference），不进行模型训练  
- 输入为自采集的真实照片（Photo 域）  
- 输出为 Monet 绘画风格图像  

---

## 5. 使用方法（推理）

### 5.1 准备输入图片

将需要进行风格迁移的真实照片（jpg/png 格式）
放入以下目录：

```

inputs/photos/

````

---

### 5.2 运行推理命令

在 `third_party/pytorch-CycleGAN-and-pix2pix/` 目录下执行：

```
python test.py --dataroot ..\..\inputs\photos --name style_monet_pretrained --model test --no_dropout
```

程序将自动加载预训练生成器模型，并对输入图片进行风格迁移。

---

### 5.3 查看输出结果

生成结果保存在：

```
third_party/pytorch-CycleGAN-and-pix2pix/results/style_monet_pretrained/test_latest/images/
```

最终用于展示与报告的图片已整理至：

```
outputs/results_style_monet/
```

---

## 6. 实验结果说明

实验结果表明，CycleGAN 能够在保持图像整体结构的前提下，
成功迁移莫奈绘画中典型的色彩分布与笔触风格。

在部分复杂场景中，生成结果仍存在模糊或细节不稳定的问题，
