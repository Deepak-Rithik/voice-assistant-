import speech_recognition as sr
import pyttsx3
import datetime
import webbrowser

# Initialize text-to-speech engine
engine = pyttsx3.init()

def speak(text):
    print("Assistant:", text)
    engine.say(text)
    engine.runAndWait()

def listen():
    recognizer = sr.Recognizer()

    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source)

        try:
            audio = recognizer.listen(source, timeout=5)
            command = recognizer.recognize_google(audio)
            print("You:", command)
            return command.lower()

        except sr.UnknownValueError:
            speak("Sorry, I didn't understand. Please repeat.")
            return ""

        except sr.RequestError:
            speak("Network error occurred.")
            return ""

        except Exception:
            speak("Something went wrong.")
            return ""

def tell_time():
    current_time = datetime.datetime.now().strftime("%I:%M %p")
    speak(f"The current time is {current_time}")

def tell_date():
    today = datetime.datetime.now().strftime("%d %B %Y")
    speak(f"Today's date is {today}")

def search_google(query):
    url = f"https://www.google.com/search?q={query}"
    webbrowser.open(url)
    speak(f"Searching Google for {query}")

def open_website(site):
    webbrowser.open(site)
    speak("Opening website")

def process_command(command):

    if "hello" in command:
        speak("Hello! How can I help you?")

    elif "time" in command:
        tell_time()

    elif "date" in command:
        tell_date()

    elif "open google" in command:
        open_website("https://www.google.com")

    elif "open youtube" in command:
        open_website("https://www.youtube.com")

    elif "open github" in command:
        open_website("https://github.com")

    elif "search" in command:
        query = command.replace("search", "")
        search_google(query)

    elif "exit" in command or "stop" in command:
        speak("Goodbye!")
        return False

    else:
        speak("Sorry, I don't know that command.")

    return True

def main():

    speak("Voice Assistant Started")

    running = True

    while running:
        command = listen()

        if command:
            running = process_command(command)

if __name__ == "__main__":
    main()