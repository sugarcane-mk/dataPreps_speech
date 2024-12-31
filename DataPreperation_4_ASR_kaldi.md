# Dataset Preparation Pipeline for ASR suitable format for Kaldi Recipe

This repository contains Jupyter Notebooks (`.ipynb`) for preparing audio datasets for Automatic Speech Recognition (ASR) systems. The pipeline performs several tasks, including splitting audio files based on time-labeled segments, normalizing audio, generating phonetic transcripts, mapping transcripts to audio files, and creating a phoneme dictionary.

## Prerequisites
Keep your audio files and transcripts ready before processing as given below

```
data/
│
├── audio_files/        # Raw audio
│   ├── audio01.wav
│   ├── audio02.wav
│   └── ...
│
├── labels/            # Time labels for each sentence
│   ├── audio01.lab
│   ├── audio01.lab
│   └── ...
│
├── transcripts.txt    # phonetic form
│
└── original_text.txt  # original form

```
## Installation

1. Install the pydub library:
```bash
!pip install pydub==0.25.1
```

2. Mount your Google Drive to the Colab/Notebook environment:
```python
from google.colab import drive
drive.mount('/content/drive')
```

---

## Steps in the Pipeline
  

### 1. **Audio Splitting and Normalization**
- **Description:**  
  Splits audio files into smaller segments based on time frame labels and normalizes them for consistency.
- The audio files are segmented into individual sentences using `.lab` files containing the start and end timestamps for each sentence.
- These timestamps guide the extraction of specific segments from the audio files, creating individual files for each sentence.
     
- **Output Format:**  
  Each audio segment is saved in `.wav` format with the naming convention:  

**Code:**
```python
from pydub import AudioSegment
import os

# Define file paths and parameters
audio_file = '/content/drive/Shareddrives/Audio_processing/Segmented_data/MSU/label/111-130/111-130(4).wav'
speaker = "MSU"
session = 4

# Example lines for segmentation
lines = [
    "0 100000 1",
    "100000 200000 2",
    # Add more lines with start, end, and sentence numbers
]

for line in lines:
    parts = line.strip().split()
    start_time = float(parts[0]) / 10000
    end_time = float(parts[1]) / 10000
    sentence_number = int(parts[2])

    audio = AudioSegment.from_file(audio_file)
    extracted_audio = audio[start_time:end_time]
    output_file = f"/content/drive/Shareddrives/Audio_processing/Segmented_data/{speaker}/speech/test/{speaker}{sentence_number}{session}.wav"
    os.makedirs(os.path.dirname(output_file), exist_ok=True)
    extracted_audio.export(output_file, format="wav")

print("done")
```
---

### 2. Clean and Split Transcripts

   - Creates `.txt` files for each audio with the transcript only.
   - Removes symbols from the sentences.

**Code:**
```python
import os
import re

def clean_sentence(sentence):
    return re.sub(r'[^a-zA-Z0-9\s]', '', sentence)

def generate_copies(input_file, output_folder):
    os.makedirs(output_folder, exist_ok=True)
    with open(input_file, 'r', encoding='utf-8') as file:
        sentences = file.readlines()
        for sentence_number, sentence in enumerate(sentences, start=1):
            clean_text = clean_sentence(sentence.strip())
            for copy_number in range(1, 11):
                filename = f"MSU_{sentence_number}_{copy_number}.txt"
                file_path = os.path.join(output_folder, filename)
                with open(file_path, 'w', encoding='utf-8') as output_file:
                    output_file.write(clean_text)
    print(f"Files generated in the folder: {output_folder}")

input_file = "/content/transliteration1-197.csv"
output_folder = "/content/drives/Audio_processing/Segmented_data/Suradha_MSR/text"
generate_copies(input_file, output_folder)
```

### 3. **Text-to-Phoneme Mapping**
- **Description:**  
Creates a phoneme dictionary by extracting unique words from all transcripts and splitting them into phonemes. Each entry consists of:
original_text     transcribed_text    phonemes (separated by spaces)

- **Output:**  
A phoneme dictionary in text format.

```
Below is an example of the phoneme dictionary created in the pipeline:

original_text    transcribed_text    phonemes
hello            helo                h e l o
world            wrld                w r l d
example          egzampl             e g z a m p l
...

```

---

### 4. **Data Splitting for Transcripts**
- **Description:**  
Splits all transcripts into individual text files with the same name as their corresponding audio files. This ensures each audio file is easily mapped to its transcript.
- **Output:**  
- Separate text files for each transcript.  
- Example filename: `speakerId_sentenceId_repeatCount.txt`.



---

### 5. **Final Output Files**
After running the pipeline, the following files and directories will be created:
- **Audio Files:**
- **Transcript Files:**
- **Phoneme Dictionary:**
```
data/
│
├── audio_files/
│   ├── speaker_1_1_0.wav
│   ├── speaker_1_1_1.wav
│   └── ...
│
├── spk_2_utt.txt
|
├── transcripts/
│   ├── speaker_1_1_0.txt
│   ├── speaker_1_1_1.txt
│   └── ...
│
├── phoneme_dictionary.txt
│
├── unique_words.txt
│
└── README.md


```
---
## How to Use
- Dowonload the note book [DataPrep_4_ASR.ipynb](https://github.com/sugarcane-mk/dataPreps_speech/blob/main/notebooks/DataPrep_4_ASR.ipynb)
- Modify your paths in the code
- Run using Jupyter or Google colab
---
## Contributing
Contributions to improve this pipeline are welcome! Please submit a pull request or open an issue for any suggestions or bugs.


