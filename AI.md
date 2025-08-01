
# AI Cheatsheet

Examples below primarily focus on CPUs for computations instead of GPUs, to minimize hardware requirements for maximum reproduciblity.

For more models, see https://ollama.com/library, https://huggingface.co/models, and https://civitai.com/.

## General experimental workflow

### For bash projects

~~~
# Create a project directory and navigate to it
$ mkdir my_project && cd $_

# Create a script to perform needed installs, etc.
$ nano init.sh

# Execute the script
$ ./init.sh

# Do the experiment
~~~

### For python projects

~~~
# Create a virtual environment
$ python -m venv my_ai_project

# Activate it
$ source my_ai_project/bin/activate

# Navigate to it
$ cd my_ai_project

# Create a script to perform needed installs, etc.
$ nano init.sh

# Execute the script
$ ./init.sh

# Do the experiment

# Deactivate the VE
$ deactivate
~~~

## Text to text

~~~
# Pull model
$ ollama pull llama3.2

# Make request
$ ollama run llama3.2 "Explain the basics of machine learning."

# Example: pipe a text file into the prompt
$ ollama run llama3.2 "Summarize this article." < my_article.txt
~~~

## Text to code

~~~
# Pull model
$ ollama pull qwen2.5-coder

# Make request
$ ollama run qwen2.5-coder "Show quicksort algorithm in python."
~~~

## Text to image

~~~
# Create a virtual environment
$ python -m venv my_ai_project

# Activate it
$ source my_ai_project/bin/activate

# Install the requirements
$ pip install diffusers torch transformers accelerate

# Create a python script to run the generator:

from diffusers import StableDiffusionPipeline
import torch

# Fully disable safety checker
def dummy(images, **kwargs):
    return images, [False] * len(images)

model_id = "runwayml/stable-diffusion-v1-5"

# Force CPU and float32
pipe = StableDiffusionPipeline.from_pretrained(model_id, torch_dtype=torch.float32, safety_checker=None)
pipe.safety_checker = dummy
pipe = pipe.to("cpu")

# Prompt that won't trigger safety
prompt = "a serene landscape with mountains, lake, and sunrise, detailed painting"

# Generate and save
image = pipe(prompt).images[0]
image.save("output.png")
print("Image saved to output.png")

# Execute the script
$ python the_script.py
~~~

## Image to image

~~~
# Create a virtual environment and activate it
$ python -m venv img2img
$ source img2img/bin/activate

# Install needed packages
$ pip install diffusers transformers torch accelerate

# Create a python script to run the generator:

from diffusers import StableDiffusionImg2ImgPipeline
import torch
from PIL import Image

def dummy(images, **kwargs):
        return images, [False] * len(images)

model_id = "CompVis/stable-diffusion-v1-4"

pipe = StableDiffusionImg2ImgPipeline.from_pretrained(model_id, torch_dtype=torch.float32, safety_checker=None)
pipe.safety_checker = dummy
pip = pipe.to("cpu")

init_image = Image.open("~/temp/duck.jpg").convert("RGB")
prompt = "make it look like a duck boat"

image = pipe(prompt=prompt, image=init_image, strength=0.75, guidance_scale=7.5).images[0]
image.save("output.jpg")

# Execute the script
$ python the_script.py
~~~

## Image to text

~~~
# Pull model
$ ollama pull llava:7b

# Make request
$ ollama run llava:7b "What's in this image? ./duck.jpg"
~~~

## Background remover

~~~
# Create a virtual environment and activate it
$ python -m venv rembg
$ source rembg/bin/activate

# Install needed packages
$ pip install rembg onnxruntime click filetype watchdog aiohttp gradio asyncer

# Remove a background from an image
$ rembg -i {input_image_path} {output_image_path}
# Example:
$ rembg -i ~/temp/duck.jpg ~/temp/duck_no_background.jpg
~~~

## Models

### Qwen

- https://ollama.com/library/qwen3
- https://qwenlm.github.io/blog/qwen3/

`$ pip install diffusers torch transformers accelerate modelscope`

Generate.py

~~~
from modelscope import AutoModelForCausalLM, AutoTokenizer

model_name = "Qwen/Qwen3-30B-A3B"

# load the tokenizer and the model
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype="auto",
    device_map="auto"
)

# prepare the model input
prompt = "Give me a short introduction to large language model."
messages = [
    {"role": "user", "content": prompt}
]
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True,
    enable_thinking=True # Switch between thinking and non-thinking modes. Default is True.
)
model_inputs = tokenizer([text], return_tensors="pt").to(model.device)

# conduct text completion
generated_ids = model.generate(
    **model_inputs,
    max_new_tokens=32768
)
output_ids = generated_ids[0][len(model_inputs.input_ids[0]):].tolist() 

# parsing thinking content
try:
    # rindex finding 151668 (</think>)
    index = len(output_ids) - output_ids[::-1].index(151668)
except ValueError:
    index = 0

thinking_content = tokenizer.decode(output_ids[:index], skip_special_tokens=True).strip("\n")
content = tokenizer.decode(output_ids[index:], skip_special_tokens=True).strip("\n")

print("thinking content:", thinking_content)
print("content:", content)
~~~
