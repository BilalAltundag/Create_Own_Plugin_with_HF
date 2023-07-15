# Hugging Face Plugin for Streamlined Development
## Hugging Face Plugin

Welcome to the Hugging Face Plugin documentation! This plugin is designed to supercharge your development workflow, enabling you to accomplish tasks faster and more efficiently. With a single language model, you can build a wide range of tools and automate repetitive coding tasks.

## Installation

To get started, follow these steps:

Clone the Hugging Face Plugin repository:

'''python
git clone https://github.com/CASIA-IVA-Lab/FastSAM.git
'''

Download the pretrained weights:

'''
wget https://huggingface.co/spaces/An-619/FastSAM/resolve/main/weights/FastSAM.pt
'''

Install the required dependencies:

'''
pip install -r FastSAM/requirements.txt
pip install git+https://github.com/openai/CLIP.git
'''

## Setup

Before diving into the amazing features of this plugin, ensure that you have set up your environment correctly. Follow these steps:

Set the transformers_version to the desired version (e.g., "v4.29.0") using the provided dropdown menu.

Install the necessary libraries and dependencies:

'''
pip install huggingface_hub>=0.14.1 git+https://github.com/huggingface/transformers@$transformers_version -q diffusers accelerate datasets torch soundfile sentencepiece opencv-python openai
'''

## Authenticate with your Hugging Face account:

'''
from huggingface_hub import notebook_login
notebook_login()
'''

Initialize the agent by providing the required credentials:

## For StarCoder (HF Token):

'''
from transformers.tools import HfAgent
agent = HfAgent("https://api-inference.huggingface.co/models/bigcode/starcoder", token=token)
print("StarCoder is initialized 💪")
'''

## For OpenAssistant (HF Token):

'''
from transformers.tools import HfAgent
agent = HfAgent(url_endpoint="https://api-inference.huggingface.co/models/OpenAssistant/oasst-sft-4-pythia-12b-epoch-3.5", token=token)
print("OpenAssistant is initialized 💪")
'''

## For OpenAI (API Key):

'''
from transformers.tools import OpenAiAgent
pswd = getpass.getpass('OpenAI API key:')
agent = OpenAiAgent(model="text-davinci-003", api_key=pswd)
print("OpenAI is initialized 💪")
'''

## Examples
### Customer Information Agent
You can create a customer information agent using a DataFrame. Here's an example:

'''
import pandas as pd

data = [
    {"Name": "John", "Last Name": "Smith", "Working Status": "Employed", "Rating Score": 4},
    {"Name": "Emma", "Last Name": "Johnson", "Working Status": "Employed", "Rating Score": 3},
    {"Name": "Alex", "Last Name": "Brown", "Working Status": "Unemployed", "Rating Score": 2},
    {"Name": "Olivia", "Last Name": "Davis", "Working Status": "Employed", "Rating Score": 5},
    {"Name": "Olivia", "Last Name": "Brown", "Working Status": "Employed", "Rating Score": 6},
    {"Name": "Liam", "Last Name": "Wilson", "Working Status": "Employed", "Rating Score": 1}
]

df = pd.DataFrame(data)

def get_customer_info(name, last_name=None):
    if last_name is None:
        filtered_df = df[df['Name'] == name]
    else:
        filtered_df = df[(df['Name'] == name) & (df['Last Name'] == last_name)]

    if len(filtered_df) == 0:
        return "No customer found."
    else:
        return filtered_df.to_dict('records')
'''

### Fast Sam Agent
You can perform image segmentation using the Fast Sam model. Here's an example:

'''
from PIL import Image

class FastSamTool(Tool):
    name = "FastSamTool"
    description = ("Returns image segmentation of an image. It takes 'image' and 'text_prompt' as inputs and returns the segmented image.")

    inputs = ["text", "text"]
    outputs = ["image"]

    def __call__(self, image: str, text_prompt: str):
        command = f"python FastSAM/Inference.py --model_path FastSAM.pt --img_path ./images/{image} --text_prompt '{text_prompt}' --imgsz 1024"
        !{command}

        image = Image.open(f"./output/{image}")
        return image
'''

## Conclusion
With the Hugging Face Plugin, you have the power to streamline your development process and automate various tasks using a single language model. Explore the provided examples and unleash your creativity to build your own efficient tools. Happy coding!

Please note that the above examples are just a glimpse of what you can achieve with the Hugging Face Plugin. For more details and additional functionalities, refer to the complete documentation in the GitHub repository.
