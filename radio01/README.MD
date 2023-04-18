# CYBER ARENA 4.0 CHALLENGE
## SIGNALS INTELLIGENCE - radio01
### nocnoc37
##### ALL THE FILES REQUIRED FOR THE CHALLENGE CAN BE ACCESSED HERE --> [FILES](https://github.com/1ocho3/CyberArena4.0/tree/main/radio01/CTF-Files)
###### noise filtering - modulation scheme - signal demodulation - signal decoding - binary/ascii
---
### CONTENT
---  
THE CHALLENGE BEGS THE FOLLOWING QUESTION:  

"***THIS SIGNAL HAS BEEN INTERCEPTED AND IT IS BELIEVED THAT ENCODED INFORMATION IS BEING SENT TO PREVENT IT FROM BEING ANALYZED BY THIRD PARTIES. CAN YOU FIND OUT WHAT IT IS?***"  
<br>
Unzipping the file we are given, (```$ unzip radio01```) outputs the following:  
-Furthermore we can see that it is a .raw file (basic raw data)  

![screenshot showing various command line inputs: 1st unzip radio01 2nd ls: radio01 ](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/content-initial-approach-terminal.png?raw=true)  

For those of you who have already played with SDR devices, you may have come across various programms to annalyze signals.  
Feel free to chose your own, but for the purpose of this challenge I will show you how to solve it with [UniversaRadioHacker](https://github.com/jopohl/urh) <-- Link to the repository for easy install.  

---   
Once in URH, open signals.raw and zoom-in on one of the packets.  

![signal1](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/signal1.png?raw=true)  
![signal1zoomed](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/signal1zoomed.png?raw=true)

Now its time to guess the modulation scheme. There are 3 types: Amplitude, Frecuency, and Phase modulation. 
They are easily recognizable. Hereby I hand the diagram to recognize modulation schemes, and a wonderful post to a deeper understanding.  

---
![modulation scheme](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/modulation-scheme.jpg?raw=true)  
[Wonderful post about modulation](https://documentation.meraki.com/MR/Wi-Fi_Basics_and_Best_Practices/Wireless_Fundamentals%3A_Modulation)  

---

Now that you understand what types of modulations there are, you can guess this is an analog signal in frequency-shift keying (FSK) or as is commonly known as, "frecuency modulated", where the change in frecuency is interpreted as ones and zeros (0/1).

---

Knowing this, hop onto urh, and set modulation to FSK and hit AUTODETECT PARAMETERS.  
The signal is so clear that we won't need a bandpass filter.
After doing this, set "Signal view" to demodulated, as URH will interpret the ones and zeros and give you the following view:  

![signal1 demodulated view](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/signal1%20demodulated%20view.png?raw=true)
  
---
The signal can be divided into to portions, a repeating synchronization pattern, and the actual message.
In our case the synchronization pattern is no other than a string of 01 pairs repeated over and over again until a point where two zeros meet.  

![singal1-separated](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/signal1-message-separated.png?raw=true)  

---
Manchester encoding works by translating a HIGH to LOW transitions as a zeros, and a LOW to HIGH as a ones:  

![manchester encoding scheme](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/manchester-encoding-scheme.png?raw=true)

Therefore, looking at our first message `01101001011010010110100...` and to the image above we see that their structures are very similar.  
Let's try and decode the message, we will use an online tool as it is as efective as URH and is easier for begginers.  

![manchester-decoder-online](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/manchester-decoded.png?raw=true)  

---
[Machester Decoder Online](https://eleif.net/manchester.html)  

Introducing our raw bits:  `0110100101101001011010011010010101101001010101100110100101101010011010101001101001101010011001010110101001011001011010101001011001100110101010100110100101010110011010010110101001101001010101100110100110010110011010011010100101101010101001100`  

Returns the following __binary__ string:  
`011001100110110001100001011001110111101101110100011100100111100101011111011000010110011101100001011010010110111001111101`  

---
To turn binary into plaintext, you can use one of the best kwonk decoding/decyphering online tools out there: [CyberChef](https://gchq.github.io/CyberChef/)  
The output is a plain text string. Try the same process with the other transmissions to find the flag.

![nottheflag](https://github.com/1ocho3/CyberArena4.0/blob/main/radio01/readme-required/not-the-flag-keep-trying.png?raw=true)
