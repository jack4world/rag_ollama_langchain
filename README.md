# rag_ollama_langchain

 利用模版rag-chroma-private创建RAG应用

我们现在开始创建应用。

首先安装LangChain CLI。这是使用LangChain模版的重要工具。

$ pip install -U langchain-cli
现在就可以在命令行中使用 langchain 命令。

利用该CLI创建LangChain应用 private-llm 。命令如下：
langchain-templates % langchain app new private-llm
What package would you like to add? (leave blank to skip): 
langchain-templates % cd private-llm 
private-llm % ls
Dockerfile      README.md       app             packages        pyproject.toml


接下来为应用添加模版 rag-chroma-private。此模板实现RAG，并不依赖外部API。它利用了Ollama提供的Llama 2模型，GPT4All用于Embedding，以及Chroma用于向量存储。

private-llm % langchain app add rag-chroma-private
Would you like to `pip install -e` the template(s)? [y/n]: y
Adding https://github.com/langchain-ai/langchain.git@master...
 - Downloaded templates/rag-chroma-private to rag-chroma-private
......
Successfully installed asgiref-3.7.2 bcrypt-4.1.1 ...

To use this template, add the following to your app:

```

from rag_chroma_private import chain as rag_chroma_private_chain

add_routes(app, rag_chroma_private_chain, path="/rag-chroma-private")
```

现在将上述命令执行的输出最后给出的代码加入app/server.py。

from rag_chroma_private import chain as rag_chroma_private_chain

add_routes(app, rag_chroma_private_chain, path="/rag-chroma-private")

这条route正是该langchain应用在该模版的支持下提供的接口。

在运行该应用前，还需要安装Ollama，以支持本地运行开源大模型，比如Llama 2 7B。

按 https://github.com/jmorganca/ollama 提供的安装指令完成安装，我们应该在命令行可以使用工具 ollama 。通过如下命令拉取期望使用的模型。模版 rag-chroma-private 使用的是模型 llama2:7b-chat，我们通过如下命令拉取：

$ ollama pull llama2:7b-chat


现在可以通过以下命令，启动应用 private-llm。

$ langchain serve


我们来试试Playground。浏览器中访问：

http://127.0.0.1:8000/rag-chroma-private/playground/



该RAG应用的知识库是什么呢？我们看看模版源码就清楚了。

https://github.com/langchain-ai/langchain/blob/master/templates/rag-chroma-private/rag_chroma_private/chain.py

模版的核心代码chain.py中，定义了加载的数据。

loader = WebBaseLoader("https://lilianweng.github.io/posts/2023-06-23-agent/")
data = loader.load()


正是这一篇博文 LLM Powered Autonomous Agents：

https://lilianweng.github.io/posts/2023-06-23-agent/

大家可以基于这篇文章的内容来问答。