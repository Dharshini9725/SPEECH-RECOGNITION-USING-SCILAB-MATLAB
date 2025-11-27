### AIM: 

To perform and verify speech recognition using SCILAB. 

### APPARATUS REQUIRED: 
PC installed with SCILAB. 

### PROGRAM : 
```
# 🎤 Speech-to-Text Colab demo: Whisper + SpeechRecognition fallback
# ---------------------------------------------------------------

# 1) Install dependencies for Whisper
!pip install -q openai-whisper pydub
!apt -qq install -y ffmpeg

import os
from google.colab import files

print("👉 Upload your audio file (wav / mp3 / m4a / etc):")
uploaded = files.upload()
file_path = list(uploaded.keys())[0]
print("Uploaded file:", file_path)


# Optionally, convert to WAV for consistency (e.g. if mp3 / m4a)
from pydub import AudioSegment

ext = file_path.split('.')[-1].lower()
if ext not in ["wav", "WAV"]:
    try:
        audio = AudioSegment.from_file(file_path)
        file_path = "converted.wav"
        audio.export(file_path, format="wav")
        print("Converted to WAV as", file_path)
    except Exception as e:
        print("Could not convert to WAV, will try original file. Error:", e)

# 2) Use Whisper to transcribe
import whisper

print("Loading Whisper model...")
model = whisper.load_model("base")  # you can choose "tiny", "small", "base", "medium", "large"
print("Model loaded. Transcribing...")
result = model.transcribe(file_path)
text = result["text"]
print("✅ Whisper transcription:")
print(text)

# (Optional) Save transcription to file
with open("transcription.txt", "w", encoding="utf-8") as f:
    f.write(text)
print("Saved transcription to transcription.txt")


# 3) Fallback / alternative (for short/simple English audio): SpeechRecognition + Google API
print("\n--- Fallback: SpeechRecognition method ---")
!pip install -q SpeechRecognition pydub

import speech_recognition as sr

r = sr.Recognizer()
try:
    with sr.AudioFile(file_path) as source:
        audio_data = r.record(source)
    print("Recognizing with Google Web Speech API …")
    text2 = r.recognize_google(audio_data)
    print("✅ SpeechRecognition result:")
    print(text2)
except Exception as e:
    print("SpeechRecognition failed:", e)


```

### OUTPUT: 
<img width="886" height="455" alt="Screenshot 2025-11-27 183229" src="https://github.com/user-attachments/assets/ade96fa0-1526-47d6-9c19-63ec3f748db4" />

### Result:
Thus the speech recognition using COLAB was performed and verified.

