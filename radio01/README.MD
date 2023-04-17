# CYBER ARENA 4.0 CHALLENGE
## Inteligencia de señales - radio01
### nocnoc37
##### Todos los archivos necesarios para el reto son accesibles aquí --> [ARCHIVOS](WWW.ARCHIVOS)
###### noise filtering - modulation scheme - signal demodulation - signal decoding - binary/ascii
---
### CONTENT
---  
THE CHALLENGE BEGS THE FOLLOWING QUESTION:  

"***THIS SIGNAL HAS BEEN INTERCEPTED AND IT IS BELIEVED THAT ENCODED INFORMATION IS BEING SENT TO PREVENT IT FROM BEING ANALYZED BY THIRD PARTIES. CAN YOU FIND OUT WHAT IT IS?***"  
<br>
Unzipping the file we are given, (```unzip radio01```) outputs the following:  
-Furthermore we can see that it is a .raw file (basic raw data)  
-Binwalk doesn't retrieve any valuable information:

![screenshot showing various command line inputs: 1st unzip radio01 2nd ls: radio01 ](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/content-initial-approach.png?raw=true)  

<br>

For those of you who have already played with SDR devices, you may have come across various programms to annalyze signals.  
Feel free to chose your own, but for the purpose of this challenge I will show you how to solve it with [UniversaRadioHacker](https://github.com/jopohl/urh) <-- Link to the repository for easy install.  

---   
Once in URH, open signals.raw  
Now its time to guess the modulation scheme. There are 3 types: Amplitude, Frecuency, and Phase modulation. 
They are easily recognizable. Hereby I give you the scheme to recognize modulation schemes and a wonderful post to a deeper understanding.  

---
![modulation scheme](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/modulation-scheme.jpg?raw=true)
