请直接说明来意，将会尽快回复你。
from telegram import InlineKeyboardButton, InlineKeyboardMarkup, Update
from telegram.ext import Application, CommandHandler, CallbackQueryHandler, CallbackContext

async def start(update: Update, context: CallbackContext) -> None:
    keyboard = [
        [InlineKeyboardButton("同意", callback_data="agree")],
        [InlineKeyboardButton("不同意", callback_data="disagree")],
    ]
    reply_markup = InlineKeyboardMarkup(keyboard)
    await update.message.reply_text("请选择：", reply_markup=reply_markup)

async def button(update: Update, context: CallbackContext) -> None:
    query = update.callback_query
    await query.answer()
    if query.data == "agree":
        # 允许双向私聊
        await query.edit_message_text(text="已同意，可以进行双向私聊。")
    elif query.data == "disagree":
        # 禁止双向私聊
        await query.edit_message_text(text="已拒绝，无法进行双向私聊。")

def main():
    application = Application.builder().token("YOUR_BOT_TOKEN").build()
    application.add_handler(CommandHandler("start", start))
    application.add_handler(CallbackQueryHandler(button))
    application.run_polling()

if __name__ == "__main__":
    main()
