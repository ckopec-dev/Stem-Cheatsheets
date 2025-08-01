# Ollama Cheatsheet

## Installation and basic usage on Ubuntu

### Install

~~~
$ sudo snap install ollama
# or
$ curl -fsSL https://ollama.com/install.sh | sh
$ sudo systemctl enable ollama
$ sudo systemctl start ollama
~~~

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

# Make a request to the api using curl. Response will be in json format.
curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-r1",
  "messages": [{ "role": "user", "content": "Solve: 25 * 25" }],
  "stream": false
}'
~~~

### Remove a local model
`$ ollama rm {model-name}`

## Models

### llama3.1
~~~
# General purpose text-to-text model. 
$ ollama run llama3.1:latest "Explain the basics of machine learning."

    Benchmark test A:
            518.80 msec task-clock                       #    0.001 CPUs utilized
             6,032      context-switches                 #   11.627 K/sec
             1,150      cpu-migrations                   #    2.217 K/sec
             3,271      page-faults                      #    6.305 K/sec
       618,756,209      cycles                           #    1.193 GHz                         (51.11%)
                 0      stalled-cycles-frontend                                                 (50.68%)
                 0      stalled-cycles-backend                                                  (50.61%)
       246,402,399      instructions                     #    0.40  insn per cycle              (49.60%)
        53,257,442      branches                         #  102.655 M/sec                       (49.97%)
         3,487,837      branch-misses                    #    6.55% of all branches             (49.79%)

     620.295709235 seconds time elapsed
       0.214825000 seconds user
       0.333389000 seconds sys

    Benchmark test B:
    real    0m51.176s
    user    0m0.059s
    sys     0m0.073s
~~~
