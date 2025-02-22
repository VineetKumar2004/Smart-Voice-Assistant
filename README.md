# Smart-Voice-Assistant
**Clone the repository:**
   git clone https://github.com/VineetKumar2004/Smart-Voice-Assistant.git
   cd Smart-Voice-Assistant


   
import pyttsx3
import speech_recognition as sr
import random
import webbrowser
import datetime
from plyer import notification
import pyautogui
import wikipedia
import pywhatkit as pwk
import os
import requests
from googletrans import Translator
import logging





engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)
engine.setProperty('rate', 150)
engine.say('Hello , i will speak this text')    
engine.runAndWait()

def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def command():
    content=" "
    while content==" ":
    #obtain audio from the microphone
        r=sr.Recognizer()
        with sr.Microphone() as source:
            print('Say Something')
            audio = r.listen(source)
            
        try:
            content = r.recognize_google(audio,language='en-in')
            print('Your command is ........')
            print("command = " + content)
        except Exception as e:
            print('Please try again...........') 
            # content= None  
        #         
    return content

def main_process():
        while True:
            request=command().lower()
            if "hello" in request:
                speak("Welcome,How can i help you.")
            elif "play music" in request:
                speak("Playing Music")
                webbrowser.open("https://www.youtube.com/live/ugeRr5HbsU4?si=zpoH6K5O9okoRoIa")

 
            elif "play sad song" in request:
                speak("Playing sad song")
                webbrowser.open("https://youtu.be/i61nN7hcbPA?si=Sd_5HCRfSNR1WqBm")

            elif "play old song" in request:
                speak("playing old song")
                webbrowser.open("https://youtu.be/sCIiVN0zIGE?si=ZMSs4zeBiNlpd1fv")

            elif "play english song" in request:
                speak("playing old song")
                webbrowser.open("https://youtu.be/U0ZoqmyGJo8?si=mceWiF30-8pkZHVC")
    


            elif "time" in request:
                now_time = datetime.datetime.now().strftime("%d:%n")
                speak("current date is"+str(now_time))
            elif "say date" in request:
                now_time = datetime.datetime.now().strftime("%d:%m")
                speak("Current date is "+str(now_time))  


            elif "new task" in request:
                task = request.replace("new task","")
                task = task.strip()
                if task !="":
                    speak("Adding task:" + task)
                    with open("todo.txt","a") as file:
                        file.write(task + "\n") 
            elif "speak task" in request:
                with open("todo.txt","r") as file:
                    speak("work we have to do today is: "+ file.read())   
            elif "show work" in request:
                with open ("todo.txt","r") as file:
                    tasks=file.read()
                notification.notify(
                    title = " Today's work ",
                    message = tasks
                )   


            elif "open youtube" in request:
                webbrowser.open("www.youtube.com")
            elif "close youtube" in request:
             os.system("taskkill /im chrome.exe /f")  
            elif "open twitter" in request:
                webbrowser.open("www.twitter.com")
            elif "open linkden" in request:
                webbrowser.open("www.linkden.com")
            elif "close linkedin" in request:
                os.system("taskkill /im chrome.exe /f")
            elif "open instagram" in request:
                webbrowser.open("www.instagram.com")
            elif "close instagram" in request:
                os.system("taskkill /im chrome.exe /f")    
            elif "open facebook" in request:
                webbrowser.open("www.facebook.com")
            elif "close facebook" in request:
                os.system("taskkill /im chrome.exe /f")
            elif "open chat GPT" in request:
                webbrowser.open("www.chatgpt.com")   
            elif "close chat GPT" in request:
                os.system("taskkill /im chrome.exe /f")
            elif "open google" in request:
                webbrowser.open("www.google.com")     
            elif "close google" in request:
                os.system("taskkill /im chrome.exe /f")

                
            
            
            elif "open" in request:
                query=request.replace("open", "")
                pyautogui.press("super")
                pyautogui.typewrite(query)
                pyautogui.sleep(2)    
                pyautogui.press("enter")      


            elif "wikipedia" in request:
                request = request.replace("jarvis","")
                request = request.replace("search wikipedia", "")
                print(request)                                #we can remove this request also
                result=wikipedia.summary(request,sentences=2)
                print(result)
                speak(result)
            

            elif "search google" in request:
                request = request.replace("jarvis","")
                request = request.replace("search google", "")
                webbrowser.open("https://www.google.com/searching="+request)


            elif "send whatsapp" in request:
                pwk.sendwhatmsg("+91 7071790480","Hii,How are you",10,2,30 )
                request = request.replace("jarvis","")
                request = request.replace("search google", "")
                webbrowser.open("https://www.google.com/searching="+request)


            elif " today news" in request:
             api_key = "pub_68100a3f50455c22b28aad57d82300e7095e6"
             base_url = "https://newsapi.org/v2/top-headlines?country=us&apiKey=" + api_key
             response = requests.get(base_url)
             data = response.json()
             articles = data["articles"]
             for article in articles[:5]:
                speak(article["title"])
            
            elif "translate" in request:
               translator = Translator()
               text_to_translate = request.replace("translate ", "")
               translated_text = translator.translate(text_to_translate, dest='es').text
               speak(f"The translation is: {translated_text}")
            

            elif "weather" in request:
              city = "vizianagaram,andhra pradesh"
              client_id = "your_client_id"
              client_secret = "your_client_secret"
              base_url = f"https://data.api.xweather.com/conditions/{city}?format=json&plimit=1&filter=1min&client_id={client_id}&client_secret={client_secret}"
              response = requests.get(base_url)
              data = response.json()
              if "conditions" in data:
                condition = data["conditions"][0]
                temperature = condition["temperature"]
                description = condition["description"]
                speak(f"The current temperature in {city} is {temperature} degrees Celsius with {description}.")
            

            elif "exit" in request:
              speak("Thank you, Have a nice day")
            break
        else:
            speak("Sorry, I am not able to understand your command")
            speak("Please try again")

        


main_process()           

