<h1 align="center">🦾 LeLab</h1>

<p align="center">
  <b>LeRobot 的图形化界面 — 标定、遥操作、采集、训练、推理一站式 Web 应用。</b>
</p>

<div align="center">

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/OneRobotAI/lelab/blob/main/LICENSE)

</div>

**LeLab** 将完整的 LeRobot 工作流 — 标定、遥操作、数据采集、训练、回放 — 集成到一个浏览器界面。插上机械臂，打开应用，即刻开始。无需 CLI 操作，无需键盘提示。

专为 SO-101 leader/follower 机械臂设计，让新手能在几分钟内从"开箱"到"训练第一个策略"。

## 功能一览

<div align="center">
  <table>
    <tr>
      <td>🎯 <b>标定 (Calibrate)</b></td>
      <td>双机械臂图形化标定流程 — 无需键盘操作</td>
    </tr>
    <tr>
      <td>🕹️ <b>遥操作 (Teleoperate)</b></td>
      <td>操作 leader，follower 实时跟随。关节数据实时传输</td>
    </tr>
    <tr>
      <td>📹 <b>数据采集 (Record)</b></td>
      <td>录制 episodes 到 LeRobotDataset，支持摄像头</td>
    </tr>
    <tr>
      <td>🧠 <b>训练 (Train)</b></td>
      <td>启动 LeRobot 训练任务，实时查看日志</td>
    </tr>
    <tr>
      <td>🤖 <b>推理 (Inference)</b></td>
      <td>在 follower 上运行训练好的策略</td>
    </tr>
    <tr>
      <td>⏪ <b>回放 (Replay)</b></td>
      <td>回放任意已录制的 episode</td>
    </tr>
    <tr>
      <td>☁️ <b>上传 (Upload)</b></td>
      <td>一键将数据集推送到 <a href="https://huggingface.co/">Hugging Face Hub</a></td>
    </tr>
  </table>
</div>

---

## 安装指南

### 前置要求

- **Python 3.12+**
- **Git**
- **SO-101 机械臂的 USB 串口驱动**（Windows 需安装 CH340/CP2102 驱动）

### 通用方式（uv 推荐，三平台通用）

```bash
# 安装 uv（如果还没有）
pip install uv

# 安装 LeLab
uv tool install git+https://github.com/OneRobotAI/lelab.git

# 启动（自动打开浏览器）
lelab
```

---

## Windows 安装

### pip 方式

```powershell
# 克隆仓库
git clone https://github.com/OneRobotAI/lelab.git
cd lelab

# 创建虚拟环境
python -m venv venv
venv\Scripts\activate

# 安装
pip install -e .

# 启动
lelab
```

### Windows 常见问题

**摄像头无法使用**
- LeLab 在 Windows 上使用 DirectShow 后端
- 确保无其他应用（如 Zoom、Teams）占用摄像头

**串口连接失败**
- 打开设备管理器，查看 COM 端口号
- 在 LeLab 前端手动选择正确的端口（默认 COM3，实际可能不同）

**训练时视频解码报错**
- LeLab 已内置自动修复：当 torchcodec 无法加载 FFmpeg DLL 时，自动回退到 PyAV
- 若仍有问题，可在训练时选择 `pyav` 作为视频后端

---

## macOS 安装

### pip 方式

```bash
# 克隆仓库
git clone https://github.com/OneRobotAI/lelab.git
cd lelab

# 创建虚拟环境
python3 -m venv venv
source venv/bin/activate

# 安装
pip install -e .

# 启动
lelab
```

### macOS 常见问题

**摄像头权限**
- 首次运行系统会请求摄像头权限，允许 Terminal 访问

**串口权限**
- 若遇 `Permission denied`：
  ```bash
  sudo chmod 666 /dev/tty.usbmodem*
  ```

**Apple Silicon (M1/M2/M3)**
- 使用 ARM64 版 Python，PyTorch 已原生支持

---

## Ubuntu 安装

### pip 方式

```bash
# 克隆仓库
git clone https://github.com/OneRobotAI/lelab.git
cd lelab

# 创建虚拟环境
python3 -m venv venv
source venv/bin/activate

# 安装
pip install -e .

# 启动
lelab
```

### Ubuntu 常见问题

**串口权限**
```bash
sudo usermod -a -G dialout $USER
# 注销并重新登录生效
```

**摄像头权限**
```bash
sudo usermod -a -G video $USER
```

**缺少 FFmpeg**
```bash
sudo apt update
sudo apt install ffmpeg
```

**NVIDIA GPU 支持**
- 安装 NVIDIA 驱动和 CUDA，验证：`nvidia-smi`
- 如需 CUDA 版 PyTorch，参考 [PyTorch 官网](https://pytorch.org/get-started/locally/)

---

## 开发模式

```bash
git clone https://github.com/OneRobotAI/lelab.git
cd lelab

# 创建虚拟环境
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 安装开发依赖
pip install -e ".[dev,test]"

# 启动开发服务器（热重载）
lelab --dev
```

开发模式下：
- Vite 前端运行于 `:8080`（热重载）
- uvicorn 后端运行于 `:8000`（热重载）
- 浏览器自动打开 `http://localhost:8080`

---

## 系统架构

```
lelab/
├── server.py          # FastAPI 主服务器
├── train.py           # 训练模块（含视频后端自动检测）
├── teleoperate.py     # 遥操作模块
├── record.py          # 数据采集模块
├── calibrate.py       # 标定模块
├── rollout.py         # 推理模块
├── jobs.py            # 任务管理
├── datasets.py        # 数据集操作
└── utils/
    └── config.py      # 配置管理
```

## 相关资源

- **[LeRobot](https://github.com/huggingface/lerobot):** 底层机器人框架
- **[LeLab HF Space](https://huggingface.co/spaces/lerobot/LeLab):** 官方在线体验
- **[Discord](https://discord.gg/q8Dzzpym3f):** LeRobot 社区

## 许可证

Apache License 2.0
