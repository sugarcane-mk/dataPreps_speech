# Dataset Preparation Scripts

This repository contains scripts for preparing datasets for various audio processing and machine learning tasks. Below is a description of each script and its purpose, along with links to the corresponding code files.

---

## 1. Clean And Prepare Transcripts (`spk2utt` Preparation)
**Description:**  
This script processes a text file containing sentence numbers and transcripts, cleans the transcripts, and generates a `spk2utt.txt` file. Each transcript is repeated 10 times with a unique speaker ID.

- **Input File Format:**
```
1. kadxawulai wandandgu.
2. nandxri sei!
```
- **Output File Format (`spk2utt.txt`):**
```
speakerID_1_0 kadxawulai wandandgu
speakerID_1_1 kadxawulai wandandgu
...
speakerID_2_9 nandxri sei
```
---

## 2. Audio File Renaming and Organizing
**Description:**  
This script renames audio files based on their metadata and organizes them into folders for easy processing.

- **Input:**  
- A folder of audio files with inconsistent naming.

- **Output:**  
- Renamed audio files organized in structured folders.

---

## Usage Instructions

To use any of the scripts, clone this repository and navigate to the script directory:

```bash
git clone https://github.com/sugarcane-mk/dataPreps_speech.git
cd dataPreps_speech/Notebooks
```
Open the `.ipynb` notebook in Jupyter or Google Colab and execute.

## Notebook link
1. [Data Preparation for ASR](https://github.com/sugarcane-mk/dataPreps_speech/blob/main/Notebooks/DataPrep_4_ASR.ipynb), [Refer Details](https://github.com/sugarcane-mk/dataPreps_speech/blob/main/Data_4_ASR_kaldi.md)
   
