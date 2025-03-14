from pyrogram import Client, filters

api_id = 28535962  # Apna API ID
api_hash = "bdb750e9848cdb4681137826876be976"
bot_token = "7989688279:AAEXhBJ0Kl-ENMEF4egtaMavXTor_4WIcrA"

app = Client("caption_bot", api_id=api_id, api_hash=api_hash, bot_token=bot_token)

# Dictionary to store user file references
user_files = {}

@app.on_message(filters.private & filters.document)
async def ask_caption(client, message):
    user_id = message.from_user.id
    user_files[user_id] = message
    await message.reply_text("Caption likhiye jo aap file ke sath bhejna chahte hain:")

@app.on_message(filters.private & filters.text)
async def send_file_with_caption(client, message):
    user_id = message.from_user.id
    if user_id in user_files:
        file_msg = user_files[user_id]
        caption = message.text
        await file_msg.copy(message.chat.id, caption=caption)
        del user_files[user_id]
    else:
        await message.reply_text("Pehle koi file bhejiye fir caption likhiye.")

app.run()
