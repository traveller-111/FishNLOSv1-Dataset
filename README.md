# FishNLOS-v1: 城市峡谷GNSS非视距信号识别基准数据集
A real-world fisheye dataset for GNSS NLOS signal detection and semantic segmentation in urban canyons

> **官方数据集仓库**：论文《面向边缘端部署的语义分割模型HF-PSPNet：在复杂城市 GNSS NLOS 识别中的应用》所使用的核心数据集。
> 本项目开源了 **FishNLOS-v1** —— 一个面向真实复杂城市峡谷场景的高分辨率鱼眼图像数据集。

---

## 1.简介 (Introduction)

在自动驾驶、移动测绘及无人系统等现代应用中，高频且连续的高精度定位是保障系统安全与鲁棒运行的基石。然而，在城市峡谷这一典型的异构复杂环境中，全球导航卫星系统(GNSS)的可靠性面临严峻挑战。特别是在高楼林立的密集街区与植被交错的绿化区域，多径效应(Multipath)与非视距(NLOS)传播成为制约定位精度的主要杀手。一方面，高层建筑构成的天然屏障不仅阻断低仰角卫星的直射信号，更引发严重的信号反射与折射，导致接收机误锁多径信号，造成数米至数十米的测距误差；另一方面，城市植被对GNSS信号表现出显著的吸收与散射特性，且树冠随风摆动引发的动态遮挡进一步导致载波相位延迟与观测值抖动。更关键的是，在高频GNSS定位场景下(如10Hz-50Hz)，载体的高速运动使得卫星的可视性状态在毫秒级的时间尺度上发生剧烈变化。若无法实时、精准地剔除受污染的观测值，错误的卫星数据将迅速污染卡尔曼滤波器的状态估计，导致定位轨迹发散。因此，如何在保证高频输出的同时，实现对NLOS信号的实时、精准识别，已成为当前学术界与工业界亟待解决的核心难题。
为解决上述问题，利用视觉传感器辅助GNSS定位已成为主流技术路线。该路线的核心在于利用全向鱼眼相机捕获环境的空间几何信息，通过图像语义分割技术将天空视图精确划分为“天空(LOS)”与“障碍物(NLOS)”区域，从而从物理几何层面构建卫星选择的“硬约束”。相较于仅依赖载噪比(C/N0)等统计指标的传统信号处理方法，基于深度学习的语义分割方案具备更强的环境鲁棒性与可解释性。
然而，社区目前缺少完整的鱼眼相机拍摄图与相对应的卫星星历数据供深度学习使用。且所拍摄的图逐帧拆分后需要精细标注以实现背景和天空的分割。
因此本项目开源一套FishNLOS-v1数据集，内含1194张原始图片与其较高质量标注结果，可被直接用于训练二分类语义分割模型。
若在使用数据集过程中遇到任何问题，且问题列表中无满意解决方案，请提交新议题，我们将尽快回复


---

**2.数据采集流程** 该数据集采集于**北京市海淀区**典型的城市峡谷环境，涵盖了深层建筑峡谷、茂密植被遮挡及混合干扰等多种极具挑战性的真实场景。所有图像均经过精细的像素级标注，旨在为 GNSS 信号质量评估与多源融合定位提供可靠的数据支撑。
·语义分割原始图片的采集均使用180° 超广角鱼眼相机，通过绕特定轨迹进行行走，将一片区域划分为500-600张鱼眼相机图片
·卫星Azi Ele 等数据采用ubloxGNSS芯片
·姿态角Yaw Pitch Roll 等数据采用IMU芯片
数据采集时的轨迹和实拍如下

<img width="693" height="506" alt="Snipaste_2026-01-19_16-50-43" src="https://github.com/user-attachments/assets/2c487034-2771-4669-9822-082359013f47" />
---

## 3.数据集介绍 (Dataset Features)

