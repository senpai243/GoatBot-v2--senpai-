import streamlit as st
import random

# -------------------------
# Questions/Réponses
# -------------------------
questions = {
    "Quel est le nom du démon renard dans Naruto ?": "kurama",
    "Quel est le fruit du démon de Luffy ?": "gomu gomu",
    "Qui est le rival de Light Yagami dans Death Note ?": "l",
    "Comment s'appelle le père de Goku ?": "bardock",
    "Quel est le clan de Sasuke ?": "uchiha",
    "Qui est le Hokage dans Boruto ?": "naruto",
    "Qui est le capitaine de l’équipage du Chapeau de Paille ?": "luffy",
    "Quel est le vrai nom de Zoro ?": "roronoa zoro",
    "Quel est le prénom de l’élève de Jiraya ?": "naruto",
    "Quel est le nom complet du rival de Yugi ?": "seto kaiba"
}

# -------------------------
# Interface Streamlit
# -------------------------
st.set_page_config(page_title="Quiz Anime Multijoueur 🌀", page_icon="🧠")
st.title("🎮 Quiz Multijoueur - Spécial Animés 🍥🔥")
st.markdown("Réponds à **5 questions**. Si tu rates 5 fois... 💀 Game over !")

# Nom du joueur
joueur = st.text_input("Entre ton prénom pour commencer 👇")

# Démarrer le quiz
if joueur:
    if "score" not in st.session_state:
        st.session_state.score = 0
        st.session_state.erreurs = 0
        st.session_state.questions_restantes = random.sample(list(questions.items()), 5)
        st.session_state.index = 0

    if st.session_state.index < len(st.session_state.questions_restantes):
        question, bonne_reponse = st.session_state.questions_restantes[st.session_state.index]
        st.subheader(f"Question {st.session_state.index + 1} :")
        reponse = st.text_input(question, key=st.session_state.index)

        if reponse:
            if reponse.lower() == bonne_reponse:
                st.success("✅ Bonne réponse !")
                st.session_state.score += 1
            else:
                st.error(f"❌ Faux ! La bonne réponse était : {bonne_reponse}")
                st.session_state.erreurs += 1

            st.session_state.index += 1
            st.experimental_rerun()
    else:
        st.markdown("### 🏁 Fin du quiz")
        st.info(f"🔢 Score final de {joueur} : {st.session_state.score} / 5")

        if st.session_state.erreurs >= 5:
            st.error("💀 Tu as perdu ! Trop de mauvaises réponses.")
        elif st.session_state.score == 5:
            st.success("🥇 Tu es un vrai Hokage !")
        elif st.session_state.score >= 3:
            st.success("🥈 Tu progresses, jeune ninja.")
        else:
            st.warning("🥉 Continue ton entraînement.")

        # Rejouer
        if st.button("🔁 Rejouer"):
            del st.session_state.score
            del st.session_state.erreurs
            del st.session_state.questions_restantes
            del st.session_state.index
            st.experimental_rerun()
