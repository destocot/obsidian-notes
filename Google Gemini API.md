- Gemini is built from the ground up for **multimodality** -- reasoning seamlessly across text, images, video, audio, and code.

# 3 Flavors of Gemini
- Ultra - highly-complex tasks
- Pro - scaling across a wide range of tasks
- Nano - on-device tasks

### Google AI Gemini API vs Google Cloud Vertex AI Gemini API
- Google AI Gemini API
	- Google AI Studio
	- API & SDK - Python | Node.js | Android (Kotlin/Java) | Swift | Go
	- Free tier - Yes
	- Enterprise support - no
-  Google Cloud Vertex AI Gemini API
	- Vertex AI Studio
	- API & SDK - Python | Node.js | Java | Go
	- Free tier - $300 Google Cloud credit for new users
	- Enterprise support - Yes

# Bard

[Google Bard](https://bard.google.com/chat)

- Have ability to access drafts (alternative responses) on your latest response.
- Has image prompting abilities
	- only one image per prompt (API can support more images)
- Has Google-It button, to find more external resources that confirm or refute a response.
- Can share prompt-responses
	- collaborators can continue the prompt
	- can export to google docs / email
- Can read messages out loud / give prompts with microphone
- Streaming Responses - Respond once completed | Respond in real time
- Can provide feedback

 - Google collects your Bard conversations, related product usage information, info about your location, and your feedback. (Some default behavior can be disabled)
## Features
- location specific prompting
- YouTube Video features
- Math Equations
- Visualizing Data
- Google Hotels & Google Flights
- Google Workspace Extension
	- Can respond about your e-mails.
	- Can respond about files in your google drive.
	- Can draft emails.
	- Can search google.
	- Can generate and export code.

---
# Prompt Engineering

 **Prompt Guidelines**

```
I'm a course creator that makes courses primarily on Udemy. I'm working on a course that teaches students about Bard, Google Gemini, the Google AI Studio, and Vertex AI, as well as some prompt engineering. Please generate me the content for my Udemy course landing page.

In your output I need:
- A course title
- A course subtitle
- 5-7 learning objectives
- 2 paragraphs of text explaining what the course covers

I'm hoping this marketing copy will appeal to my existing students (who largely know some coding) but also students who are new to me and my courses and want to learn about AI and LLMs.
```

**The Input**
- Question
- Task
- Completion
- Entity

 **The Context**
**Examples**
```
Generate me a color palette with 2 colors based on an input sentence.

Prompt: "We sat by the fire as the snow fell"
Your Reply: #C25636, #FDFEFE
Prompt: "Tropical waves and sand"
Your Reply: #40E0D0, #FDC593

Prompt: "aurora against a dark sky"
Your Reply:
```

**Output Format**
- Markdown
- CSV
- JSON
- Table

**Image Prompting Tips**

---
# Google AI Studio

- Browser Based IDE
- Made for Prototyping
- Free - 60 QPM (queries per minute)

```
https://makersuite.google.com/app
```

## Gemini Pro
- Input: text
- Output: text
- Input token limit: 30720
- Output token limit: 2048
- Rate Limit: 60 requests per minute

- Using {{ input }} test inputs
	- adjust prompt as needed
	- can have multiple test inputs

```
Extract the first name, last name, middle initial, and suffix from this name: {{ name }}. Format the data as a ﻿{{ format }}﻿
```

## Structured Prompts
- 0-shot Prompting: no examples
- 1-shot Prompting: 1 example
- n-shot  (few-shot) Prompting: n examples

- Can provide optional tone and style instructions
- Can provide up to 500 examples
- can provide multiple inputs and outputs

- Import data from a CSV (actions > import examples)

> I am Udemy course creator and teach mostly technical topics. Given a list of topics covered in an online course, generate the title, subtitle, and description keeping with the tone and style of the provided examples.

## Preview Prompt
- `< >` Get code
- Preview button

## Chat Prompts
- Simulates a back and forth conversation with a model
- Can create examples,
	- if there is an undesirable response, you can add it as an example and change the response (rinse and repeat)

## Gemini Pro Vision



