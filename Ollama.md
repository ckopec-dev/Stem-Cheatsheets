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

### deepseek-r1

~~~
# Text-to-code.

$ ollama run deepseek-r1:1.5b "Explain the basics of machine learning."

    Benchmark test A:
            806.79 msec task-clock                       #    0.002 CPUs utilized
             7,600      context-switches                 #    9.420 K/sec
             2,840      cpu-migrations                   #    3.520 K/sec
             3,269      page-faults                      #    4.052 K/sec
       964,847,524      cycles                           #    1.196 GHz                         (47.68%)
                 0      stalled-cycles-frontend                                                 (46.88%)
                 0      stalled-cycles-backend                                                  (51.72%)
       334,623,529      instructions                     #    0.35  insn per cycle              (52.80%)
        69,727,138      branches                         #   86.425 M/sec                       (53.28%)
         5,325,708      branch-misses                    #    7.64% of all branches             (48.64%)

     537.352191971 seconds time elapsed
       0.361164000 seconds user
       0.473059000 seconds sys

    Benchmark test B:
    real    0m33.382s
    user    0m0.077s
    sys     0m0.130s

$ ollama run deepseek-r1:7b "Explain the basics of machine learning."

    Benchmark test A:
            607.68 msec task-clock                       #    0.001 CPUs utilized
             6,694      context-switches                 #   11.016 K/sec
             1,568      cpu-migrations                   #    2.580 K/sec
             3,195      page-faults                      #    5.258 K/sec
       724,547,949      cycles                           #    1.192 GHz                         (49.81%)
                 0      stalled-cycles-frontend                                                 (52.62%)
                 0      stalled-cycles-backend                                                  (53.38%)
       301,312,826      instructions                     #    0.42  insn per cycle              (50.60%)
        64,672,186      branches                         #  106.424 M/sec                       (47.85%)
         4,103,756      branch-misses                    #    6.35% of all branches             (47.03%)

     850.357542925 seconds time elapsed
       0.280679000 seconds user
       0.355879000 seconds sys

    Benchmark test B:
    real    1m30.386s
    user    0m0.075s
    sys     0m0.114s

$ ollama run deepseek-r1:8b "Explain the basics of machine learning."

    Benchmark test A:
    1,454.27 msec task-clock                       #    0.000 CPUs utilized
            15,801      context-switches                 #   10.865 K/sec
             3,710      cpu-migrations                   #    2.551 K/sec
             3,268      page-faults                      #    2.247 K/sec
     1,721,780,564      cycles                           #    1.184 GHz                         (48.98%)
                 0      stalled-cycles-frontend                                                 (48.64%)
                 0      stalled-cycles-backend                                                  (50.61%)
       782,692,792      instructions                     #    0.45  insn per cycle              (51.23%)
       172,360,838      branches                         #  118.520 M/sec                       (51.52%)
        10,006,724      branch-misses                    #    5.81% of all branches             (49.53%)

    4170.998087820 seconds time elapsed
       0.736106000 seconds user
       0.770648000 seconds sys

    Benchmark test B:
    real    2m55.654s
    user    0m0.108s
    sys     0m0.138s

$ ollama run deepseek-r1:14b "Explain the basics of machine learning."

    Benchmark test A:
          1,304.85 msec task-clock                       #    0.000 CPUs utilized
            13,547      context-switches                 #   10.382 K/sec
             2,930      cpu-migrations                   #    2.245 K/sec
             3,162      page-faults                      #    2.423 K/sec
     1,456,021,624      cycles                           #    1.116 GHz                         (47.46%)
                 0      stalled-cycles-frontend                                                 (48.98%)
                 0      stalled-cycles-backend                                                  (52.23%)
       665,386,472      instructions                     #    0.46  insn per cycle              (52.72%)
       145,364,880      branches                         #  111.404 M/sec                       (51.29%)
         8,366,781      branch-misses                    #    5.76% of all branches             (47.98%)

    3767.132665010 seconds time elapsed
       0.631358000 seconds user
       0.720608000 seconds sys

    Benchmark test B:
    real    4m15.584s
    user    0m0.096s
    sys     0m0.132s

$ ollama run deepseek-r1:32b "Explain the basics of machine learning."

    Benchmark test B:
    real    9m21.230s
    user    0m0.087s
    sys     0m0.126s

$ ollama run deepseek-r1:70b "Explain the basics of machine learning."

    Benchmark test B:
    real    24m6.046s
    user    0m0.113s
    sys     0m0.246s
~~~

### gemma3

~~~
# Text-to-text.

