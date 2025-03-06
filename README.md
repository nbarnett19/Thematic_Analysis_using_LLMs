# Thematic Analysis with Large Language Models

## Overview
This Python package enables qualitative researchers to perform Reflexive Thematic Analysis (RTA), as outlined by Braun and Clarke, using Large Language Models (LLMs). It automates the process of generating codes and themes from textual data using various prompting techniques, including zero-shot, few-shot, and chain-of-thought (CoT) approaches, with optional Retrieval-Augmented Generation (RAG) for enhanced contextualization.

This tool is based on research conducted in the master's thesis "Conducting Qualitative Thematic Analysis with Large Language Models and RAG Implementation" by Natalie A. Barnett (Lucerne University of Applied Sciences and Arts, 2024). The research systematically evaluates LLMs’ ability to perform thematic analysis and informs best practices for integrating AI into qualitative research workflows.

## Features
* **Automated Thematic Analysis**: Generates codes and themes from qualitative data using LLMs.
* **Multiple Prompting Techniques**: Supports zero-shot, few-shot, and chain-of-thought prompting.
* **Retrieval-Augmented Generation** (RAG): Optional integration of external knowledge to enhance theme generation.
* **Structured JSON Outputs**: Outputs codes and themes in an organized format suitable for further analysis.
* **Configurable LLM Models**: Works with all versions of API accessible GPT and Gemini.

## Technologies Used
- **Python**
- **LangChain** for LLM workflow integration
- **OpenAI & Google API** for LLM access
- **Chroma** as a vector database
- **Tesseract OCR & Poppler-utils** for document parsing
- **Pandas, NumPy** for data processing

## Installation
### Step 1: Install System Dependencies (Required for document parsing)
```sh
sudo apt update && sudo apt install -y poppler-utils tesseract-ocr libtesseract-dev libleptonica-dev
```
### Step 2: Install the Package
You can install the package either from PyPI or directly from GitHub.

**Option 1: Install from PyPI (Recommended)**
```
pip install TA_using_LLMs
```
**Option 2: Install from GitHub (For Development & Latest Updates)**
   ```sh
git clone https://github.com/nbarnett19/Thematic_Analysis_using_LLMs.git
cd Thematic_Analysis_using_LLMs
pip install -r requirements.txt

   ```

## Quick Start
```
# Define research questions (as a list of strings)
rqs = ["How does self-tracking influence understanding of glucose metabolism?"]

# Establish connection to LLM via API
from TA_using_LLMs.logic import ModelManager
model_manager = ModelManager(model_choice='gemini-1.5-pro', temperature=0.5, top_p=0.5)

# Load text data from folder
from TA_using_LLMs.logic import FolderLoader
loader = FolderLoader(folder_path)
docs = loader.load_txt()

# Split documents into chunks (for improved analysis)
chunks = loader.split_text(docs, chunk_size=1000, chunk_overlap=500)

# Initialize Thematic Analysis with the loaded data
from TA_using_LLMs.logic import ThematicAnalysis
prompt = ThematicAnalysis(llm=model_manager.llm, docs=docs, chunks=chunks, rqs=rqs)

# Perform analysis using zero-shot control prompting
simple_TA_analysis = prompt.zs_control_gemini(filename="simple_TA_analysis.json")

# Convert results to a Pandas dataframe for easy inspection
import pandas as pd
df = pd.json_normalize(simple_TA_analysis)
print(df.head())  # Display the first few rows
```
For an in-depth demo of the package, please refer to this [Colab](https://colab.research.google.com/drive/19MrRwsY0dn3rtzGQUKtI1Ubyb0Swz0Rw?usp=sharing)

## Background & Research
This package implements methodologies developed in my master's thesis, which investigates LLM capabilities in qualitative research. The study compares different prompt engineering strategies and evaluates their effectiveness using expert feedback and statistical analyses. Results indicate that:
- **CoT prompting** produces the most analytically coherent themes.
- **RAG** does not consistently improve results and may introduce noise.
- **GPT-4o and Gemini 1.5 Pro** exhibit strong thematic alignment, with minor variations in interpretation.

If you are interested in the theoretical foundation and methodology, you can read the full thesis here: [LINK TO THESIS](https://drive.google.com/file/d/1fK1tvNWiJrrVz3b2TGcqcA_4VDaKf0bd/view?usp=sharing)

### How It Works
1. Data Preprocessing: Text is chunked for better LLM processing.
2. Code Generation: The LLM identifies meaningful segments and assigns codes.
3. Theme Generation: The LLM groups codes into coherent themes.
4. (Optional) RAG Integration: External documents provide additional context to enhance thematic analysis.

### Example Output
```
{
    "theme": "Influence of Self-Tracking on Understanding Glucose Metabolism",
    "theme_definition": "This theme explores how self-tracking with a glucose sensor influences residents' understanding of glucose metabolism, highlighting the physiological insights gained and the impact of lifestyle factors on glucose levels.",
    "subthemes": [
        "Physiological Insights from Self-Tracking",
        "Impact of Lifestyle Factors on Glucose Levels"
    ],
    "subtheme_definitions": [
        "Residents gained insights into the physiological processes of glucose metabolism, such as the body's response to different foods and activities, through self-tracking.",
        "Residents observed how lifestyle factors like stress, diet, and physical activity influence glucose levels, enhancing their understanding of glucose metabolism."
    ],
    "supporting_quotes": [
        "I found it impressive to observe my blood sugar for the first time after eating a pizza over lunch. It shot up from under 6 to almost 9 mmol/l.",
        "I noticed how little the feeling of low blood sugar correlates with hunger. The idea that 'I am hypoglycemic' is only true to a very limited extent.",
        "I've noticed myself when I'm under stress, when I'm having a hard day, I feel shaky and then I eat something and then I feel better and somehow that's where the interest came from: 'Okay, what's actually going on with my blood sugar?'",
        "And also to get a feeling for which foods have which influence, for example a very balanced meal with lots of protein actually caused a relatively stable curve and you felt very good, whereas a very carbohydrate-rich meal with almost exclusively carbohydrates caused big peaks."
    ]
}
```

## Citation
If you use this package in your research, please cite:
> Barnett, N. A. (2024). Conducting Qualitative Thematic Analysis with Large Language Models and RAG Implementation. Lucerne University of Applied Sciences and Arts. [LINK TO THESIS](https://drive.google.com/file/d/1fK1tvNWiJrrVz3b2TGcqcA_4VDaKf0bd/view?usp=sharing)

## License
This project is licensed under the MIT License. See `LICENSE` for details.

---
