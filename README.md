import os
from flask import Flask, request
import requests

TOKEN = os.environ.get("TELEGRAM_TOKEN")
URL = f"https://api.telegram.org/bot{TOKEN}"

app = Flask(__name__)

@app.route("/")
def home():
    return "Bot Telegram está online 🚀"

@app.route(f"/{TOKEN}", methods=["POST"])
def webhook():
    data = request.get_json()

    if "message" in data:
        chat_id = data["message"]["chat"]["id"]
        text = data["message"].get("text", "")

        reply = f"Recebi: {text}"

        requests.post(
            f"{URL}/sendMessage",
            json={
                "chat_id": chat_id,
                "text": reply
            }
        )

    return "ok"

if __name__ == "__main__":
    port = int(os.environ.get("PORT", 10000))
    app.run(host="0.0.0.0", port=port)