$ ollama run gemma3:1b "Explain the basics of machine learning."

    Benchmark test A:
            525.46 msec task-clock                       #    0.002 CPUs utilized
             5,108      context-switches                 #    9.721 K/sec
             1,268      cpu-migrations                   #    2.413 K/sec
             3,203      page-faults                      #    6.096 K/sec
       627,999,644      cycles                           #    1.195 GHz                         (50.80%)
                 0      stalled-cycles-frontend                                                 (53.41%)
                 0      stalled-cycles-backend                                                  (52.14%)
       223,243,353      instructions                     #    0.36  insn per cycle              (49.77%)
        50,680,348      branches                         #   96.450 M/sec                       (47.06%)
         3,443,723      branch-misses                    #    6.79% of all branches             (48.54%)

     261.467038840 seconds time elapsed
       0.217555000 seconds user
       0.327515000 seconds sys

    Benchmark test B:
    real    0m12.747s
    user    0m0.046s
    sys     0m0.093s

# Image-to-text.

$ ollama run gemma3:4b "Describe this image" /duck.jpg
# Note: the output description was completely inaccurate.

    Benchmark test B:
    real    0m8.924s
    user    0m0.030s
    sys     0m0.066s
~~~

### gemma3n

~~~
# Text-to-text.

$ ollama run gemma3n:e2b "Explain the basics of machine learning."

    Benchmark test A:
            789.64 msec task-clock                       #    0.001 CPUs utilized
             8,159      context-switches                 #   10.333 K/sec
             2,094      cpu-migrations                   #    2.652 K/sec
             3,295      page-faults                      #    4.173 K/sec
       938,565,772      cycles                           #    1.189 GHz                         (51.35%)
                 0      stalled-cycles-frontend                                                 (49.77%)
                 0      stalled-cycles-backend                                                  (47.01%)
       330,715,955      instructions                     #    0.35  insn per cycle              (49.01%)
        71,620,312      branches                         #   90.700 M/sec                       (50.44%)
         5,247,369      branch-misses                    #    7.33% of all branches             (53.22%)

     714.484195718 seconds time elapsed
       0.360330000 seconds user
       0.462163000 seconds sys

    Benchmark test B:
    real    1m13.451s
    user    0m0.081s
    sys     0m0.139s

$ ollama run gemma3n:e4b "Explain the basics of machine learning."

    Benchmark test A:
            781.21 msec task-clock                       #    0.001 CPUs utilized
             8,653      context-switches                 #   11.076 K/sec
             1,951      cpu-migrations                   #    2.497 K/sec
             3,246      page-faults                      #    4.155 K/sec
       938,725,361      cycles                           #    1.202 GHz                         (50.64%)
                 0      stalled-cycles-frontend                                                 (48.92%)
                 0      stalled-cycles-backend                                                  (51.72%)
       347,590,011      instructions                     #    0.37  insn per cycle              (49.78%)
        75,290,670      branches                         #   96.377 M/sec                       (51.39%)
         5,404,280      branch-misses                    #    7.18% of all branches             (48.48%)

     968.548975603 seconds time elapsed
       0.334691000 seconds user
       0.480663000 seconds sys

    Benchmark test B:
    real    1m47.846s
    user    0m0.087s
    sys     0m0.120s
~~~

### llama3.1

~~~
# Text-to-text.

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

$ ollama run llama3.1:70b "Explain the basics of machine learning."

    Benchmark test B:
    real    11m10.060s
    user    0m0.063s
    sys     0m0.089s
~~~

### mistral-small3.2

~~~
# Text-to-code.

$ ollama run mistral-small3.2:24b "Explain the basics of machine learning."

    Benchmark test B:
    real    19m19.772s
    user    0m0.532s
    sys     0m0.665s
~~~

### qwen2.5vl

~~~
# Text-to-text.

$ ollama run qwen2.5vl:7b "Explain the basics of machine learning."

    Benchmark test B:
    real    0m34.101s
    user    0m0.048s
    sys     0m0.074s

# Text-to-image.
$ ollama run qwen2.5vl:7b "What's in this image? ./duck.jpg"

  Benchmark test B:
  real    1m21.230s
  user    0m0.136s
  sys     0m0.184s
~~~

### qwen3

~~~
# Text-to-text.

$ ollama run qwen3:8b "Explain the basics of machine learning."

    Benchmark test B:
    real    2m9.977s
    user    0m0.083s
    sys     0m0.118s
~~~

### qwen3-coder

~~~
# Text-to-code.

$ ollama run qwen3-coder "Show quicksort algorithm in python."

    Benchmark test B:
    real    1m4.136s
    user    0m0.065s
    sys     0m0.127s
~~~
