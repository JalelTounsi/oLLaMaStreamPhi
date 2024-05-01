
# oLLaMaStreamPhi
# Build an open source AI assistant with Streamlit, Microsoft Phi-3 & Ollama

Feel free to check the below video to understand the code, follow along and run the AI assistant locally on your computer.
[AI assistant using Streamlit, Phi3 and Ollama](https://www.youtube.com/watch?v=4RWdMbNaEBs)

Let's build a chatbot with just Python using the Streamlit library, Ollama, and Microsoft Phi-3. 

## Streamlit: 
turns data scripts into shareable web apps in minutes. All in pure Python. No front‑end experience required.
You can find more info in the official Streamlit docs.


## Ollama: 
allows you to run open-source large language models, locally
You can find more info in the official Ollama docs.


## Phi-3 Mini: 
is a 3.8B parameters, lightweight, state-of-the-art open model by Microsoft.
You can find more info in the official Phi-3 Mini docs.


# Steps

1 - Create a new conda environment
```
conda create --name envStreamPhi
```
2 - Activate the environment
```
conda activate envStreamPhi
```
3 - Clone StreamLit template
```
git clone https://github.com/streamlit/streamlit.git
```
4 - Install ollama & pull the phi-3 model
```
ollama pull phi3
```
5 - Pull the Embeddings model:
```
ollama pull nomic-embed-text
```
6 - Test installation
```
streamlit hello
```

# Build the AI assistant

In order to build the AI assistant, you have 2 choices : clone the repo and get all the code from the get-go or coding along with me.

## I - First option : 
1 - Clone the project from Github 
```
git clone https://github.com/JalelTounsi/oLLaMaStreamPhi.git
```
2 - run the application
```
streamlit run app.py
```

## II - Second option: 
code along
1 - Create your app.py file
```
app.py
```
2 - Add imports
```
import streamlit as st
import ollama
```

3 - Add the defacto message
```
if "messages" not in st.session_state:
    st.session_state["messages"] = [{"role": "assistant", "content": "Hello tehre, how can I help you, today?"}]
```
4 - Add the message history
```
for msg in st.session_state.messages:
    if msg["role"] == "user":
        st.chat_message(msg["role"], avatar="🧑‍💻").write(msg["content"])
    else:
        st.chat_message(msg["role"], avatar="🤖").write(msg["content"])
```
5 - Configure model
```
def generate_response():
    response = ollama.chat(model='phi3', stream=True, messages=st.session_state.messages)
    for partial_resp in response:
        token = partial_resp["message"]["content"]
        st.session_state["full_message"] += token
        yield token
```
6 - Configure the prompt
```
if prompt := st.chat_input():
    st.session_state.messages.append({"role": "user", "content": prompt})
    st.chat_message("user", avatar="🧑‍💻").write(prompt)
    st.session_state["full_message"] = ""
    st.chat_message("assistant", avatar="🤖").write_stream(generate_response)
    st.session_state.messages.append({"role": "assistant", "content": st.session_state["full_message"]})   
```
7 - all the codebase of app.py
```
import streamlit as st
import ollama

st.title("💬 Phi3 Chatbot")

if "messages" not in st.session_state:
    st.session_state["messages"] = [{"role": "assistant", "content": "Hello tehre, how can I help you, today?"}]

### Write Message History
for msg in st.session_state.messages:
    if msg["role"] == "user":
        st.chat_message(msg["role"], avatar="🧑‍💻").write(msg["content"])
    else:
        st.chat_message(msg["role"], avatar="🤖").write(msg["content"])

## Configure the model
def generate_response():
    response = ollama.chat(model='phi3', stream=True, messages=st.session_state.messages)
    for partial_resp in response:
        token = partial_resp["message"]["content"]
        st.session_state["full_message"] += token
        yield token

if prompt := st.chat_input():
    st.session_state.messages.append({"role": "user", "content": prompt})
    st.chat_message("user", avatar="🧑‍💻").write(prompt)
    st.session_state["full_message"] = ""
    st.chat_message("assistant", avatar="🤖").write_stream(generate_response)
    st.session_state.messages.append({"role": "assistant", "content": st.session_state["full_message"]})   
```
Run the Streamlit app 
```
streamlit run app.py
```
