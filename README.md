# Pi-FM-RDS with live microphone input

## Adding a live microphone to the original project

This guide will help you add live microphone capabilities to the original Pi-FM-RDS project. It works by making the Pi listen for raw data through netcat, and streaming the audio from a windows machine with ffmpeg.

**PiFmRds** has been developed for experimentation purposes only. It is not a media center and is not intended to broadcast music to your stereo system. For more details, please refer to the [legal warning](#warning-and-disclaimer).

## How to Use It?

1. **Set it up**  
   Follow the instructions provided in [ChristopheJacquet's repository](https://github.com/ChristopheJacquet/PiFmRds) and make sure it's working

2. **Configure UFW on the Pi**  
   Install `ufw` and allow port 12345 on your Raspberry Pi:

       sudo apt update && sudo apt install ufw
       sudo ufw allow 12345/tcp
       sudo ufw status
       sudo ufw enable

3. **Netcat**  
   Check if Netcat is installed using `nc -h`. If not, do:

       sudo apt update && sudo apt install netcat

4. **Start netcat to begin listening for audio**

       nc -l -p 12345 | sudo ./pi_fm_rds -audio -

   **Side note:** If you want a specific frequency, you must put the `-freq` parameter before `-audio`:

       nc -l -p 12345 | sudo ./pi_fm_rds -freq 93.3 -audio -

5. **Prepare your Windows machine**  
   - Download and install [ffmpeg](https://www.ffmpeg.org/download.html)
   - Unzip ffmpeg, go to its directory and open a command prompt

   Run the following command to list available audio devices:

       ffmpeg -list_devices true -f dshow -i dummy

   You'll see something like this:

       [dshow @ 0000000000000000] "Microphone (Realtek Audio Device)"
       [dshow @ 0000000000000000]  Alternative name "device_cm_000000"

   Copy the name of your microphone. **It's case sensitive**

6. **Stream Audio to Your Raspberry Pi**  

       ffmpeg -f dshow -i audio="<YOUR AUDIO DEVICE>" -f wav tcp://<IP OF RASPBERRY PI>:12345

   Congrats! If everything was done correctly, you should be piping audio to the radio station. CTRL + C at any time to exit.
---
© [Christophe Jacquet](https://jacquet.xyz/en/) (F8FTK), 2014-2024. Released under the GNU GPL v3.
