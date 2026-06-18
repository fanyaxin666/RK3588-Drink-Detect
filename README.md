# RK3588-Drink-Detect
基于RK3588的饮料目标检测系统边缘端部署
### 一、项目简介
本项目为Linux边缘离线AI检测方案，独立完成从模型训练到硬件端侧推理的全流程开发。基于YOLOv5训练饮料目标检测模型，通过RKNN-Toolkit2完成模型INT8量化，解决量化精度丢失、RK3588 NPU调用崩溃等底层部署问题；在RK3588开发板实现摄像头视频流实时离线推理，稳定输出30~35FPS检测画面。
项目框架模块化设计，可快速迁移至工业设备监控、面板指示灯故障识别等边缘AI场景。
项目周期：2026.04 - 2026.05
### 二、技术栈
硬件:瑞芯微 RK3588 开发板（6TOPS NPU）、USB摄像头
软件&框架
1. 模型训练：Python、PyTorch、YOLOv5、OpenCV-Python
2. 模型转换：RKNN-Toolkit2
3. 端侧推理：C++、OpenCV、ARM64 Linux
4. 系统环境：Ubuntu22.04
### 三、项目目录结构
RK3588-Drink-Detect/
├── train_model/        # 模型训练代码、饮料数据集、pt权重文件
├── rknn_convert/       # RKNN量化转换脚本、精度调试代码
├── edge_infer/         # RK3588板端实时推理C++源码
├── docs/               # 部署踩坑记录、推理效果图
├── .gitignore          # 过滤训练缓存、临时权重文件
└── README.md           # 项目说明文档
## 四、环境部署
1. PC训练环境安装
pip install torch torchvision opencv-python yolov5
sudo apt update
sudo apt install libopencv-dev gcc g++ cmake
五、完整运行流程
1. 数据集标注，使用YOLOv5完成饮料目标检测模型训练，导出 .pt 权重；
2. 调用RKNN-Toolkit2脚本完成模型INT8量化，做精度补偿，生成适配RK3588 NPU的 .rknn 模型；
3. 使用cmake编译端侧C++实时推理程序；
4. 开发板接入USB摄像头，执行推理程序，本地离线实时识别画面内饮料目标。
### 六、项目核心亮点
1. 优化RKNN量化预处理逻辑，缓解INT8量化带来的检测精度损失；
2. 重写NPU资源释放逻辑，解决多帧并发推理时调用崩溃、内存泄漏底层问题；
3. 单RK3588硬件实现30~35FPS稳定离线视频推理，无需云端网络支持；
4. 模块化推理框架，解耦图像预处理、NPU推理、后处理绘图模块，可快速拓展其他检测场景。
### 七、效果指标
离线推理帧率：30~35FPS
支持无网络环境持续检测
适配工业边缘低功耗嵌入式设备
