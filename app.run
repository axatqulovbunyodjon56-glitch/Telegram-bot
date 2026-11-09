import os
import asyncio
from flask import Flask
from threading import Thread
from telegram import Update
from telegram.ext import Application, CommandHandler, MessageHandler, filters, ContextTypes

# Telegram Token
TOKEN = os.getenv("TELEGRAM_TOKEN")

# /start buyrug'i uchun javob
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text("Salom! Men Render serverida ishlayotgan Telegram botman!")

# Oddiy xabarlarga javob (Aks-sado)
async def echo(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    user_message = update.message.text
    await update.message.reply_text(f"Siz yozdingiz: {user_message}")

# Botni ishga tushirish funksiyasi
async def run_bot():
    application = Application.builder().token(TOKEN).build()
    application.add_handler(CommandHandler("start", start))
    application.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, echo))
    
    await application.initialize()
    await application.start()
    await application.updater.start_polling()
    print("Bot muvaffaqiyatli ishladi!")

# Server o'chib qolmasligi uchun Flask veb-interfeysi (Render talabi)
app = Flask(__name__)

@app.route('/')
def home():
    return "Bot faol holatda!"

def run_flask():
    app.run(host='0.0.0.0', port=int(os.environ.get('PORT', 5000)))

if __name__ == "__main__":
    # Flask-ni alohida oqimda (thread) ishga tushiramiz
    t = Thread(target=run_flask)
    t.start()
    
    # Botni asosiy tsiklda ishga tushiramiz
    loop = asyncio.get_event_loop()
    loop.run_until_complete(run_bot())
    loop.run_forever()
