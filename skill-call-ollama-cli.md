# Skill - Call Ollama CLI

---

This recipe is for creating a skill that will call the Ollama CLI and return structured data for your agent. This example is for offloading machine vision identification to a local model. The recipe assumes you have Ollama installed and have pulled down a vision capable model.

 

## Step 1

Place some image files into a directory that your coding agent will have access to.

 

## Step 2

Use Ollama's create model file so that you can put instructions in the system prompt directing the model to output structured data in JSON format. This will allow you to keep the command line succinct.

Example Modelfile:

```
FROM qwen3.5:4b

SYSTEM """
Respond with a valid JSON string only, without any additional text or explanations. The category can only be one of people, food, nature, activity, transportation, technology, or other. The general_short_description should be no more than 4 words, and detailed_description should be no more than 10 words. The output should adhere to the following structure:
{
    "vision_object_detection": [
        {
            "category": "<string>",
            "general_short_description": "<string>",
            "detailed_description": "<string>"
        }
    ]
}
Do not include any introductory phrases or sentences in your response. Only provide the JSON object.
"""
```

Here is the Ollama command for creating model reference that uses the above system prompt:

`ollama create qwen3.5:4b_vision_json -f ./Modelfile`

 

## Step 3

Place the skill or agent markdown file into either your project workspace or your coding system's global skills directory. Here's a SKILL.md file that can be used in Claude Code:

````
---
name: object-identify
description: Identify objects in images
---

**object-identify** is a skill that allows utilizes the Ollama CLI to identify objects in images. The CLI is set to return JSON data with the category, general_short_description, and a detailed_description. The CLI call in the bash below has a variable for the image path and name.
```bash
ollama run qwen3.5:4b_vision_json --think=false "What is in this image? <variable>"
```
````

**Note:** the above tag-like <variable> for your agent to decide what to put there.

 

## Step 4

Restart your coding agent so the new skill will become available. You can use the skill like this:

```
/object-identify go through all the images in the images dir, identify the images, and organize them by copying them into new directories by category name
```
