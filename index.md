---
layout: default
---

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](./another-page.html).


# 1 开始
## 1.1 安装
1. 安装 langchain 库


```python
!pip install langchain
# or
!conda install langchain -c conda-forge
```

2. 安装 openai 库


```python
!pip install openai
```

---
## 1.2 配置环境并导入依赖
1. 方式一：直接添加电脑环境变量 `OPEN_AI_KEY="..."`
2. 方式二：在终端运行`export OPENAI_API_KEY="..."`
3. 方式三：在jupyter中运行如下代码


```python
import os
os.environ["OPENAI_API_KEY"] = "..."
```

配置好环境变量后，获取 OpenAI 接口的 api_key


```python
import os
import openai

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

---
## 1.3 LLM的初始化和调用

LangChain中有两种类型的语言模型，分别称为：

- LLMs：普通语言模型。该类模型将字符串作为输入，并返回字符串。如：`langchain.llms.OpenAI()`
- ChatModels：对话语言模型。该类模型将以 langchain 中的 `ChatMessage` 列表作为输入，并输出单个`ChatMessage`。如`langchain.chat_models.ChatOpenAI()`

LangChain为两种模型提供了不同的接口用于内容生成：

- 对于 LLMs，使用 `predict` 方法接受一个字符串
- 对于ChatModels: 使用 `predict_message` 接收 `ChatMessage` 列表

首先，以构建 **“基于公司产品生成公司名称”** 的任务为例，使用 LLMs 类型的模型进行生成


```python
from langchain.llms import OpenAI # 导入指定的语言模型

text = "What would be a good company name for a company that makes colorful socks?"
llm = OpenAI() # 初始化LLM
llm.predict(text) # 使用predict方法进行生成
```




    '\n\nSocktastic!'



然后，我们来使用 ChatModel 类型的模型进行生成，该类模型可以设置历史对话信息。

LangChain 中，对于 `ChatMessage`需要指定两部分内容:
- `role`: 产生该消息的实体的角色
- `content`: 该消息的具体内容

LangChain也提供了几个包装类来方便构建不同角色的消息：
- `HumanMessage`: 来自用户的消息
- `AIMessage`: 来自AI助手（LLM）的信息
- `SystemMessage`: 通常指第一轮对话前提供给系统的背景信息
- `FunctionMessage`: 函数调用产生的信息（较少用）

仍然以构建 **“基于公司产品生成公司名称”** 的任务为例


```python
from langchain.schema import HumanMessage, ChatMessage
from langchain.chat_models import ChatOpenAI

text = "What would be a good company name for a company that makes colorful socks?"
chat_model = ChatOpenAI() # 初始化LLM

messages = [HumanMessage(content=text)] 
# messages = [ChatMessage(role='human', content='text')] # 上一行代码等价

chat_model.predict_messages(messages) # 使用predict_message方法进行生成
```




    AIMessage(content='VibrantFootwear', additional_kwargs={}, example=False)


