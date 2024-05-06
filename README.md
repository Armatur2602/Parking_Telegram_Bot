# Parking_Telegram_Bot

import telebot
from telebot import types

bot = telebot.TeleBot('6112927542:AAHX-NqfLVwY4E1zdAgbw16PpYl-hibTwq8')


@bot.message_handler(commands=['start'])
def start(message):
    markup = types.ReplyKeyboardMarkup(resize_keyboard=True, row_width=2)
# btn1 creation - Мене було оштрафовано
    btn1 = types.KeyboardButton("Мене було оштрафовано")
# btn2 creation- Я загубив повідопмлення
    btn2 = types.KeyboardButton("Я загубив повідопмлення")
# btn3 creation - Мені невідомо про наявність штрафу
    btn3 = types.KeyboardButton("Мені невідомо про наявність штрафу")
    markup.add(btn1, btn2, btn3)
# send_message in chat
    bot.send_message(message.chat.id,
                     text="Доброго дня, {0.first_name}! Я тестовий бот відділу паркування. Я спробую Вам допомогти."
                     .format(message.from_user), reply_markup=markup)


@bot.message_handler(content_types=['text'])
def func(message):
    if message.text == "Я загубив повідопмлення":
        bot.send_message(message.chat.id,
                         text="Зв'яжіться із відділом паркування за телефоном (0432)57-53-16 з 8:30 до 17:00 годин")
        bot.send_message(message.chat.id,
                         text="Вкажіть серію та номер вашого ТЗ, Ви отримаєте інформацію ")
        bot.send_message(message.chat.id,
                         text="стосовно номеру Вашого повідомлення/постанови.")

    elif message.text == "Мені не відомо про наявність штрафу":
        bot.send_message(message.chat.id,
                         text="Зв'яжіться із відділом паркування за телефоном (0432)57-53-16 з 8:30 до 17:00 години, вказавши серію та номер вашого ТЗ.")
        bot.send_message(message.chat.id,
                         text="У разі наявності штрафу, працівник відділу паркування надасть інформацію: ")
        bot.send_message(message.chat.id,
                         text=" Номер повідомлення/постанови, спосіб облати, та наявність дії пільгового періоду.")

    elif message.text == "Мене було оштрафовано":
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True, row_width=2)
        btn1 = types.KeyboardButton("Так")
        btn2 = types.KeyboardButton("Ні")
        back = types.KeyboardButton("Повернутися в головне меню")
        markup.add(btn1, btn2, back)
        bot.send_message(message.chat.id, text="Чи сплачено штраф?", reply_markup=markup)

    elif message.text == "Так":
        keyboard = types.InlineKeyboardMarkup()
        url_button = types.InlineKeyboardButton(text="Перевірити зарахування коштів",
                                                url="https://inspector.vmr.gov.ua/SitePages/fines.aspx")
        keyboard.add(url_button)
        bot.send_message(message.chat.id,
                         text="Для перевірки зарахування коштів, перейдіть за посиланням та "
                              "заповніть відповідні поля виключно українськими літерами.",
                         reply_markup=keyboard)

    elif message.text == "Ні":
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True, row_width=2)
        btn1 = types.KeyboardButton("Перевірити фотодоказову базу порушення")
        btn2 = types.KeyboardButton("Сплатити онлайн")
        back = types.KeyboardButton("Повернутися в головне меню")
        markup.add(btn1, btn2, back)
        bot.send_message(message.chat.id, text="Оберіть потрібну Вам дію.", reply_markup=markup)

    elif message.text == "Перевірити фотодоказову базу порушення":
        keyboard = types.InlineKeyboardMarkup()
        url_button = types.InlineKeyboardButton(text="Перевірити фотодоказову базу",
                                                url="https://inspector.vmr.gov.ua/SitePages/fines.aspx")
        keyboard.add(url_button)
        bot.send_message(message.chat.id,
                         text="Для перевірки фотодоказової бази, перейдіть за посиланням та "
                              "заповніть відповідні поля виключно українськими літерами.",
                         reply_markup=keyboard)

    elif message.text == "Сплатити онлайн":
        keyboard2 = types.InlineKeyboardMarkup()
        url_button = types.InlineKeyboardButton(text="Перейти до оплати",
                                                url="https://inspector.vmr.gov.ua/SitePages/payment.aspx")
        keyboard2.add(url_button)
        bot.send_message(message.chat.id,
                         text="Для оплати онлайн, перейдіть за посиланням оберіть ресурс "
                              "оплати та заповніть відповідні поля.",
                         reply_markup=keyboard2)

    elif message.text == "Повернутися в головне меню":
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True, row_width=2)
        button1 = types.KeyboardButton("Мене було оштрафовано")
        button2 = types.KeyboardButton("Я загубив повідомлення")
        button3 = types.KeyboardButton("Мені не відомо про наявність штрафу")
        markup.add(button1, button2, button3)
        bot.send_message(message.chat.id, text="Ви повернулись до головного меню.", reply_markup=markup)

    else:
        bot.send_message(message.chat.id, text="Зв'яжіться із відділом паркування за телефоном (0432)57-53-16")


bot.polling(none_stop=True)
