Using the rag-chroma-private template to create RAG application

Let's start creating the application now.

First, install LangChain CLI. This is an important tool for using LangChain templates.

```bash
$ pip install -U langchain-cli
```

You can now use the `langchain` command in the command line.

Create a LangChain application `private-llm` using this CLI. The command is as follows:

```bash
$ langchain app new private-llm
```

Next, add the `rag-chroma-private` template to the application. This template implements RAG and does not rely on external APIs. It utilizes the Llama 2 model provided by Ollama, GPT4All for Embedding, and Chroma for vector storage.

```bash
$ langchain app add rag-chroma-private
```

Would you like to `pip install -e` the template(s)? [y/n]: y

Adding https://github.com/langchain-ai/langchain.git@master...
 - Downloaded templates/rag-chroma-private to rag-chroma-private

Successfully installed asgiref-3.7.2 bcrypt-4.1.1 ...

To use this template, add the following to your app:

```python
from rag_chroma_private import chain as rag_chroma_private_chain

add_routes(app, rag_chroma_private_chain, path="/rag-chroma-private")
```

Now add the code provided in the output of the above command to `app/server.py`.

```python
from rag_chroma_private import chain as rag_chroma_private_chain

add_routes(app, rag_chroma_private_chain, path="/rag-chroma-private")
```

This route is the interface provided by the langchain application under this template.

Before running the application, you also need to install Ollama to support running open-source large models locally, such as Llama 2 7B.

Follow the installation instructions provided at https://github.com/jmorganca/ollama, and you should be able to use the tool `ollama` in the command line. Pull the desired model using the following command. The template `rag-chroma-private` uses the model `llama2:7b-chat`, which we pull using the following command:

```bash
$ ollama pull llama2:7b-chat
```

You can now start the `private-llm` application using the following command:

```bash
$ langchain serve
```

Upon successful startup, we expect to see the following output:


Let's try the Playground. Access it in your browser:

http://127.0.0.1:8000/rag-chroma-private/playground/


What is the knowledge base of this RAG application? Let's check the template source code to find out.

https://github.com/langchain-ai/langchain/blob/master/templates/rag-chroma-private/rag_chroma_private/chain.py

In the core code `chain.py` of the template, the data loading is defined.

```python
loader = WebBaseLoader("https://lilianweng.github.io/posts/2023-06-23-agent/")
data = loader.load()
```

It is this blog post "LLM Powered Autonomous Agents":

https://lilianweng.github.io/posts/2023-06-23-agent/

You can ask questions based on the content of this article.
