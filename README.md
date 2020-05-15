# Voice-assistant
# Made using python speech recognition tool
from tkinter import *
import os
import cv2 
import roman
import PIL.Image, PIL.ImageTk
from PIL import Image
import datetime
from time import ctime
import playsound
import random
import wikipedia
import speech_recognition as sr
import webbrowser
import pyaudio
import wolframalpha
from gtts import gTTS 
import requests , json
import pytesseract
import pyttsx3
from bs4 import BeautifulSoup
import pyjokes



windows = Tk()


global var
global var1

var = StringVar()
var1 = StringVar()

def speak(audio_string):
    tts = gTTS(text=audio_string , lang = 'en')
    audio_file = 'audio-' +  '.mp3'
    tts.save(audio_file)
    playsound.playsound(audio_file)
    print(audio_string )
    os.remove(audio_file)

   

def wishme():
     hour = int(datetime.datetime.now().hour)
     if hour>=0 and hour<12:
        var.set("Good morning Sushma! What a sunny day ")
        windows.update()
        speak(" Good Morning Sushma! What a sunny day") 

     elif hour>=12 and hour<16:
        var.set("Good Afternoon Sushma!")
        windows.update()
        speak("Good Afternoon Sushma!")

     else:
        var.set("Good Evening Sushma!")
        windows.update()
        speak("Good Evening Sushma!")
     
class student:
   def __init__(self,name,course,rollno):
     self.course= course 
     self.name = name 
     self.rollno = rollno 
   def get_student_details(self):
     print("Your name is:" + self.name + ".")
     print("Your course is:" + self.course)
     print("Your rollno is:"+ self.rollno)




def listencommand(ask = False):
    r = sr.Recognizer()
    with sr.Microphone() as source:
        speak("How may i help you")
        var.set("Listening...")
        windows.update()
        print("Listening..")
        r.pause_threshold = 1.5
        r.energy_threshold = 400
        audio = r.listen(source)
      
    try:
         var.set("Recognizing...")
         windows.update()
         print("Recognizing.....")
         input = r.recognize_google(audio , language='en-in')
         print(f"User said: {input}\n")

    except Exception as e:
          #print(e)   
        speak("Sorry I did not get that, Say that again please..")
        return "None"
    var1.set(input)
    windows.update()
    return input





