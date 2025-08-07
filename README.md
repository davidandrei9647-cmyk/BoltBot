from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes
import datetime

# Define zone recomandate per zi si ora (exemplu simplificat)
ZONES = {
    "joi": {
        11: "Liberty Mall / Rahova",
        12: "Liberty Mall / Rahova",
        13: "Tineretului",
        14: "Tineretului",
        15: "AFI Cotroceni",
        16: "Drumul Taberei",
        17: "Drumul Taberei",
        18: "Băneasa / Herăstrău",
        19: "Băneasa / Herăstrău",
        20: "Tineretului",
        21: "Tineretului",
        22: "Aproape de casă (Măgurele)",
        23: "Aproape de casă (Măgurele)",
    },
    "vineri": {
        11: "Liberty Mall / Rahova",
        12: "Liberty Mall / Rahova",
        13: "Tineretului",
        14: "Tineretului",
        15: "AFI Cotroceni",
        16: "Drumul Taberei",
        17: "Drumul Taberei",
        18: "Băneasa / Herăstrău",
        19: "Băneasa / Herăstrău",
        20: "Tineretului",
        21: "Tineretului",
        22: "Aproape de casă (Măgurele)",
        23: "Aproape de casă (Măgurele)",
    },
    "sambata": {
        11: "Liberty Mall / Rahova",
        12: "Liberty Mall / Rahova",
        13: "Tineretului",
        14: "Tineretului",
        15: "AFI Cotroceni",
        16: "Drumul Taberei",
        17: "Drumul Taberei",
        18: "Băneasa / Herăstrău",
        19: "Băneasa / Herăstrău",
        20: "Tineretului",
        21: "Tineretului",
        22: "Aproape de casă (Măgurele)",
        23: "Aproape de casă (Măgurele)",
    }
}

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "Salut! Trimite-mi mesaj de forma:\n"
        "/zona <zi> <ora>\n"
        "Exemplu: /zona joi 14"
    )

async def zona(update: Update, context: ContextTypes.DEFAULT_TYPE):
    try:
        zi = context.args[0].lower()
        ora = int(context.args[1])
    except (IndexError, ValueError):
        await update.message.reply_text("Te rog scrie comanda corect: /zona <zi> <ora>\nExemplu: /zona joi 14")
        return

    if zi not in ZONES:
        await update.message.reply_text("Ziua trebuie să fie: joi, vineri sau sambata")
        return

    if ora not in ZONES[zi]:
        await update.message.reply_text("Ora trebuie să fie între 11 și 23")
        return

    recomandare = ZONES[zi][ora]
    await update.message.reply_text(f"Recomandare pentru {zi} ora {ora}: {recomandare}")

def main():
    application = ApplicationBuilder().token("8351225742:AAHRwvSiok0KntvHzAAQsyUAJ6_K0XyvOYY").build()

    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("zona", zona))

    print("Botul este pornit...")
    application.run_polling()

if __name__ == '__main__':
    main()
