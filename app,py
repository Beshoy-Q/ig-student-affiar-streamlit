import os

import requests
import streamlit as st


API_URL = os.getenv("FAQ_API_URL", "http://187.77.71.97:4001").rstrip("/")


st.set_page_config(
    page_title="IG Student Affairs FAQ",
    page_icon="🎓",
    layout="centered",
)


def initialize_chat() -> None:
    if "messages" not in st.session_state:
        st.session_state.messages = []


def render_messages() -> None:
    for message in st.session_state.messages:
        with st.chat_message(message["role"]):
            st.markdown(message["content"])


initialize_chat()

st.title("🎓 IG Student Affairs FAQ")
st.caption("Ask a question about programs, schedules, payments, LMS, certificates, and more.")

with st.sidebar:
    st.header("Chat controls")
    if st.button("Clear conversation", use_container_width=True):
        st.session_state.messages = []
        st.rerun()

render_messages()

question = st.chat_input("اكتب سؤالك هنا...")
if question:
    st.session_state.messages.append({"role": "user", "content": question})
    with st.chat_message("user"):
        st.markdown(question)

    with st.chat_message("assistant"):
        with st.spinner("جاري تجهيز الرد..."):
            try:
                result = requests.post(
                    f"{API_URL}/chat",
                    json={"question": question},
                    timeout=90,
                )
                result.raise_for_status()
                response = result.json()["response"]
            except requests.RequestException:
                response = "تعذر الاتصال بالخدمة حالياً. حاول مرة أخرى بعد قليل."
            except (KeyError, ValueError, TypeError):
                response = "وصل رد غير متوقع من الخدمة. حاول مرة أخرى."

        st.markdown(response)
        st.session_state.messages.append({"role": "assistant", "content": response})
