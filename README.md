# Multi-Turn Benchmark
summary goes here
## **Project Structure**
- `data/` : Contains input files for conversations (input_conversations.jsonl) and optional model response files (model_responses.jsonl) used in the benchmark.
- `results/` : Stores the benchmark's output, including evaluation scores and metrics, saved to evaluation_results.txt.
- `src/` : Core functionality for the benchmark
  - `models/` : Houses model provider classes:
  
## **Setup Instructions**
1. **Clone the Repository**
   ```bash
   git clone some_directory
   cd name_of_benchmark
   ```
2. **Install Requirements**
   ```bash
   pip install -r requirements.txt
   ```
3. **Create `.env` File**
   Create a `.env` file in the root directory with your API keys. For example:
   ```plaintext
   OPENAI_API_KEY=your-openai-api-key (REQUIRED)
   HUGGINGFACE_TOKEN=your-huggingface-token
   ```
## **Usage**
### **1. Using Pre-Generated Responses**
If you already have model responses and want to evaluate them:
```bash
python main.py --responses-file data/model_responses.jsonl --output-file results/evaluation_results.txt
```
Make sure to format the responses file as shown in data/responses_template.jsonl

### **2. Generating Responses with a Model**
To dynamically generate responses using a supported model provider:
```bash
python main.py --model-provider openai --provider-args model=gpt-4o temp=0 --output-file results/evaluation_results.txt
```

### **3. Using Pass@k Evaluation**
To evaluate model performance with multiple attempts per conversation:
```bash
python main.py --model-provider openai --k 3 --output-file results/evaluation_results.txt
```
This will generate 3 responses per conversation and consider it successful if any attempt passes.

### **4. Generating Detailed Raw Output**
To save comprehensive evaluation details including all responses and judgments:
```bash
python main.py --model-provider openai --k 3 --output-file results/evaluation_results.txt --raw results/detailed_results.txt
```

### **Command-Line Arguments**
- `--output-file`: Path to save the final evaluation results.
- `--responses-file`: Path to a file containing pre-generated responses. (OPTIONAL)
- `--model-provider`: Specify the model provider for generating responses (`huggingface`, `openai`, etc.).
- `--provider-args`: Model-specific arguments in key=value format (e.g., `model_path=/path/to/model`).
- `--k`: Number of attempts to generate for each conversation (pass@k evaluation). Defaults to 1. (OPTIONAL)
- `--raw`: Path to save detailed raw output including all responses and evaluations. (OPTIONAL)

### **Evaluation Results**
The evaluation results include:
1. **In evaluation_results.txt:**
   - Overall Pass@k Score: Percentage of conversations where at least one attempt meets the criteria
   - Axis Scores: Per-axis scores based on pass@k criteria

2. **In detailed_results.txt (if --raw is specified):**
   - Complete conversation history
   - All model responses for each attempt
   - Judge's verdicts and reasoning
   - Expected pass criteria
   - Per-conversation pass/fail statistics
---
## **Project Dependencies**
See `requirements.txt` for a complete list of required packages.
---
