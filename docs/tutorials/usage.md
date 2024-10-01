# Usage

To use this framework, you must first install AutoGluon RAG:
```python
git clone https://github.com/autogluon/autogluon-rag
cd autogluon-rag

# Create a Virtual Environment (using Python, or conda if you prefer)
python3 -m virtualenv venv
source venv/bin/activate

#Install the package
pip install -e .
```
You can now use the package in two ways. 

## Use AutoGluon-RAG through the command line as `agrag`:

```python
AutoGluon-RAG


usage: agrag [-h] --config_file

AutoGluon-RAG - Retrieval-Augmented Generation Pipeline

options:
  -h, --help        show this help message and exit
  --config_file        Path to the configuration file 
```

## Use AutoGluon-RAG through code:
```python
from agrag.agrag import AutoGluonRAG


def ag_rag():
    agrag = AutoGluonRAG(
        preset_quality="medium_quality", # or path to config file
        web_urls=["https://auto.gluon.ai/stable/index.html"], # List of URLs to use for RAG
        base_urls=["https://auto.gluon.ai/stable/"], # List of base URLs to use when processing web     
                                                     # URLs. Only Web URLs that stem from a base URL 
                                                     # will be processed.
        parse_urls_recursive=True, # Whether to recursively parse all URLs from the provided web url list
        data_dir="s3://autogluon-rag-github-dev/autogluon_docs/" # Directory containing files to use for RAG
    )
    agrag.initialize_rag_pipeline() # Initializes all modules in the RAG pipeline
    agrag.generate_response("What is AutoGluon?") # Generator


if __name__ == "__main__":
    ag_rag()
```

## Configuring Parameters for AutoGluon-RAG:

### Using `AutoGluonRAG` class
For a list of configurable parameters that can be passed into the `AutoGluonRAG` class, refer to the tutorial [here](general/code_parameters.md). 

### Using Configuration File
You can also use a configuration file with `AutoGluonRAG`.
The configuration file contains the specific parameters to use for each module in the RAG pipeline. For an example of a config file, please refer to `example_config.yaml` in `src/agrag/configs/`. For specific details about the parameters in each individual module, refer to the `README` files in each module in `src/agrag/modules/`.

There is also a `shared` section in the config file for parameters that do not refer to a specific module. Currently, the parameters in `shared` are: 
```python
pipeline_batch_size: Optional batch size to use for pre-processing stage (Data Processing, Embedding, Vector DB Module). This represents the number of files in each batch. The default value is 20.
```