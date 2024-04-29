
# oLLaMaStreamPhi
# Build an open source AI assistant with Streamlit, Microsoft Phi-3 & Ollama
The codebase has code for building Chatbot using the Streamlit Python library. 
For accessing LLMs, it uses the Ollama Python library which is a wrapper around Ollama REST APIs.
Ollama is a CLI tool that lets us run open-source LLMs on our computers for free.
Feel free to check the below video to understand the code and set up Chatbot on your computer.
* [AI assisstant using Streamlit, Phi3 and Ollama](https://www.youtube.com/@JalelTounsi)

Let's build a chatbot with just Python using the Streamlit library, Ollama, and Microsoft Phi-3. 

Streamlit
Streamlit turns data scripts into shareable web apps in minutes. All in pure Python. No front‑end experience required.
You can find more info in the official Streamlit docs.


Ollama
Ollama allows you to run open-source large language models, locally
You can find more info in the official Ollama docs.


Phi-3 Mini
Phi-3 Mini is a 3.8B parameters, lightweight, state-of-the-art open model by Microsoft.
You can find more info in the official Phi-3 Mini docs.


Steps

Create a new conda environment
conda create --name envStreamPhi
Activate the environment
conda activate envStreamPhi
Clone StreamLit template
git clone https://github.com/streamlit/streamlit.git
Install ollama


pull the phi-3 model
the model we will be using is located here:
ollama pull phi3
Pull the Embeddings model:
ollama pull nomic-embed-text
Test installation
streamlit hello
Build the AI assisstant

In order to build the AI assisstant, you have 2 choices : clone the repo and get all the code from the get-go or coding along with me.

I - First option : Clone the project from Github 
git clone https://github.com/JalelTounsi/oLLaMaStreamPhi.git

run the application
streamlit run app.py

II - Second option: code along
Create your app.py file

Add imports
import streamlit as st
import ollama

Add the defacto message
if "messages" not in st.session_state:
    st.session_state["messages"] = [{"role": "assistant", "content": "Hello tehre, how can I help you, today?"}]
Add the message history
for msg in st.session_state.messages:
    if msg["role"] == "user":
        st.chat_message(msg["role"], avatar="🧑‍💻").write(msg["content"])
    else:
        st.chat_message(msg["role"], avatar="🤖").write(msg["content"])
Configure model
def generate_response():
    response = ollama.chat(model='phi3', stream=True, messages=st.session_state.messages)
    for partial_resp in response:
        token = partial_resp["message"]["content"]
        st.session_state["full_message"] += token
        yield token
Configure the prompt
if prompt := st.chat_input():
    st.session_state.messages.append({"role": "user", "content": prompt})
    st.chat_message("user", avatar="🧑‍💻").write(prompt)
    st.session_state["full_message"] = ""
    st.chat_message("assistant", avatar="🤖").write_stream(generate_response)
    st.session_state.messages.append({"role": "assistant", "content": st.session_state["full_message"]})   

all the codebase of app.py
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
    
Run the Streamlit app 
streamlit run app.py
