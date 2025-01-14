# 🔍🤖 Vision Agent

Vision Agent is a library that helps you utilize agent frameworks for your vision tasks.
Many current vision problems can easily take hours or days to solve, you need to find the
right model, figure out how to use it, possibly write programming logic around it to 
accomplish the task you want or even more expensive, train your own model. Vision Agent
aims to provide an in-seconds experience by allowing users to describe their problem in
text and utilizing agent frameworks to solve the task for them. Check out our discord
for updates and roadmaps!

## Getting Started
### Installation
To get started, you can install the library using pip:

```bash
pip install vision-agent
```

Ensure you have an OpenAI API key and set it as an environment variable:

```bash
export OPENAI_API_KEY="your-api-key"
```

### Vision Agents
You can interact with the agents as you would with any LLM or LMM model:

```python
>>> import vision_agent as va
>>> agent = VisionAgent()
>>> agent("How many apples are in this image?", image="apples.jpg")
"There are 2 apples in the image."
```

To better understand how the model came up with it's answer, you can also run it in
debug mode by passing in the verbose argument:

```python
>>> agent = VisionAgent(verbose=True)
```

You can also have it return the workflow it used to complete the task along with all
the individual steps and tools to get the answer:

```python
>>> resp, workflow = agent.chat_with_workflow([{"role": "user", "content": "How many apples are in this image?"}], image="apples.jpg")
>>> print(workflow)
[{"task": "Count the number of apples using 'grounding_dino_'.",
  "tool": "grounding_dino_",
  "parameters": {"prompt": "apple", "image": "apples.jpg"},
  "call_results": [[
    {
      "labels": ["apple", "apple"],
      "scores": [0.99, 0.95],
      "bboxes": [
        [0.58, 0.2, 0.72, 0.45],
        [0.94, 0.57, 0.98, 0.66],
      ]
    }
  ]],
  "answer": "There are 2 apples in the image.",
}]
```

### Tools
There are a variety of tools for the model or the user to use. Some are executed locally
while others are hosted for you. You can also ask an LLM directly to build a tool for
you. For example:

```python
>>> import vision_agent as va
>>> llm = va.llm.OpenAILLM()
>>> detector = llm.generate_detector("Can you build an apple detector for me?")
>>> detector("apples.jpg")
[{"labels": ["apple", "apple"],
  "scores": [0.99, 0.95],
  "bboxes": [
    [0.58, 0.2, 0.72, 0.45],
    [0.94, 0.57, 0.98, 0.66],
  ]
}]
```

| Tool | Description |
| --- | --- |
| CLIP | CLIP is a tool that can classify or tag any image given a set of input classes or tags. |
| ImageCaption| ImageCaption is a tool that can generate a caption for an image. |
| GroundingDINO | GroundingDINO is a tool that can detect arbitrary objects with inputs such as category names or referring expressions. |
| GroundingSAM | GroundingSAM is a tool that can detect and segment arbitrary objects with inputs such as category names or referring expressions. |
| DINOv | DINOv is a tool that can detect arbitrary objects with using a referring mask. |
| Crop | Crop crops an image given a bounding box and returns a file name of the cropped image. |
| BboxArea | BboxArea returns the area of the bounding box in pixels normalized to 2 decimal places. |
| SegArea | SegArea returns the area of the segmentation mask in pixels normalized to 2 decimal places. |
| BboxIoU | BboxIoU returns the intersection over union of two bounding boxes normalized to 2 decimal places. |
| SegIoU | SegIoU returns the intersection over union of two segmentation masks normalized to 2 decimal places. |
| BoxDistance | BoxDistance returns the minimum distance between two bounding boxes normalized to 2 decimal places. |
| MaskDistance | MaskDistance returns the minimum distance between two segmentation masks in pixel units |
| BboxContains | BboxContains returns the intersection of two boxes over the target box area. It is good for check if one box is contained within another box. |
| ExtractFrames | ExtractFrames extracts frames with motion from a video. |
| ZeroShotCounting | ZeroShotCounting returns the total number of objects belonging to a single class in a given image. |
| VisualPromptCounting | VisualPromptCounting returns the total number of objects belonging to a single class given an image and visual prompt. |
| VisualQuestionAnswering | VisualQuestionAnswering is a tool that can explain the contents of an image and answer questions about the image. |
| ImageQuestionAnswering | ImageQuestionAnswering is similar to VisualQuestionAnswering but does not rely on OpenAI and instead uses a dedicated model for the task. |
| OCR | OCR returns the text detected in an image along with the location. |

It also has a basic set of calculate tools such as add, subtract, multiply and divide.