def play():
   btn2['state'] = 'disabled'
   btn0['state'] = 'disabled'
   btn1.configure(bg = 'orange')
   wishme()
   while True:
       btn1.configure(bg = 'orange')
       input= listencommand().lower() 
       
       if ('exit') in input:
          var.set("Goodbye ! Have a nice day")
          btn1.configure(bg = '#5C85FB')
          btn2['state'] = 'normal'
          btn0['state'] = 'normal'
          windows.update()
          speak ("Goodbye! Have a nice day ")
          break

       elif ('hello') in input:
          var.set('Hello !')
          windows.update()
          speak('Hello !')

       elif ('what is your name') in input or ('who are you?') in input:
          var.set("Hello ! I am a voice bot named Nory." )
          windows.update()
          speak("Hello ,I am a voice bot named Nory "
          "I am here to make your life easier. You can command me to perform various tasks like "
          "calculations , opening applications etcetra ")
                

       elif ('do you feel') in input:
          var.set('No I am a voice bot,I do not feel')
          windows.update()
          speak('No  I am a Voicebot, I do not not feel') 
               

       elif ('which language do you speak') in input:
          var.set('I speak in English')
          windows.update()
          speak('I speak in English ')  
               

       elif ('when is my birthday') in input:
          var.set('Your birthday is on 30 July')
          windows.update()
          speak(' Your birthday is on 30 July')
                

       elif ('what is the date and time ') in input or (' date and time') in input:
          var.set(ctime()) 
          windows.update()
          speak( 'The date and time is ' + ctime())
                

       elif ('who am i') in input:
          var.set('You are a humanbeing classified as homo sapiens according to nomenclature')
          windows.update()
          speak('you are a human being classified as homo sapiens according to nomenclature') 
                
       elif('jokes') in input:
          print(pyjokes.get_joke())
          var.set(pyjokes.get_joke())
          windows.update()
          speak('here is a joke for you try and control your laughter')

       elif('game') in input:
          var.set("Opening tic-tac-toe.Enjoy!")
          windows.update()
          speak("opening tictactoe enjoy") 
          codepath = "H:\\python vs\\dist\\tic_tac_toe\\tic_tac_toe.exe" 
          os.startfile(codepath) 
          

       elif ('calculate') in input or ('find the value of') in input:
          app_id = "8QW392-UJYVJWUWX9"
          client = wolframalpha.Client(app_id)
          indx = input.lower().split().index('calculate')
          query= input.split()[indx + 1:]
          res = client.query(' '.join(query))
          answer = next(res.results).text
          var.set(answer)
          windows.update()
          speak("The answer to the question is" + answer)
          print(answer)

       elif ('news') in input:
          res = requests.get('https://timesofindia.indiatimes.com')
          soup = BeautifulSoup(res.text , 'lxml')
          news_box = soup.find('ul' ,{'class' : 'list9'})
          all_news = news_box.find_all('a')
          for news in all_news:
             print(news.text)
             speak("todays headline is :" + news.text)
          windows.update()
            
                  

       elif ('current weather') in input:
          api_key = "b11dd6fcfbefe8d4f60486bb86ec1449"
          base_url = "http://api.openweathermap.org/data/2.5/weather?"
          city_name = "kanpur"
          complete_url = base_url + "appid=" + api_key + "&q=" + city_name
          response = requests.get(complete_url)
          x = response.json()
          if x["cod"] != "404":
                y = x["main"]
                current_temperature = y["temp"]
                current_pressure = y["pressure"]
                current_humidity = y["humidity"]
                z = x["weather"]
                weather_description = z[0]["description"]
                var.set ("Temperature (in kelvin unit) =" + str(current_temperature) + "\n atmospheric pressure(in hPa unit =" +  str(current_pressure) + "\n humidity (in  percentage =" + str(current_humidity) + "\n description =" + str(weather_description))
                windows.update()
                speak("this is  the current temperature , pressure, humidity and description of kanpur")
          else:
                print("City not found")
             
       elif ("chrome") in input:
          var.set("Opening google chrome..")
          windows.update()
          speak(" Opening Google Chrome")
          os.startfile("C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe")
           
       elif('send mail') in input:
          var.set("Compose Mail")
          windows.update()
          speak("you can compose you mail here")
          url = "https://mail.google.com/mail/u/0/#inbox?compose=new"
          webbrowser.get().open(url)
         

       elif ("firefox") in input:
          var.set("Opening Mozilla firefox...")
          windows.update()
          speak(" Opening Mozilla Firefox")
          os.startfile("C:\\Program Files (x86)\\Mozilla Firefox\\firefox.exe")
          

       elif ('play music') in input:
           var.set("Here are your favourites.Enjoy!")
           windows.update()
           speak('Here are your favorites. enjoy')
           music_dir ="H:\\music"
           songs = os.listdir(music_dir)
           n = random.randint(0,5)
           print(songs)
           os.startfile(os.path.join(music_dir , songs[n]))  
            
   
       elif ('code') in input:
          var.set("Opening Visual Studio code.....")
          windows.update()
          speak("Opening visual studio code..")
          codepath ="C:\\Users\\hp\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe" 
          os.startfile(codepath) 
          
       elif ('notepad') in input:
          var.set("Opening notepad...")
          windows.update()
          speak("Opening notepad")
          codepath="H:\\python vs\\dist\\sushma_notepad\\sushma_notepad.exe"
          os.startfile(codepath)

       elif ('shareit') in input:
          var.set("Opening Shareit....")
          windows.update()
          speak("Opening Shareit..")
          codepath = "C:\\Program Files (x86)\\SHAREit Technologies\\SHAREit\\SHAREit.exe"
          os.startfile(codepath)
          

       elif ('youtube') in input:
          video = listencommand("which video i should open?")
          url = 'http://youtube.com/search?q=' + video
          var.set("Opening in youtube")
          windows.update()
          speak("Opening  in Youtube...")
          webbrowser.get().open(url)
          speak('This is the video i found '+ video)
          
          

       elif ('wikipedia') in input:
          speak('Searching wikipedia....')
          input = input.replace("wikipedia", "")
          results = wikipedia.summary(input, sentences = 2) 
          speak("According to wikipedia...")
          var.set(results)
          windows.update()
          speak(results)
      

       elif ('search') in input:
          speak("what do you want me to search for")
          search = listencommand("What do you want me to search for?")
          url = 'http://google.com/search?q=' + search
          windows.update()
          speak('Searching google...')
          webbrowser.get().open(url)
          windows.update()
          speak('here is what i found for your input that is' + search)
          
              
       elif ('open stackoverflow') in input:
          var.set("Opening Stack overflow..")
          windows.update()
          speak("Opening stackoverflow...")
          url = 'http://www.stackoverflow.com'
          webbrowser.get().open(url)    

       elif ('find location') in input:
          var.set("What is the location ?")
          location = listencommand('what is your location?')
          url = 'http://www.google.com/maps/place/' + location
          speak("searching google maps..") 
          webbrowser.get().open(url)
          speak('here is the location of' + location)
          windows.update()

       elif ('click photo') in input:
          stream=cv2.VideoCapture(0)
          grabbed,frame = stream.read()
          if grabbed:
             cv2.imshow('pic', frame)
             cv2.imwrite('pic.jpg', frame)
             stream.release()

       elif ('record video') in input:
          cap= cv2.VideoCapture(0)
          out = cv2.VideoWriter('output.avi' , -1, 20.0,(640,480))
          while(cap.isOpened()):
             ret, frame = cap.read()
             if ret:
                out.write(frame)
                cv2.imshow('frame', frame)
                if cv2.waitKey(1) & OxFF == ord('q'):
                   break
                else:
                   break
          cap.release()
          out.release()
          cv2.destroyAllWindows() 

       

      
def update(ind):
   frame = frames[(ind)%100]
   ind += 1
   label.configure(image = frame)
   windows.after(100 , update , ind)

label2 = Label(windows , textvariable = var1 , bg = '#FAB60C')
label2.config(font =("Cambria" , 20))
var1.set('NORY')
label2.pack()

label1 = Label(windows, textvariable = var , bg = '#ADD8E6')
label1.config(font =("SimSun" , 20))
var.set('Welcome Sushma!')
label1.pack()

frames = [PhotoImage(file ='C:/Users/hp/Desktop/Assistant.gif' , format = 'gif -index %i' %(i)) for i in range(100)]
windows.title('NORY')

label = Label(windows, width = 600 , height = 600)
label.pack()
windows.after(0,update,0)

btn0 = Button(text = 'WISH ME' , width =20, command = wishme , bg = '#5C85FB')
btn0.config(font = ("Lucida Handwriting" , 12))
btn0.pack()
btn1= Button(text = 'PLAY' , width = 20 , command = play , bg = '#5C85FB')
btn1.config(font = ("Lucida Handwriting", 12))
btn1.pack()
btn2=Button(text = 'EXIT', width = 20 , command = windows.destroy, bg ='#5C85FB')
btn2.config(font = ("Lucida Handwriting" , 12))
btn2.pack()

windows.mainloop()
    
