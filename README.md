# Streamlit-Webapp-ChatGPT
Your own customizable ChatGPT Webapp made in Python using Streamlit.

## Running the app

1) Simply clone this repository using:
   ```shell
   $ git clone https://github.com/anishsingh20/Streamlit-Webapp-ChatGPT/
   ```

2) Install required libraries:
   ```shell
   pip install streamlit openai
   ``` 
3) Get inside the directory ```Streamlit-GPT-app```.
   
4) Run the following python ```GptWebappv2.py``` file which has the source code using command:
   
   ```shell
    $ streamlit run chatbot_app.py
   ```
   <img width="1673" alt="Screenshot 2023-07-25 at 8 21 36 AM" src="https://github.com/parvinder0201/Chatgpt_bot-3.5-/blob/main/Screenshot%202024-04-11%20010843.png">


## INTRODUCTION

[Streamlit](https://www.streamlit.io/) is an open-source Python library that makes it easy to create and share beautiful, custom web apps for machine learning and data science. You can build and deploy powerful data apps in just a few minutes. So let's get started!

Imagine having your AI-powered chatbot web app ready to answer questions, generate creative content, and assist you in your daily tasks. Thanks to OpenAI's powerful language models and easy-to-use APIs, building a personal AI assistant is now within reach of any Python developer. This tutorial will show you how to create your ChatGPT bot using the Streamlit framework and OpenAI's GPT-3.5-turbo model.

### 1. Setting Up OpenAI API Key and Streamlit

Before building the ChatGPT bot, ensure access to OpenAI's API. You can sign up for an [API key](https://platform.openai.com/account/api-keys) on the [OpenAI website](https://platform.openai.com/account/api-keys) if you haven't already. Once you have your API key, set it up as an environment variable in your development environment on the terminal/shell:

```shell 
$ export OPENAI_API_KEY=<YOUR_API_KEY>
```

Next, install the required libraries and set up your Python environment:

```shell
$ pip install streamlit openai
```


### 2. Building the ChatGPT Bot with Streamlit

Let's dive into the code to build our chatbot using Streamlit. Create a Python file (e.g., chatbot_app.py) and follow along:

```python
import openai
import streamlit as st
import toml

secrets = toml.load("streamlit/secrets.toml")

st.title("Chat Bot (GPT-3.5)")

openai.api_key = secrets["OPENAI_API_KEY"]

if "openai_model" not in st.session_state:
    st.session_state["openai_model"] = "gpt-3.5-turbo"

if "messages" not in st.session_state:
    st.session_state.messages = []

for message in st.session_state.messages:
    with st.chat_message(message["role"]):
        st.markdown(message["content"])

if prompt := st.chat_input("What is up?"):
    st.session_state.messages.append({"role": "user", "content": prompt})
    with st.chat_message("user"):
        st.markdown(prompt)

    with st.chat_message("assistant"):
        message_placeholder = st.empty()
        full_response = ""
        for response in openai.ChatCompletion.create(
            model=st.session_state["openai_model"],
            messages=[
                {"role": m["role"], "content": m["content"]}
                for m in st.session_state.messages
            ],
            stream=True,
        ):
            full_response += response.choices[0].delta.get("content", "")
            message_placeholder.markdown(full_response + "▌")
        message_placeholder.markdown(full_response)
    st.session_state.messages.append({"role": "assistant", "content": full_response})

```

The above code can also be found inside the directory ```Streamlit-GPT-app\GptWebappv2.py``` 

### 3. Running the ChatGPT Bot using Streamlit

To run the ChatGPT bot, execute the following command in your terminal or command prompt:

```shell

$ streamlit run chatbot_app.py

```
<img width="1676" alt="Screenshot 2023-07-25 at 8 38 35 AM" src="https://github.com/parvinder0201/Chatgpt_bot-3.5-/blob/main/Screenshot%202024-04-11%20011303.png">
