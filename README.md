# AI-Based Startup Classification Toolkit

This repository contains two core Python pipelines for automated classification of healthcare startups using OpenAI LLMs.

## 🎯 Overview

The toolkit enables two distinct types of automated classification:

1. **Healthcare Category Classification** – Assigns each startup to one of nine high-level medical categories
2. **AI Complexity Classification** – Evaluates the technical sophistication of the AI system used by each startup

Both pipelines support Excel input files, robust retry logic, OpenAI API integration, and structured output formats.

## 📂 Repository Structure

```
.
├── category_classification/
│   └── classify_startups.py
├── ai_complexity_classification/
│   └── classify_ai_complexity.py
├── input_files/
├── output_files/
└── README.md
```

## 🏥 Healthcare Category Classification

### Purpose

Classifies healthcare startups into nine high-level medical categories based on:
- Company description
- Website content (optional)
- Comprehensive taxonomy of subcategories

**Use Cases:**
- Market mapping and landscape analysis
- Healthcare startup research
- Building labeled datasets for ML
- Investment and scouting workflows

### Categories

The model classifies startups into one of the following categories:

- Clinical Decision Support
- Imaging and Diagnostics
- Drug Discovery
- Mental Health
- Surgery
- Medical Treatment and Personalized Medicine
- Public Health and Epidemiology
- Medical Monitoring
- Rehabilitation and Assistive Technologies

### How It Works

1. **Load Data**: Reads Excel file with startup information
2. **Build Prompt**: Combines company description and website content
3. **Classify**: Sends prompt to GPT-4o with retry logic
4. **Process**: Applies classification to all rows
5. **Save**: Exports results to Excel

### Usage

```python
# Insert your OpenAI API key
openai.api_key = "YOUR_KEY_HERE"

# Adjust file paths as needed
df = pd.read_excel('classified_startups_with_LLM_notranslation.xlsx')

# Choose whether to run full file or test with first 20 rows
run_full_file = False

# Run classification
python classify_startups.py
```

## 🤖 AI Complexity Classification

### Purpose

Evaluates the technical sophistication of a startup's AI system and assigns a complexity level:

- **Low**: Rule-based systems, basic automation
- **Moderate**: Traditional ML algorithms
- **High**: Deep learning, CNNs, RNNs
- **Advanced**: Multimodal models, transfer learning, GANs
- **Pioneering**: Federated learning, edge AI, neurosymbolic AI

**Use Cases:**
- Assessing technical defensibility
- VC technical due diligence
- Benchmarking AI maturity across portfolios
- Academic research on AI adoption in healthcare

### Classification Criteria

The LLM evaluates three key dimensions:

1. **Algorithm Complexity**: Type and sophistication of AI/ML models used
2. **Integration Depth**: EHR integration, workflow automation, clinical decision support
3. **Clinical Validation**: FDA/CE approvals, clinical trials, hospital partnerships

### How It Works

1. **Preprocess**: Converts Excel to Parquet for faster processing (optional)
2. **Build Prompt**: Generates detailed AI complexity evaluation prompt
3. **Classify**: Uses GPT-4 to assess AI sophistication
4. **Parallel Processing**: Leverages ThreadPoolExecutor for concurrent execution
5. **Save**: Exports results to Parquet format

### Usage

```python
# Add your OpenAI key
openai.api_key = "YOUR_KEY_HERE"

# Verify file paths
df = pd.read_parquet('only_content_25.parquet')

# Run classification with parallel processing
python classify_ai_complexity.py
```

## 📦 Requirements

### Python Packages

```bash
pip install pandas openai pyarrow tqdm
```

**Dependencies:**
- `pandas` - Data manipulation
- `openai` - OpenAI API client
- `pyarrow` - Parquet file support
- `tqdm` - Progress bars
- `concurrent.futures` - Parallel processing (standard library)

## 🔑 API Key Setup

**Option 1: Direct (not recommended for production)**
```python
openai.api_key = "YOUR_KEY_HERE"
```

**Option 2: Environment Variable (recommended)**
```bash
export OPENAI_API_KEY="sk-XXXX"
```

```python
import os
openai.api_key = os.environ["OPENAI_API_KEY"]
```

## 📊 Output Files

| File | Description |
|------|-------------|
| `classified_startups_with_LLM_notranslation.xlsx` | Healthcare category classifications |
| `only_content_25.parquet` | Preprocessed data for faster processing |
| `classified_startups_ai_complexity.parquet` | AI complexity level classifications |

## ⚙️ Best Practices

- **Testing**: Default behavior processes only first 20 rows for safety
- **Performance**: Parallel execution significantly speeds up AI complexity classification
- **Reliability**: Retry loops with exponential backoff prevent API failures
- **Accuracy**: Long-form prompts with detailed context improve classification quality
- **Cost Management**: Monitor API usage when processing large datasets

## 📝 License

MIT License

## 🤝 Contributing

Contributions are welcome! Please feel free to:
- Create an issue for bugs or feature requests
- Open a pull request with improvements
- Share feedback on classification accuracy

## 📧 Contact

For questions or collaboration opportunities, please contact: a.zahlan@ucl.ac.uk 

---

**Note**: This toolkit requires an active OpenAI API key and incurs costs based on usage. Please refer to OpenAI's pricing documentation for details.
