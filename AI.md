
# AI Cheatsheet

## Text to text

~~~
# Pull model
$ ollama pull llama3.2

# Make request
$ ollama run llama3.2 "Explain the basics of machine learning."
~~~

## Text to code

~~~
# Pull model
$ ollama pull qwen2.5-coder

# Make request
$ ollama run qwen2.5-coder "Show quicksort algorithm in python."
~~~

## Text to image

## Image to image

## Image to text

~~~
# Pull model
$ ollama pull llava:7b

# Make request
$ ollama run llava:7b "What's in this image? ./duck.jpg"
~~~
