
# YOLOv8 Heatmap 可视化工具

该项目基于 YOLOv8 与 Grad-CAM 系列方法（如 GradCAM++、EigenCAM 等），实现目标检测模型的可视化热力图生成，用于辅助模型解释与分析。

## 📦 项目功能简介

- 支持多种 Grad-CAM 方法（GradCAMPlusPlus、GradCAM、EigenCAM、HiResCAM 等）
- 支持框内重归一化热力图（可选）
- 支持绘制类别与置信度框
- 动态支持自定义模型结构中任意一层作为可视化目标
- 支持文件夹批量处理

---

## 📁 文件结构

```bash
.
├── main.py              # 主运行代码
├── README.md            # 本说明文档
├── weight/              # 存放模型权重（.pt）
├── savedir/             # 热力图保存目录
└── ...
```

---

## 🛠️ 安装依赖

请使用 Python >= 3.8 环境，安装以下依赖：

```bash
pip install torch torchvision
pip install opencv-python matplotlib pillow tqdm
pip install ultralytics
pip install grad-cam==1.4.8
```

---

## 🚀 使用说明

### 1. 权重准备

将训练好的 YOLOv8 权重（`.pt` 文件）放入 `weight/` 目录下，并修改参数中的路径（`get_params` 函数中）：

```python
'weight': 'weight/yolov8_custom.pt',
```

### 2. 配置参数

在 `get_params()` 函数中可自定义以下参数：

| 参数名 | 含义 |
|--------|------|
| `weight` | 权重文件路径 |
| `layer` | 可视化目标层路径（字符串格式） |
| `device` | 运行设备，如 `cuda:0` 或 `cpu` |
| `method` | 可视化方法（GradCAMPlusPlus、EigenCAM 等） |
| `backward_type` | 反向传播目标：class、box、all |
| `conf_threshold` | 检测框置信度阈值 |
| `ratio` | 用于控制用于生成热图的前几项目标占比 |
| `show_box` | 是否显示检测框 |
| `renormalize` | 是否仅在框中显示热力图 |

### 3. 运行脚本

修改主函数调用：

```python
model('path/to/image.jpg', 'savedir', 'output.jpg')
# 或批量处理文件夹：
model('path/to/images', 'savedir', '')
```

---

## 🧠 可视化方法支持

目前支持如下 CAM 方法（来自 `pytorch-grad-cam` 库）：

- `GradCAMPlusPlus`
- `GradCAM`
- `XGradCAM`
- `EigenCAM`
- `HiResCAM`
- `LayerCAM`
- `RandomCAM`
- `EigenGradCAM`

通过修改 `get_params()` 中的 `'method'` 字段选择。

---

## 🔍 示例图像说明

- **热力图颜色** 表示模型关注区域强度。
- **检测框** 显示目标类别与置信度（可选开启）。
- **框内归一化热图** 可选，仅保留目标区域热力。

---

## 🧩 模型结构中的层路径获取方法

使用 `get_layer_by_path(model, 'model.model[9].token_mixer.pool_h')` 可获取任意模块作为 CAM 目标层。

---

## ✨ 注意事项

- 默认模型使用 ultralytics YOLOv8。
- 若使用自定义模型，请确保与 ultralytics 构建方式兼容。
- 若使用 End-to-End 模型，请确保 `.end2end = True`。

---
