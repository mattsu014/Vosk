## Introduction
What is [VOSK](https://alphacephei.com/vosk/)? VOSK is a powerful tool for real-time speech recognition that does not require an internet connection. Developed by [Alpha Cephei](https://alphacephei.com/en/), it supports multiple languages and is highly efficient. It can even run on low-performance devices, such as the Raspberry Pi.

## Using VOSK
To use VOSK, you first need to download the model from this website: [https://alphacephei.com/vosk/models](https://alphacephei.com/vosk/models)

VOSK offers models for many languages, including Portuguese, English, Japanese, and others:

![Vosk Models Exemples](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/t8gl8kzs7h5nqsdnrize.png)

Once you download the ZIP file containing the VOSK model, you will need to unzip it

![VOSK zip](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sl5ou8h07gs1itfsgy4j.png)

---

## Install Dependences
**First**, you need to install the VOSK package and PyAudio using the following command:

```
pip install vosk pyaudio
```
PyAudio is a library that allows you to capture and reproduce audio using the PortAudio API. In this code, PyAudio is used to:

- Open the microphone.
- Capture audio in real time.
- Collect audio data to Vosk for processing.

---

## Let's Code!!!
Code Exemple:

```
import pyaudio
import json
from vosk import Model, KaldiRecognizer

model = Model("vosk-model-en-us-0.22")
recognizer = KaldiRecognizer(model, 16000)

p = pyaudio.PyAudio()
stream = p.open(format=pyaudio.paInt16, channels=1, rate=16000, input=True, frames_per_buffer=8192)
stream.start_stream()

print("listening...")

while True:
    data = stream.read(4096, exception_on_overflow=False)
    if recognizer.AcceptWaveform(data):
        result = json.loads(recognizer.Result())
        print("Você disse:", result["text"])

```
I created this simple example code to demonstrate how VOSK works.

**1. Library Imports**

```
import pyaudio
import json
from vosk import Model, KaldiRecognizer
```

It's a simple import library

**2. Speech recognition model loading**

```
model = Model("vosk-model-en-us-0.22")
```
This loads the VOSK model for English (US).
**3. Speech recognizer initialization**

```
recognizer = KaldiRecognizer(model, 16000)
```

- **KaldiRecognizer:** Starts Speech Recognition model with 16000 Hz (16 kHz) sampling rate, commum sampling rate for Speech Recognition

**4. Audio Settings**

```
p = pyaudio.PyAudio()
stream = p.open(format=pyaudio.paInt16, channels=1, rate=16000, input=True, frames_per_buffer=8192)
stream.start_stream()
```
- **pyaudio.PyAudio():** Initializes the PyAudio object, which is used to configure and manage audio capture.

- **p.open():** Opens an audio stream with the following settings:

- **format=pyaudio.paInt16:** Audio format of 16 bits.

- **channels=1:** Mono audio (1 channel).

- **rate=16000:** Sampling rate of 16000 Hz.

- **input=True:** Indicates that the stream will be used for audio input (capture).

- **frames_per_buffer=8192:** Size of the audio buffer.

- **stream.start_stream():** Starts the audio capture.

**5. Speech Recognition Loop**

```
print("listening...")

while True:
    data = stream.read(4096, exception_on_overflow=False)
    if recognizer.AcceptWaveform(data):
        result = json.loads(recognizer.Result())
        print("You said:", result["text"])
```

- **stream.read(4096, exception_on_overflow=False):**
 Reads 4096 frames of audio from the stream. The exception_on_overflow=False parameter prevents the program from raising an exception in case of overflow.

- **recognizer.AcceptWaveform(data):** Sends the audio data to the speech recognizer. If the recognizer detects a complete phrase, it returns True.

- **json.loads(recognizer.Result()):** Converts the speech recognition result (which is in JSON format) into a Python dictionary.

## Conclusion
VOSK is a powerful and efficient tool for real-time speech recognition, supporting multiple languages and running seamlessly on low-performance devices like the Raspberry Pi. 
Its offline capability makes it ideal for applications where internet access is limited. 
With this guide, you’ve learned how to set up VOSK, configure audio capture, and implement a basic speech recognition system. 
Whether for voice-controlled apps, transcription tools, or language learning, VOSK offers a simple yet robust solution for integrating speech recognition into your projects.

---
Thanks for reading 🙃

I posted this article on Dev.to, so I thought it would be interesting to repost it here as well. 😊

- Follow my dev.to account: [@mattsu014](https://dev.to/mattsu014)

- Original Post: [VOSK the Offline Speech Recognition](https://dev.to/mattsu014/vosk-offline-speech-recognition-3kbb)

