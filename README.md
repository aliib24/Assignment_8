# Assignment_8

# code part

from email import message
from gc import callbacks
from itertools import count
from multiprocessing.connection import Client
import telebot
import qrcode
from gtts import gTTS
import random
from telebot.types import Message
from telebot import types


bot = telebot.TeleBot("5167810612:AAHg9Ff6bEQN8x_AsygOOPqGBGMocSbkCwA", parse_mode=None)



# Welcome message
@bot.message_handler(commands=['start'])
def great(message):
    user_first_name = str(message.chat.first_name)
    bot.reply_to(message, f"Hello {user_first_name} \n Welcome to my humble bot")

# Help message
@bot.message_handler(commands=['help'])
def send_welcome(message):
    bot.reply_to(message, "\n For playing game click on Game button"
                          "\n for calculating your age click on Age button"
                          "\n for convering a string into an voice click on voice button"
                          "\n for highest array value click on Max button"
                          "\n for arguments of the maxima in array click on Argmax button"
                          "\n for generating QRcode from a string click on QRcode button")

# age 
@bot.message_handler(commands=['age'])
def age_calculator(message):
    u_name = message.from_user.username
    bot.reply_to(message, f"please enter your age user {u_name}")
    @bot.message_handler(func=lambda message:True)
    def print_age(message):
        age = 1401 - int(message.text)
        bot.reply_to(message, f"{u_name} you are currently in {age}")

# qrcode generator
@bot.message_handler(commands=['qrcode'])
def get_qr_string(message):
    u_name = message.from_user.username
    bot.reply_to(message, f"{u_name} please enter an string to generate it into a qr code")
    @bot.message_handler(func=lambda message:True)
    def generate_qr(message):
        i_str = str(message.text)
        img = qrcode.make(i_str).save("myqr.png")
        photo = open('myqr.png','rb')
        bot.send_photo(message.chat.id, photo)
        
# voice generator
@bot.message_handler(commands=['voice'])
def get_voice_string(message):
    u_name = message.from_user.username
    bot.reply_to(message, f"{u_name} Please enter an string to generate it into a voice")
    @bot.message_handler(func=lambda message:True)
    def generate_voice(message):
        i_str = str(message.text)
        voice = gTTS(text=i_str, lang='en', slow=False)
        voice.save("my_voice.mp3")
        g_voice = open('my_voice.mp3','rb')
        bot.send_audio(message.chat.id, g_voice)

# game
@bot.message_handler(commands=['game'])
def game_input_number(message):
    u_name = message.from_user.username
    bot.reply_to(message, f"{u_name} Please enter a number from 1 to 10")
    number = random.randint(1 , 10)
    @bot.message_handler(func=lambda message:True)
    def game(message):
        for i in range(3):
            x = int(message.text)

            if x == number:
                bot.reply_to(message, "ok")
                i += 1

            elif x > number:
                bot.reply_to(message, "high")
                i += 1

            elif x < number:
                bot.reply_to(message, "low")
                i += 1
            
            else:
                bot.reply_to(message, "you failed")

#argmax
@bot.message_handler(commands=['argmax'])
def max_index(message):
    bot.reply_to(message, "the array indexes are: [14,7,78,15,8,19,20]")
    number_list = [14,7,78,15,8,19,20]
    max_value = max(number_list)
    max_index = number_list.index(max_value)
    bot.reply_to(message, max_index)

#max
@bot.message_handler(commands=['max'])
def max_index_array(message):
    bot.reply_to(message, "the array indexes are: [14,7,78,15,8,19,20]")
    number_list = [14,7,78,15,8,19,20]
    max_value = max(number_list)
    bot.reply_to(message, max_value)

# Running
bot.infinity_polling()

# final result in telegram
<img width="1440" alt="Screen Shot 2022-05-12 at 3 36 50 PM" src="https://user-images.githubusercontent.com/89723046/168060540-568fd471-3fc0-4ecc-ac62-c984b9a6c444.png">
