
# LangChain Agent: 网站内容总结 PDF 报告生成器

这是一个基于 LangChain 框架的 Python 项目，用于演示如何构建一个可以自主执行多步骤任务的 Agent。

该项目实现了一个自动化工作流：给定一个 URL，Agent 会自动访问该网站，使用 Playwright 浏览器工具提取并总结其内容，然后将总结报告生成为一个排版精美的 PDF 文件。

## 核心功能

* **浏览器 Agent (`summarize_website`)**: 利用 `langchain_community.agent_toolkits.PlayWrightBrowserToolkit` 创建一个能够“浏览”网页的 Agent。这使其能够处理 JavaScript 动态渲染的网站。
* **PDF 生成工具 (`generate_pdf`)**: 一个自定义工具，使用 `reportlab` 库将任意文本内容生成为 PDF 文件，并内置了中文字体（如黑体、宋体）的尝试性注册，以确保中文报告的正确显示。
* **LCEL 链式调用**: 使用 LangChain 表达式语言 (LCEL) 将工具串联起来，构建了两种不同的工作流：
    1.  **简单链**: `网站总结 -> PDF 生成`
    2.  **优化链**: `网站总结 -> LLM 优化排版 -> PDF 生成`
* **模型支持**: 本项目使用 `langchain-deepseek` 来调用 DeepSeek LLM, 此处可以换成其他LLM。

## 详细介绍

关于本项目的设计思路、LangChain Agent 的核心概念（如 ReAct、Tool Calling）以及代码的详细分步解析，请参考我的个人博客文章：

**[此处填写你的博客文章链接]**

## 快速上手

### 1. 环境配置

**a. 克隆项目**

```bash
git clone [你的 GitHub 项目 URL]
cd [你的项目目录]
````

**b. 创建并激活虚拟环境** (推荐)

```bash
# Windows
python -m venv venv
.\venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

**c. 安装依赖**

本项目依赖于一系列 Python 库。

```bash
pip install langchain langchain_community langchain-deepseek lxml langchainhub beautifulsoup4 reportlab playwright python-dotenv
```

**d. 安装 Playwright 浏览器**

Playwright 需要下载它所控制的浏览器内核，这是一个**必要步骤**：

```bash
playwright install
```

**e. 配置 API 密钥**

1.  在项目根目录创建一个名为 `.env` 的文件。

2.  打开 `.env` 文件，并添加你的 DeepSeek API 密钥：

    ```ini
    DEEPSEEK_API_KEY="sk-your_deepseek_api_key_here"
    ```

### 2\. 运行项目

一切准备就绪后，直接运行主 Python 脚本：

```bash
python langchain_agent.py
```

程序启动后，你将看到一个交互式提示：

```
选择处理方式:
1. 简单串行链（直接总结 → PDF）
2. 优化串行链（总结 → 优化 → PDF）
请选择 (1/2):
```

1.  输入 `1` 将运行简单链，直接将网站总结（可能格式杂乱）存入 PDF。
2.  输入 `2` 将运行优化链，在生成 PDF 之前，会额外调用一次 LLM 来“润色”和“排版”总结内容，使其更适合报告阅读。

任务完成后，生成的 PDF 文件（如 `website_summary_20251023_110742.pdf`）将保存在项目的根目录中。

### 修改测试 URL

如需测试你自己的 URL，请打开 `langchain_agent.py` 文件，找到 `if __name__ == "__main__":` 部分，并修改 `test_url` 变量的值：

```python
if __name__ == "__main__":
    # 修改这个 URL 来测试你自己的网站
    test_url = "[https://www.google.com](https://www.google.com)"
    
    # ... 后续运行逻辑 ...
```

