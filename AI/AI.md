
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

## Text to sql 

~~~
$ pip install ollama

# Create a python script:

r = ollama.generate(
    model='duckdb-nsql',
    system='''Here is the database schema that the SQL query will run on:
CREATE TABLE taxi (
    VendorID bigint,
    tpep_pickup_datetime timestamp,
    tpep_dropoff_datetime timestamp,
    passenger_count double,
    trip_distance double,
    fare_amount double,
    extra double,
    tip_amount double,
    tolls_amount double,
    improvement_surcharge double,
    total_amount double,
);''',
    prompt='get all columns ending with _amount from taxi table',
)

print(r['response'])

# Execute the script
$ python the_script.py

# Will output something like this:
# SELECT COLUMNS('.*_amount') FROM taxi;
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

## Motion detection in animated GIFs

- A possible use is to combine sequential images into an animated GIF.

Key Features:

- Frame Extraction: Loads GIF frames and converts them to grayscale for processing
- Motion Detection: Compares consecutive frames using absolute difference and thresholding
- Noise Reduction: Uses morphological operations to clean up motion masks
- Object Detection: Finds and filters motion contours based on size
- Statistical Analysis: Provides detailed motion statistics and summaries
- Visualization: Creates plots showing motion masks, overlays, and timeline graphs

How It Works:

- Frame Comparison: Calculates pixel differences between consecutive frames
- Thresholding: Applies a threshold to identify significant changes
- Morphological Filtering: Removes noise using opening and closing operations
- Contour Analysis: Finds connected motion regions and filters by minimum area
- Statistics: Tracks motion percentage, object counts, and activity levels

Required dependencies:

`$ pip install opencv-python pillow matplotlib numpy`

~~~python
import cv2
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt
from typing import List, Tuple, Optional
import os

