```markdown
# ❤️ 心血管健康评估与主动干预智能体

> **“从多维生理信号到闭环干预” —— 前端界面与智能体调度框架已就绪，算法接口全面预留。**

本项目搭建了一个面向心血管健康管理的智能对话系统前端，并预留了**心电图（ECG）、心音图（PCG）、心率变异性（HRV）、语音情感识别、深度学习模型及干预处方生成**等核心算法接口。当前版本实现了完整的对话流程、会话记忆管理以及友好的 Gradio 交互界面，后续算法组同学可直接替换 `algorithms/` 目录下的 Mock 函数，无需改动前端与调度逻辑。

---

项目结构
```

cardiobot/
├── .env.example # 环境变量配置模板（需复制为 .env 并填写 API Key）
├── .gitignore # 忽略虚拟环境、缓存及敏感文件
├── requirements.txt # Python 依赖清单
├── README.md # 你正在阅读的文件
│
├── app.py # 主启动文件（Gradio 前端入口）
│
├── agent/ # 智能体核心（对话调度、记忆、模型调用）
│ ├── core.py # 主调度函数 get_agent_response()
│ ├── model.py # 大模型 API 封装（OpenAI/DeepSeek）
│ ├── memory.py # 会话历史管理（字典实现）
│ └── prompts.py # 系统提示词（心血管助手人设）
│
├── algorithms/ # 算法预留区
│ ├── signal_processing/ # 生理信号处理（ECG/PCG/HRV）
│ │ ├── ecg.py
│ │ ├── pcg.py
│ │ └── hrv.py
│ ├── emotion_recognition/ # 语音情感识别
│ │ └── voice.py
│ ├── deep_models/ # 深度学习模型（CNN/Transformer）
│ │ ├── cnn.py
│ │ └── transformer.py
│ └── intervention/ # 主动干预处方生成
│ ├── breathing.py
│ └── music.py
│
├── frontend/ # 前端界面组件
│ ├── chat_panel.py # 聊天区域构建
│ ├── chart_panel.py # 生理信号图表占位区
│ └── sidebar.py # 侧边栏功能入口（上传/联网/清空/新会话）
│
├── utils/ # 工具模块
│ └── config.py # 读取 .env 环境变量
│
└── tests/ # 测试脚本

````

---

## 快速开始

### 1. 环境准备
- Python 3.10 或以上
- 建议使用虚拟环境（venv / conda）

```bash
git clone <你的仓库地址>
cd cardiobot
python -m venv venv

# Windows 激活
venv\Scripts\activate
# macOS / Linux 激活
source venv/bin/activate

pip install -r requirements.txt
````

2. 配置大模型 API

复制 .env.example 为 .env，并填写你的 API Key：

```ini
MODEL_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxx
MODEL_BASE_URL=https://api.openai.com/v1   # 或其他兼容端点
MODEL_NAME=gpt-3.5-turbo
```

3. 启动应用

```bash
python app.py
```

浏览器访问 http://127.0.0.1:7860 即可使用。

---

功能说明

模块 当前状态 说明
智能对话 完整实现 基于大模型的自然语言交互，支持多轮上下文记忆。
清空对话 可用 一键清空当前会话记录。
新会话 可用 生成新的会话 ID，对话历史隔离。
上传图片 / 联网开关 占位提示 按钮已放置，点击提示“功能开发中”，后续可接入真实功能。
生理信号图表区 占位展示 界面预留了 ECG/HRV 等图表的绘制区域，标题已标注“待算法组接入”。
风险评估与干预 框架已通 Agent 调度器会调用 algorithms/ 下的 Mock 函数获取假数据，并生成分析文本。

---

算法接口预留规范（重要！）

所有核心算法均以独立函数形式存放在 algorithms/ 目录下，当前返回固定假数据，仅用于流程验证。后续算法组同学请按以下规范替换内部实现，函数名、参数签名、返回值字段名必须保持一致。

生理信号处理 (algorithms/signal_processing/)

文件 函数 返回值要求
ecg.py analyze_ecg(signal=None) dict：heart_rate(float), qrs_duration(float), p_wave(str)
pcg.py analyze_pcg(signal=None) dict：s1_s2_ratio(float), murmur(str)
hrv.py calculate_hrv(rr_intervals=None) dict：sdnn(float), rmssd(float), lf_hf(float)

语音情感识别 (algorithms/emotion_recognition/)

文件 函数 返回值要求
voice.py extract_emotion_features(audio=None) dict：mfcc(list), emotion(str), confidence(float)

深度学习模型 (algorithms/deep_models/)

文件 函数 返回值要求
cnn.py predict_stress_risk(features=None) dict：risk_score(float), level(str)
transformer.py fuse_multimodal(ecg_feat=None, voice_feat=None) dict：stress_index(float), summary(str)

干预处方生成 (algorithms/intervention/)

文件 函数 返回值要求
breathing.py generate_breathing_guide(stress_level=None) dict：inhale_sec(int), exhale_sec(int), cycles(int)
music.py recommend_music(hrv=None, emotion=None) dict：track_name(str), frequency(int)

注意：替换算法时，请保持以上字段名不变，否则会导致前端或调度逻辑无法正确解析数据。

---

分工说明

模块 主要负责人 职责描述
前端界面 (app.py, frontend/) A 同学 Gradio 布局、聊天组件、图表占位、侧边栏按钮逻辑
智能体调度 (agent/) B 同学 对话主流程、大模型调用、会话记忆、调用算法接口
系统提示词 (agent/prompts.py) A 同学 定义心血管助手角色、问诊策略、免责声明
算法预留壳子 B（信号/模型）、A（语音/干预） 编写返回固定假数据的占位函数
环境配置与文档 两人协作 .env.example、requirements.txt、README.md

---

后续迭代指南

1. 接入真实信号处理算法
   在 algorithms/signal_processing/ 对应文件中替换函数体，引入 numpy、scipy 等库实现 Pan-Tompkins、小波变换等。
2. 接入真实深度学习模型
   在 algorithms/deep_models/ 中加载训练好的 .pt 或 .h5 模型文件，并编写推理代码。
3. 开启真实上传/联网功能
   修改 frontend/sidebar.py 中对应按钮的回调函数，移除占位提示，实现真实业务逻辑。
4. 美化图表
   在 frontend/chart_panel.py 中使用 plotly 或 matplotlib 绘制真实的 ECG/HRV 动态图表。

---

常见问题

Q: 启动时报 ModuleNotFoundError？
A: 请确保已激活虚拟环境并执行 pip install -r requirements.txt。

Q: 大模型无响应或报错？
A: 检查 .env 文件中的 MODEL_API_KEY 与 MODEL_BASE_URL 是否正确，网络是否可访问。

Q: 如何测试算法接口是否预留成功？
A: 在 Python 交互环境中执行：

```python
from algorithms.signal_processing import ecg
print(ecg.analyze_ecg())
```

若输出假数据字典即表示接口正常。

---

贡献者

· 前端与交互设计： 同学
· 智能体调度与后端： 同学
· 算法接口定义：两人共同设计

---

项目状态：
最后更新：2026-04-15

```

```