* **真实物理世界采集**: 非合成数据，真实反映了城市环境中的光照变化（逆光/强光/阴影）与复杂几何结构。
* **超广角全天域视野**: 采用 180° 鱼眼相机采集，能够捕捉完整的地平线以上空间信息，这是判定卫星可见性的前提。
* **覆盖了四类最具挑战性的复杂场景**:
    *  **深层城市峡谷**: 密集高层建筑群，SVF极低。
    <p align="center">
       <img src="https://github.com/user-attachments/assets/7d01f1f3-77ca-44ff-b62b-98b573d092a2" width="32%" />
       <img src="https://github.com/user-attachments/assets/822b45ea-c963-4951-9402-c3d4662b572c" width="32%" />
       <img src="https://github.com/user-attachments/assets/9ec6e792-3102-4932-97fc-8fc760ffd5e7" width="32%" />
    </p>
     
    *  **高动态植被**: 道路两侧茂密行道树，包含大量非结构化遮挡。
      <p align="center">
        <img src="https://github.com/user-attachments/assets/c9af047e-43d5-423a-a0de-de9e55de7b0b" width="32%" />
        <img src="https://github.com/user-attachments/assets/bd8ac8f7-c04d-4bc1-ba66-119d8df0b7ca" width="32%" />
        <img src="https://github.com/user-attachments/assets/1cc1f780-3c5b-48ad-ab72-962cc4d49212" width="32%" />
      </p>
    *  **混合光照环境**: 包含正午强光及玻璃幕墙反光场景。
      <p align="center">
        <img src="https://github.com/user-attachments/assets/8141e8b4-54d9-43fa-abb0-47821857a395" width="32%" />
        <img src="https://github.com/user-attachments/assets/e481c120-9346-439f-8df3-35f3a2013ce3" width="32%" />
        <img src="https://github.com/user-attachments/assets/396c0542-e231-4d31-8ce2-817d96450803" width="32%" />
      </p>
* **高精度像素级标注**: 针对植被间隙、建筑边缘及细微障碍物（如电线、路灯）进行了精细化分割标注。部分标注结果展示：
      <p align="center">
        <img src="https://github.com/user-attachments/assets/83530c39-7642-4550-93d3-40ca39f46db2" width="19%" alt="280413_img_roi" />
        <img src="https://github.com/user-attachments/assets/6e40e235-5e78-49b2-9c27-d5bfd85cfc94" width="19%" alt="280563_img_roi" />
        <img src="https://github.com/user-attachments/assets/631772b6-0b1a-45dd-9104-5e17a8999204" width="19%" alt="280617_img_roi" />
        <img src="https://github.com/user-attachments/assets/d8689190-9ea9-4943-b701-6382ff0e6389" width="19%" alt="280657_img_roi" />
        <img src="https://github.com/user-attachments/assets/371761b6-9f5b-4f3d-9f35-e18f0bb83af8" width="19%" alt="282338_img_roi" />
      </p>
---

## 4.数据规格与标注说明 (Specifications)

### 1. 基本信息
| 属性 | 说明 |
| :--- | :--- |
| **图像数量** | 1194张 |
| **分辨率** | $926 \times 926$ 像素 |
| **图像格式** | `.jpg` (RGB 3通道) |
| **标签格式** | `.png` (单通道灰度图) |
| **采集地点** | 中国北京·海淀区花园路街道及周边 |

### 2. 类别定义 (Class Definitions)
本数据集主要用于区分“可视区域”与“遮挡区域”，标签定义如下：

| 类别 ID (Pixel Value) | 类别名称 (Class Name) | 描述 (Description) | 可视化颜色 (RGB) |
| :---: | :--- | :--- | :--- |
| **0** | **Background** | 包含建筑物、植被、地面等所有遮挡物 (NLOS区域) | ⚫ (0, 0, 0) |
| **1** | **Sky** | 开阔天空区域，视为卫星信号可视区 (LOS区域) | ⚪ (255, 255, 255) |

> **注意**：掩码图像为单通道 PNG，像素值仅包含 0 和 1（读取时建议归一化处理）。

---

## 5.文件结构 (Directory Structure)

数据集目录结构如下所示，图像与掩码通过文件名严格对应：

```text
data/FishNLOS-v1/
├── images/                  # 原始鱼眼图像 (RGB)
│   ├── 281700_img_roi.jpg
│   ├── 281701_img_roi.jpg
│   ├── ...
│   └── 292223_img_roi.jpg
└── masks/                   # 语义分割标签 (Gray)
    ├── 281700_img_roi.png   # 对应 281700_img_roi.jpg 的标签
    ├── 281701_img_roi.png
    ├── ...
    └── 292223_img_roi.png
```

---
## 6.结果展示
我们利用该数据集训练的语义分割模型推理结果及其可视化如下
<img width="404" height="513" alt="模型分割结果展示" src="https://github.com/user-attachments/assets/59d3108c-b302-47d6-9d32-064802bb877f" />
每组样本包含四张子图，从左至右依次为：鱼眼相机原始RGB图像；模型推理生成的语义掩码；原始图像与预测掩码的叠加视图，用于展示边界对齐情况；人工标注的真值标签

---
## 7.Related Publication


---

## 8.Lincense
本项目遵循MIT License
数据集仅可被用于学术或研究
任何商业使用请联系作者

---

本仓库目前只存放50张图片，完整数据集待论文录用后公布
