# Audio Segmentation and Processing

This repository contains a Python script for segmenting audio files, cleaning and splitting transcripts, generating lexicons, and normalizing audio.

## Prerequisites

- Google Colab or Jupyter Notebook environment
- Python 3.x
- pydub library
- Google Drive mounted to Colab/Notebook

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

## Segmentation

1. **Audio Segmentation:**
   - The audio files are segmented into individual sentences using `.lab` files containing the start and end timestamps for each sentence.
   - These timestamps guide the extraction of specific segments from the audio files, creating individual files for each sentence.

2. **Code:**
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

## Clean and Split Transcripts

1. **Clean & Split:**
   - Creates `.txt` files for each audio with the transcript only.
   - Removes symbols from the sentences.

2. **Code:**
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

## Lexicon Generation

1. **Unique Word Identification:**
   - Processes text files to identify unique words.

2. **Code:**
```python
import re
import os

def process_text_files(input_filepath, output_filepath):
    unique_words = set()
    try:
        with open(input_filepath, 'r', encoding='utf-8') as infile:
            for line in infile:
                cleaned_line = re.sub(r'[^\w\s]', '', line).lower()  # Convert to lowercase
                words = cleaned_line.split()
                unique_words.update(words)

        with open(output_filepath, 'w', encoding='utf-8') as outfile:
            for word in sorted(unique_words):
                outfile.write(word + '\n')
        print(f"Unique words saved to: {output_filepath}")
    except Exception as e:
        print(f"Error processing file: {e}")

input_filepath = "/content/drive/Shareddrives/Audio_processing/Segmented_data/Suradha_MSR/text/sample.txt"
output_filepath = "/content/drive/Shareddrives/Audio_processing/Segmented_data/Suradha_MSR/lexicon.txt"
process_text_files(input_filepath, output_filepath)
