pip install SpeechRecognition pyttsx3 gTTS
import speech_recognition as sr
import pyttsx3

# Инициализация распознавателя речи и синтезатора
recognizer = sr.Recognizer()
engine = pyttsx3.init()

def speak(text):
    engine.say(text)
    engine.runAndWait()

def listen():
    with sr.Microphone() as source:
        print("Слушаю...")
        audio = recognizer.listen(source)
        try:
            text = recognizer.recognize_google(audio, language='ru-RU')
            print(f"Вы сказали: {text}")
            return text
        except sr.UnknownValueError:
            print("Не удалось распознать речь.")
            return None
        except sr.RequestError:
            print("Ошибка запроса к сервису распознавания.")
            return None

def main():
    speak("Привет! Чем могу помочь?")
    while True:
        command = listen()
        if command:
            if "время" in command:
                from datetime import datetime
                now = datetime.now().strftime("%H:%M:%S")
                speak(f"Текущее время: {now}")
            elif "пока" in command:
                speak("До свидания!")
                break
            else:
                speak("Извините, я не понял команду.")

if __name__ == "__main__":
    main()
