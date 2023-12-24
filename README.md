
Update
v3.5 Now it also supports for Twitch Streamer

v3.0 Now not only supports Japanese TTS using VoiceVox. But also supports TTS for RU (Russian), EN (English), DE (German), ES (Spanish), FR (French), TT (Tatar), UA (Ukrainian), UZ (Uzbek), XAL (Kalmyk), Indic (Hindi), using Seliro TTS. Change voicevox_tts on run.py to seliro_tts, for detailed information of how to use Seliro TTS

AI Waifu Vtuber & Assistant
This project is inspired by shioridotdev and utilizes various technologies such as VoiceVox Engine, DeepL, Whisper OpenAI, Seliro TTS and VtubeStudio to create an AI waifu virtual YouTuber
Technologies Used:
VoiceVox Docker or 
Unsupported image
DeepL
Deeplx
Whisper OpenAI
Seliro TTS
VB-Cable
VtubeStudio
Installation
Install the dependencies:
pip install -r requirements.txt

Create config.py and store your Openai API key
api_key = 'yourapikey'

Change the owner name
owner_name = "Prakhar"

if you want to use it for livestream, create a list of users that you want to blacklist on run.py

blacklist = ["Nightbot", "streamelements"]

Change the lore or identity of your assistant. Change the txt file at characterConfig\Pina\identity.txt

If you want to stream on Twitch you need to change the config file at utils/twitch_config.py. Get your token from Here. Your token should look something like oauth:43rip6j6fgio8n5xly1oum1lph8ikl1 (fake for this tutorial). After you change the config file, you can start the program using Mode - 3

server = 'irc.chat.twitch.tv'
port = 6667
nickname = 'testing' # You don't need to change this
token = 'oauth:43rip6j6fgio8n5xly1oum1lph8ikl1' # get it from https://twitchapps.com/tmi/.
user = 'ardha27' # Your Twitch username
channel = '#aikohound' # The channel you want to retrieve messages from

Choose which TTS you want to use, VoiceVox or Silero. Uncomment and Comment to switch between them
# Choose between the available TTS engines
# Japanese TTS
voicevox_tts(tts)
# Silero TTS, Silero TTS can generate English, Russian, French, Hindi, Spanish, German, etc. Uncomment the line below. Make sure the input is in that language
# silero_tts(tts_en, "en", "v3_en", "en_21")

If you want to use VoiceVox, you need to run VoiceVox Engine first. You can run them on local using VoiceVox Docker or on Google Colab using VoiceVox Colab. If you use the Colab one, change voicevox_url on utils\TTS.py using the link you get from Colab.

voicevox_url = 'http://localhost:50021'

if you want to see the voice list of VoiceVox you can check this VoiceVox and see the speaker id on speaker.json then change it on utils/TTS.py. For Seliro Voice sample you can check this Seliro Samples

Choose which translator you want to use depends on your use case (optional if you need translation for the answers). Choose between google translate or deeplx. You need to convert the answer to Japanese if you want to use VoiceVox, because VoiceVox only accepts input in Japanese. The language answer from OpenAI will depens on your assistant lore language characterConfig\Pina\identity.txt and the input language
tts = translate_deeplx(text, f"{detect}", "JA")
tts = translate_google(text, f"{detect}", "JA")

DeepLx is free version of DeepL (No API Key Required). You can run Deeplx on docker, or if you want to use the normal version of deepl, you can make the function on utils\translate.py. I use DeepLx because i can't register on DeepL from my country. The translate result from DeepL is more accurate and casual than Google Translate. But if you want the simple way, just use Google Translate.

If you want to use the audio output from the program as an input for your Vtubestudio. You will need to capture your desktop audio using Virtual Cable and use it as input on VtubeStudio microphone.

If you planning to use this program for live streaming Use chat.txt and output.txt as an input on OBS Text for Realtime Caption/Subtitles

FAQ
Error Transcribing Audio
def transcribe_audio(file):
    global chat_now
    try:
        audio_file= open(file, "rb")
        # Translating the audio to English
        # transcript = openai.Audio.translate("whisper-1", audio_file)
        # Transcribe the audio to detected language
        transcript = openai.Audio.transcribe("whisper-1", audio_file)
        chat_now = transcript.text
        print ("Question: " + chat_now)
    except:
        print("Error transcribing audio")
        return
    result = owner_name + " said " + chat_now
    conversation.append({'role': 'user', 'content': result})
    openai_answer()

Change this Line of Code to this. This will help you to get more information about the error

def transcribe_audio(file):
    global chat_now
    audio_file= open(file, "rb")
    # Translating the audio to English
    # transcript = openai.Audio.translate("whisper-1", audio_file)
    # Transcribe the audio to detected language
    transcript = openai.Audio.transcribe("whisper-1", audio_file)
    chat_now = transcript.text
    print ("Question: " + chat_now)
    result = owner_name + " said " + chat_now
    conversation.append({'role': 'user', 'content': result})
    openai_answer()

Another option to solve this problem, you can upgrade the OpenAI library to the latest version. Make sure the program capture your voice/sentence, try to hear the input.wav

Mecab Error
this library is a little bit tricky to install. If you facing this problem, you can just delete and don't use the katakana_converter on utils/TTS.py. That function is optional, you can run the program without it. Delete this two line on utils/TTS.py

from utils.katakana import *
katakana_text = katakana_converter(tts)

and just pass the tts to next line of the code

params_encoded = urllib.parse.urlencode({'text': tts, 'speaker': 46})

Credits
This project is inspired by the work of shioridotdev. Special thanks to the creators of the technologies used in this project including VoiceVox Engine, DeepL, Whisper OpenAI, and VtubeStudio.

Console

Results of your code will appear here when you 
Run

Open in Editor

Close Tab
Replit is out of sync!

This might disrupt other people's work or create unexpected bugs.

Reset to Mine

This is the safest option and won't disrupt other's work.


#Remember to unzip :
.cache.zip
.git.zip
.upm.zip
characterconfig.zip
utils.zip
