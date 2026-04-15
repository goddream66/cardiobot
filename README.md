cardiobot/
├── .env.example # 环境变量配置模板（需复制为 .env 并填写 API Key）
├── .gitignore # 忽略虚拟环境、缓存及敏感文件
├── requirements.txt # Python 依赖清单
├── README.md # 项目说明文档
│
├── app.py # 主启动文件（Gradio 前端入口）
│
├── agent/ # 智能体核心（对话调度、记忆、模型调用）
│ ├── **init**.py
│ ├── core.py # 主调度函数 get_agent_response()
│ ├── model.py # 大模型 API 封装（OpenAI/DeepSeek）
│ ├── memory.py # 会话历史管理
│ └── prompts.py # 系统提示词
│
├── algorithms/ # 算法预留区
│ ├── **init**.py
│ ├── signal_processing/ # 生理信号处理（ECG/PCG/HRV）
│ │ ├── **init**.py
│ │ ├── ecg.py
│ │ ├── pcg.py
│ │ └── hrv.py
│ ├── emotion_recognition/ # 语音情感识别
│ │ ├── **init**.py
│ │ └── voice.py
│ ├── deep_models/ # 深度学习模型（CNN/Transformer）
│ │ ├── **init**.py
│ │ ├── cnn.py
│ │ └── transformer.py
│ └── intervention/ # 主动干预处方生成
│ ├── **init**.py
│ ├── breathing.py
│ └── music.py
│
├── frontend/ # 前端界面组件
│ ├── **init**.py
│ ├── chat_panel.py # 聊天区域构建
│ ├── chart_panel.py # 生理信号图表占位区
│ └── sidebar.py # 侧边栏功能入口（上传/联网/清空/新会话）
│
├── utils/ # 工具模块
│ ├── **init**.py
│ └── config.py # 读取 .env 环境变量
│
└── tests/ # 测试脚本
├── test_agent.py
└── test_algorithms_mock.py