class GifMotionDetector:
    def __init__(self, threshold: float = 30.0, min_area: int = 100):
        """
        Initialize the GIF motion detector.
        
        Args:
            threshold: Pixel difference threshold for motion detection
            min_area: Minimum contour area to consider as motion
        """
        self.threshold = threshold
        self.min_area = min_area
        self.frames = []
        self.motion_data = []
    
    def load_gif(self, gif_path: str) -> List[np.ndarray]:
        """Load GIF frames and convert to grayscale arrays."""
        if not os.path.exists(gif_path):
            raise FileNotFoundError(f"GIF file not found: {gif_path}")
        
        gif = Image.open(gif_path)
        frames = []
        
        try:
            while True:
                # Convert frame to RGB then to grayscale
                frame = gif.convert('RGB')
                frame_array = np.array(frame)
                gray_frame = cv2.cvtColor(frame_array, cv2.COLOR_RGB2GRAY)
                frames.append(gray_frame)
                gif.seek(len(frames))  # Move to next frame
        except EOFError:
            pass  # End of GIF
        
        self.frames = frames
        print(f"Loaded {len(frames)} frames from {gif_path}")
        return frames
    
    def detect_motion_between_frames(self, frame1: np.ndarray, frame2: np.ndarray) -> Tuple[np.ndarray, int, List]:
        """
        Detect motion between two consecutive frames.
        
        Returns:
            motion_mask: Binary mask showing motion areas
            motion_pixels: Number of pixels with motion
            contours: List of motion contours
        """
        # Calculate absolute difference
        diff = cv2.absdiff(frame1, frame2)
        
        # Apply threshold
        _, motion_mask = cv2.threshold(diff, self.threshold, 255, cv2.THRESH_BINARY)
        
        # Apply morphological operations to reduce noise
        kernel = np.ones((3, 3), np.uint8)
        motion_mask = cv2.morphologyEx(motion_mask, cv2.MORPH_OPEN, kernel)
        motion_mask = cv2.morphologyEx(motion_mask, cv2.MORPH_CLOSE, kernel)
        
        # Find contours
        contours, _ = cv2.findContours(motion_mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
        
        # Filter contours by area
        filtered_contours = [c for c in contours if cv2.contourArea(c) >= self.min_area]
        
        # Count motion pixels
        motion_pixels = np.sum(motion_mask > 0)
        
        return motion_mask, motion_pixels, filtered_contours
    
    def analyze_gif_motion(self) -> List[dict]:
        """Analyze motion throughout the entire GIF."""
        if len(self.frames) < 2:
            raise ValueError("Need at least 2 frames to detect motion")
        
        motion_data = []
        
        for i in range(len(self.frames) - 1):
            frame1 = self.frames[i]
            frame2 = self.frames[i + 1]
            
            motion_mask, motion_pixels, contours = self.detect_motion_between_frames(frame1, frame2)
            
            # Calculate motion statistics
            total_pixels = frame1.shape[0] * frame1.shape[1]
            motion_percentage = (motion_pixels / total_pixels) * 100
            
            frame_data = {
                'frame_index': i,
                'motion_pixels': motion_pixels,
                'motion_percentage': motion_percentage,
                'num_objects': len(contours),
                'motion_mask': motion_mask,
                'contours': contours
            }
            
            motion_data.append(frame_data)
        
        self.motion_data = motion_data
        return motion_data
    
    def get_motion_summary(self) -> dict:
        """Get summary statistics of motion in the GIF."""
        if not self.motion_data:
            return {}
        
        motion_percentages = [frame['motion_percentage'] for frame in self.motion_data]
        object_counts = [frame['num_objects'] for frame in self.motion_data]
        
        summary = {
            'total_frames': len(self.frames),
            'frames_analyzed': len(self.motion_data),
            'avg_motion_percentage': np.mean(motion_percentages),
            'max_motion_percentage': np.max(motion_percentages),
            'min_motion_percentage': np.min(motion_percentages),
            'avg_objects_detected': np.mean(object_counts),
            'max_objects_detected': np.max(object_counts),
            'frames_with_motion': sum(1 for p in motion_percentages if p > 0),
            'most_active_frame': np.argmax(motion_percentages),
            'least_active_frame': np.argmin(motion_percentages)
        }
        
        return summary
    
    def visualize_motion(self, frame_index: int = 0, save_path: Optional[str] = None):
        """Visualize motion detection for a specific frame transition."""
        if frame_index >= len(self.motion_data):
            raise ValueError(f"Frame index {frame_index} out of range")
        
        frame_data = self.motion_data[frame_index]
        frame1 = self.frames[frame_index]
        frame2 = self.frames[frame_index + 1]
        motion_mask = frame_data['motion_mask']
        
        # Create visualization
        fig, axes = plt.subplots(2, 2, figsize=(12, 10))
        
        # Original frames
        axes[0, 0].imshow(frame1, cmap='gray')
        axes[0, 0].set_title(f'Frame {frame_index}')
        axes[0, 0].axis('off')
        
        axes[0, 1].imshow(frame2, cmap='gray')
        axes[0, 1].set_title(f'Frame {frame_index + 1}')
        axes[0, 1].axis('off')
        
        # Motion mask
        axes[1, 0].imshow(motion_mask, cmap='hot')
        axes[1, 0].set_title('Motion Mask')
        axes[1, 0].axis('off')
        
        # Motion overlay
        motion_overlay = frame2.copy()
        motion_overlay = cv2.cvtColor(motion_overlay, cv2.COLOR_GRAY2RGB)
        
        # Draw contours on overlay
        for contour in frame_data['contours']:
            cv2.drawContours(motion_overlay, [contour], -1, (255, 0, 0), 2)
        
        axes[1, 1].imshow(motion_overlay)
        axes[1, 1].set_title(f'Motion Detected ({frame_data["motion_percentage"]:.1f}%)')
        axes[1, 1].axis('off')
        
        plt.tight_layout()
        
        if save_path:
            plt.savefig(save_path, dpi=150, bbox_inches='tight')
        
        plt.show()
    
    def plot_motion_timeline(self, save_path: Optional[str] = None):
        """Plot motion activity over time."""
        if not self.motion_data:
            print("No motion data available. Run analyze_gif_motion() first.")
            return
        
        frame_indices = [data['frame_index'] for data in self.motion_data]
        motion_percentages = [data['motion_percentage'] for data in self.motion_data]
        object_counts = [data['num_objects'] for data in self.motion_data]
        
        fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(12, 8))
        
        # Motion percentage over time
        ax1.plot(frame_indices, motion_percentages, 'b-', linewidth=2)
        ax1.set_ylabel('Motion Percentage (%)')
        ax1.set_title('Motion Activity Over Time')
        ax1.grid(True, alpha=0.3)
        
        # Number of objects over time
        ax2.plot(frame_indices, object_counts, 'r-', linewidth=2)
        ax2.set_xlabel('Frame Index')
        ax2.set_ylabel('Number of Moving Objects')
        ax2.set_title('Moving Objects Count Over Time')
        ax2.grid(True, alpha=0.3)
        
        plt.tight_layout()
        
        if save_path:
            plt.savefig(save_path, dpi=150, bbox_inches='tight')
        
        plt.show()

# Example usage
def main():
    # Initialize detector
    detector = GifMotionDetector(threshold=25.0, min_area=50)
    
    # Example usage (you'll need to replace with actual GIF path)
    gif_path = "example.gif"
    
    try:
        # Load and analyze GIF
        print("Loading GIF...")
        detector.load_gif(gif_path)
        
        print("Analyzing motion...")
        motion_data = detector.analyze_gif_motion()
        
        # Print summary
        summary = detector.get_motion_summary()
        print("\n=== Motion Detection Summary ===")
        print(f"Total frames: {summary['total_frames']}")
        print(f"Average motion: {summary['avg_motion_percentage']:.2f}%")
        print(f"Maximum motion: {summary['max_motion_percentage']:.2f}%")
        print(f"Frames with motion: {summary['frames_with_motion']}")
        print(f"Average objects detected: {summary['avg_objects_detected']:.1f}")
        print(f"Most active frame: {summary['most_active_frame']}")
        
        # Visualize motion for most active frame
        most_active = summary['most_active_frame']
        detector.visualize_motion(most_active)
        
        # Plot motion timeline
        detector.plot_motion_timeline()
        
    except FileNotFoundError:
        print("Please provide a valid GIF file path to test the motion detector.")
        print("You can download sample animated GIFs from sites like giphy.com")

if __name__ == "__main__":
    main()
~~~
