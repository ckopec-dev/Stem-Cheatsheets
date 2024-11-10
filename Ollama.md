# Ollama Cheatsheet

## Installation and basic usage on Ubuntu

~~~
$ curl -fsSL https://ollama.com/install.sh | sh

# Start ollama
$ ollama serve

# Pull a model
$ ollama pull llama3.1:latest

# Show available models
$ ollama list

# Run a model
$ ollama run llama3.1:latest

# Ask a question
>>> How much does a duck weigh?

# Exit the interactive interface
>>> /bye

# Get help
$ ollama help

# Stop a running model
$ ollama stop llama3.1:latest
~~~
