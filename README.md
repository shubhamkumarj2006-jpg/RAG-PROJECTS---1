import  streamlit as st 
import ollama 

MODEL = "llama3.2"

st.set_page_config(
    page_title="Ollama Mini Chatbot",
    page_icon="🤖"
)
st.title("🤖 Ollama Mini Chatbot")
st.caption("your local AI assistant")

if "messages" not in st.session_state:
    st.session_state.messages = []
    with st.sidebar:
        st.subheader("Settings")
        st.write(f"Model:{MODEL}")

if st.button("Clear Chat"):
            st.session_state.messages = []
            st.rerun()
for message in st.session_state.messages:
       with st.chat_message(message["role"]):
              st.markdown(message["content"])

              prompt = st.chat_input("Ask me anything.....")
              if prompt:
                     st.session_state.messages.append({
                            "role":"user",
                            "content":prompt 
                     })
                     with st.chat_message("user"):
                            st.markdown(prompt)
                            with st.chat_message("assistant"):
                                   try:
                                          with st.spinner("Thinking..."):
                                                 response = ollama.chat(
                                                        model=MODEL,
                                                        messages=st.session_state.messages 
                                                 )
                                                 answer = response["message"]["content"]
                                                 st.markdown(answer)
                                                 st.session_state.messages.append({
                                                        "role":"assistant",
                                                        "content":answer 
                                                 })
                                   except Exception as error:
                                          st.error(
                                                 "Could not connect to Ollama."
                                                 "Check that Ollama is running and"
                                                 "the model is installed."
                                          )
                                          st.caption(str(error))
                                          
                                          
                                                 


