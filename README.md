# 🐧 TenVision

这是一个基于 **OpenCV** 的实验性项目，专门用于识别并计算腾讯云风格的“图形点选”验证码位置。

## ✨ 核心特性

* **OpenCV 驱动**：纯计算机视觉逻辑，不依赖大型深度学习模型，主打一个轻量。
* **形状识别**：针对特定几何特征进行特征点提取与匹配。
* **GitHub Action 闭环**：利用 GitHub 免费算力，实现“提交-识别-回推”的自动化体验。

---

## 🎮 怎么玩？(互动体验)

不需要在本地配置 Python 环境，你直接在 GitHub 就能体验这个算法：

1. **提交 Issue**：点击 [New Issue](https://github.com/AmethystDev-Labs/TenVision/issues/new)。
2. **上传图片**：Issue标题随意，内容可以是一张或多张腾讯云点选验证码
3. **自动回帖**：算法处理完成后，机器人会自动在你的 Issue 下回复一张**标记了坐标点**的图片。

[腾讯云图形点选验证码](https://cloud.tencent.com/product/captcha)


---

## 🛠️ 技术栈

* **Language:** Python 3.12
* **Library:** OpenCV-Python, Numpy
* **Automation:** GitHub Actions 

## 🏗️ 本地开发

如果你想在本地跑跑看：

```bash
# 克隆项目
git clone https://github.com/SnowBall-Bqiu/TenVision.git

# 安装依赖
pip install -r requirements.txt

# 运行 Demo
python main.py
```

建议使用虚拟环境
```bash
python -m venv venv
```
或者使用以下命令创建并激活虚拟环境，并且安装依赖
```bash
python -m venv ten-captcha&&ten-captcha\Scripts\activate&&pip install -r requirements.txt
```
pip3和python3
```bash
python3 -m venv ten-captcha&&ten-captcha/bin/activate&&pip3 install -r requirements.txt
```
## 📡 API 服务
项目提供 FastAPI 接口服务，支持本地部署调用。

### 启动服务

```bash
# 安装依赖
pip install -r requirements.txt

# 启动 API 服务
python main.py
```

服务默认运行在 `http://0.0.0.0:8000`

### API 端点

#### 1. 验证码识别

**POST** `/recognize`

识别验证码图片，返回点击坐标。

**请求头 (Headers)**

| 参数 | 必填 | 说明 |
|------|------|------|
| `X-Return-Visualization` | 否 | 是否返回识别后的图片，`true` 或 `false`，默认 `true` |
| `X-API-Token` | 是 | API 认证 Token |
| `X-API-Key` | 是 | API 认证 Key |

**请求体 (Body)**

| 参数 | 类型 | 说明 |
|------|------|------|
| `file` | File | 验证码图片文件 (PNG/JPG) |

**响应格式**

```json
{
  "success": true,
  "points": [
    {"x": 90, "y": 265},
    {"x": 342, "y": 212},
    {"x": 191, "y": 365}
  ],
  "candidates_count": 6,
  "message": "识别成功",
  "visualization": "iVBORw0KGgoAAAANSUhEUgAA..."
}
```

**字段说明**

| 字段 | 类型 | 说明 |
|------|------|------|
| `success` | boolean | 识别是否成功 |
| `points` | array | 点击坐标列表，按顺序对应顶部的3个目标符号 |
| `candidates_count` | int | 检测到的候选符号数量 |
| `message` | string | 状态信息 |
| `visualization` | string/null | Base64编码的识别结果图片（当 `X-Return-Visualization` 为 `false` 时为 null） |

### 调用示例

#### cURL

```bash
curl -X POST "http://localhost:8000/recognize" \
  -H "X-Return-Visualization: true" \
  -H "X-API-Token: your_token" \
  -H "X-API-Key: your_key" \
  -F "file=@images/0.png"
```

#### Python

```python
import requests

url = "http://localhost:8000/recognize"
headers = {
    "X-Return-Visualization": "true",
    "X-API-Token": "your_token",
    "X-API-Key": "your_key"
}
files = {"file": open("images/0.png", "rb")}

response = requests.post(url, headers=headers, files=files)
result = response.json()
print(result["points"])  # 获取点击坐标
```

### 认证配置

API 使用环境变量进行认证，请在 `.env` 文件中配置：

```env
auth=your_token_here&&your_key_here
```

格式为 `APIToken&&APIKey`，使用 `&&` 连接。

#### 2. 服务状态检查

**GET** `/`

检查服务是否正常运行。

**响应示例**

```json
{
  "status": "ok",
  "message": "验证码识别服务运行中"
}
```
**成功示例**
```json
{
  "status": "ok",
  "message": "验证码识别服务运行中"
}
```
---

## ⚖️ 本项目使用MIT许可证
