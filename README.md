# ConstitutionalAiTuning

## About

`ConstitutionalAiTuning` is a Python library designed to facilitate the fine-tuning of Large Language Models (LLMs) using the constitutional AI approach. This library provides tools for generating prompts for initial answers, critiques, and revisions, and includes utilities for managing different constitutions the LLM can be trained on.

## Features

- **Prompt Template Generation**: Generate structured prompts for model training and evaluation.
- **Constitution Loader**: Load and parse JSON files representing different constitutional setups.
- **Model Interaction**: Interface with the LLM to obtain initial answers, critiques, and revisions.
- **Fine-Tuning Utilities**: Tools to streamline the process of fine-tuning your LLM.

## Installation

To install `ConstitutionalAiTuning`, you can use the following pip command:

```bash
pip install git+https://github.com/steffen74/ConstitutionalAiTuning.git

```

## Usage

Here is a basic example of how to use `ConstitutionalAiTuning`:

```python
import torch
from transformers import pipeline
from ConstitutionalAiTuning.constitution_loader import load_constitution
from ConstitutionalAiTuning.interaction import ModelInteractor

# Initialize the text generation pipeline (use an appropriate model)
text_gen_pipeline = pipeline(
    "text-generation",
    model="HuggingFaceH4/zephyr-7b-beta",
    torch_dtype=torch.bfloat16,
    device_map="auto",
)

# Load a constitution file (see examples/educational_assistant.json for an example)
# Replace with the actual path to your constitution file
constitution = load_constitution('examples/educational_assistant.json')

# Initialize the ModelInteractor with the pipeline
interactor = ModelInteractor(text_gen_pipeline)

# Example input prompts (assuming a list of prompts)
questions = [{'question': 'Example question 1'}, {'question': 'Example question 2'}]

# Run a single interaction with the first prompt in the list
single_interaction_response = interactor.run_single_interaction(input_prompts, constitution, prompt_index=0)

print("Single Interaction Response:")
print("Question:", single_interaction_response['question'])
print("Initial Answer:", single_interaction_response['initial_answer'])
print("Critique:", single_interaction_response['critique'])
print("Revision:", single_interaction_response['revision'])
print("\n")

# Run the interaction loop to get responses for each prompt
responses = interactor.run_interaction_loop(input_prompts, constitution)

# Display the responses from the interaction loop
print("Interaction Loop Responses:")
for response in responses:
    print("Question:", response['question'])
    print("Initial Answer:", response['initial_answer'])
    print("Critique:", response['critique'])
    print("Revision:", response['revision'])
    print("\n")
```

### Explanation

- **Pipeline Initialization**: We start by initializing a text generation pipeline from Hugging Face's Transformers. You can specify any model compatible with the text generation task.

- **Constitution Loading**: We load the constitution using `load_constitution`, which contains the instructions for generating prompts.

- **Prompt Generation**: An instance of `PromptTemplate` is created, and an initial answer prompt is generated.

- **Model Interactor**: We initialize the `ModelInteractor` class with the pipeline.

- **Execute LLM Request**: Using the `execute_llm_request` method of `ModelInteractor`, we pass the generated prompt to get the initial answer.

- **Interaction Loop**: Demonstrates how to use the `run_interaction_loop` method to process multiple prompts and generate initial answers, critiques, and revisions.

### Notes

- Make sure to replace `'path/to/constitution.json'` and the chosen model in the pipeline defition with the actual path to your constitution file and the model you intend to use.
- This example assumes that the necessary packages (`transformers`, etc.) are installed and that `constitution.json` is correctly formatted.
- The exact output and behavior will depend on the model used and the contents of the constitution instructions.

## Contributing

We welcome contributions to `ConstitutionalAiTuning`! Please read our [Contributing Guidelines](CONTRIBUTING.md) for information on how to get involved.

## License

`ConstitutionalAiTuning` is licensed under the [MIT License](LICENSE).

## Contact

For questions and feedback, please reach out to [steffen@opencampus.sh](mailto:steffen@opencampus.sh).
