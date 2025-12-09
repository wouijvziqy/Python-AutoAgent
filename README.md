# 智能渗透测试 Agent 系统

基于 LLM + 代码生成 + 沙箱执行的自动化渗透测试框架

## 🎯 系统架构

```
用户请求 → Agent (LLM) → 生成Python代码 → 沙箱执行 → 返回结果 → Agent分析 → 迭代执行
```

### 核心组件

1. **Agent 引擎** - 基于 LLM 的决策中心
2. **工具管理器** - 统一管理各类安全工具
3. **沙箱执行器** - 安全隔离的代码执行环境
4. **上下文窗口** - 维护完整的测试链路

## 🚀 快速开始

### 1. 安装依赖

```bash
pip install -r requirements.txt
```

### 2. 配置环境变量

```bash
cp .env.example .env
# 编辑 .env 文件，填入你的 OpenAI API Key
```

### 3. 运行示例

```bash
python smart_pentest_agent.py
```

## 📦 系统特性

### ✅ 已实现功能

- **多工具集成**: Nmap、漏洞扫描、Web爬虫、SQL注入检测
- **安全沙箱**: 限制代码执行权限，防止恶意操作
- **上下文记忆**: 保持测试链路的连贯性
- **自动迭代**: 根据扫描结果动态调整策略
- **结构化输出**: 生成标准化的渗透测试报告

### 🔧 可扩展功能

- 接入真实的 LLM API (OpenAI/Claude/本地模型)
- 集成更多安全工具 (Metasploit, Burp Suite, etc.)
- 实现真实的 Nmap 调用
- 添加漏洞利用模块
- 支持分布式扫描
- Web UI 界面

## 📖 使用示例

### 基础用法

```python
from smart_pentest_agent import SmartPentestAgent, MockLLM

# 创建 Agent
agent = SmartPentestAgent(llm=MockLLM())

# 执行渗透测试
result = agent.run("扫描 example.com 并检测所有高危漏洞")

# 查看报告
print(result["report"])
```

### 使用真实 LLM

```python
from smart_pentest_agent import SmartPentestAgent, OpenAILLM

# 配置 OpenAI
llm = OpenAILLM(api_key="your-api-key", model="gpt-4")

# 创建 Agent
agent = SmartPentestAgent(llm=llm)

# 执行任务
result = agent.run("对 192.168.1.100 进行全面的安全评估")
```

## 🛠️ 自定义工具

### 添加新工具

```python
from smart_pentest_agent import SecurityToolInterface

class CustomTool(SecurityToolInterface):
    def get_tool_description(self) -> str:
        return "工具描述"
    
    def execute(self, *args, **kwargs):
        # 工具实现
        return {"result": "data"}

# 注册到工具管理器
agent.tool_manager.tools["custom_tool"] = CustomTool()
```

## 🔒 安全注意事项

1. **仅在授权环境中使用** - 未经授权的渗透测试是违法的
2. **沙箱隔离** - 代码在受限环境中执行，但仍需谨慎
3. **API Key 保护** - 不要将 API Key 提交到版本控制
4. **速率限制** - 避免对目标造成 DoS 影响

## 📊 系统流程图

```
┌─────────┐
│  用户   │
└────┬────┘
     │ 输入: "扫描 example.com"
     ▼
┌─────────────┐
│   Agent     │ ◄─── 上下文窗口
│   (LLM)     │
└──────┬──────┘
       │ 生成代码
       ▼
┌──────────────────┐
│  Python Executor │
│   (Sandbox)      │
└──────┬───────────┘
       │ 调用工具
       ▼
┌──────────────────┐
│   Tool Manager   │
│  - Nmap          │
│  - VulnScanner   │
│  - WebCrawler    │
└──────┬───────────┘
       │ 返回原始数据
       ▼
┌──────────────────┐
│  Code Processing │
│  (数据清洗/分析)  │
└──────┬───────────┘
       │ 返回 report
       ▼
┌──────────────────┐
│   Agent 分析     │
│  (决定是否迭代)   │
└──────┬───────────┘
       │
       ▼
   最终报告
```

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

MIT License

## ⚠️ 免责声明

本工具仅供学习和授权测试使用。使用者需自行承担使用本工具的法律责任。
