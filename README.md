# Function to get user input and respond
def chat_with_bot():
    print("Chatbot is ready to talk! Type 'exit' to end the conversation.")
    while True:
        user_input = input("You: ")
        if user_input.lower() == 'exit':
            print("Goodbye!")
            break
        response = chatbot.get_response(user_input)
        print("Bot:", response)


# Start chatting
if __name__ == "__main__":
    chat_with_bot()


from flask import Flask, render_template, request
from chatterbot import ChatBot
from chatterbot.trainers import ChatterBotCorpusTrainer

app = Flask(__name__)

chatbot = ChatBot('WebChatBot')
trainer = ChatterBotCorpusTrainer(chatbot)
trainer.train('chatterbot.corpus.english')

@app.route("/")
def home():
    return render_template("index.html")

@app.route("/get", methods=["POST"])
def get_bot_response():
    userText = request.form["msg"]
    return str(chatbot.get_response(userText))

if __name__ == "__main__":
    app.run(debug=True)
