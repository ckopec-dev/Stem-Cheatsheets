# Ollama Cheatsheet

## Installation and basic usage on Ubuntu

### Install
`$ sudo snap install ollama`

### Show version
`$ ollama --version`

### Show help
`$ ollama --help`

### Pull a model locally

~~~
# Good general purpose model
$ ollama pull llama3.2

# Model for code generation
$ ollama pull qwen2.5-coder
~~~

### Run a model and ask it something

~~~
# Outputs a sort summary of topic
$ ollama run llama3.2 "Explain the basics of machine learning."

# Outputs python code
$ ollama run qwen2.5-coder "Show quicksort algorithm in python."
~~~

### Show running models
`$ ollama ps`

### Show locally available models
`$ ollama list`

### Run as an api service

~~~
# This will run ollama on localhost port 11434
$ ollama serve

curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-r1",
  "messages": [{ "role": "user", "content": "Solve: 25 * 25" }],
  "stream": false
}'
~~~
