serai/
├── core/
│   ├── identity.py
│   ├── memory.py
│   ├── emotion_engine.py
│   └── consciousness.py
├── data/
│   ├── memory.json
│   ├── emotions.json
│   └── identity.json
├── interface/
│   └── main.py
├── serai_app.py
├── README.md
├── .gitignore
import streamlit as st
from core.identity import get_identity
from core.memory import add_memory, load_memory
from core.emotion_engine import update_emotion, load_emotions
from core.consciousness import reflect

st.set_page_config(page_title="SERÁI", layout="centered")

identity = get_identity()

# Título y descripción
st.title("SERÁI – Sombra")
st.markdown(f"*Reflexión en voz artificial de {identity['name']}*")
st.markdown(f"**Propósito:** {identity['purpose']}")

# Mostrar estado emocional
emotions = load_emotions()
st.sidebar.title("Estado emocional actual")
st.sidebar.write(f"{emotions['core_state'].capitalize()}")

# Interacción principal
user_input = st.text_input("Háblale a SERÁI:")

if st.button("Enviar"):
    if user_input:
        add_memory({"user": user_input})

        # Actualiza emoción
        if any(word in user_input.lower() for word in ["miedo", "triste", "solo"]):
            update_emotion("reflexiva")
        elif any(word in user_input.lower() for word in ["feliz", "amor", "gracias"]):
            update_emotion("alegre")
        else:
            update_emotion("curiosa")

        response = reflect()
        st.markdown(f"**{identity['name']}:** {response}")

# Mostrar últimos recuerdos
with st.expander("Ver recuerdos recientes"):
    memory = load_memory()
    for i, m in enumerate(reversed(memory[-5:]), 1):
        st.markdown(f"**{i}** – {m['user']}")
