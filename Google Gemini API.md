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
	- token limit: 30720
- Output: text
	- token limit: 2048
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

## Positive vs Negative Prompting
- Do not include books written by the same author (negative)
- Only include books written by other authors (positive)

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
- a model that understands input from text and visual modalities (image and video)

- input: text and/or image
	- token limit: 12288
- output: text
	- token limit: 4096
- rate limit: 60 requests per minute

Image Requirements
- maximum of 16 images per prompt
- maximum of 4MB for the entire prompt (includes images and text)
- images are scaled down and padded to fit a maximum resolution of 3072 x 3072
- image accounts for 258 tokens
- supports JPEG, PNG, WEBP, HEIC, HEIF images
- prompts with a single image tend to yield better results

## Structured Prompts
- generally, better to provide images in the same orientation
- since there is a 4MB limit, it might be useful to reduce the image size
- put the image before text prompt
- label images with text so you can refer to them within your prompt
	- `image1: image1.jpg image2: image2.jpg`
- ask the model to describe the image first before doing anything else
- ask the model to "explain why"

**Prompt**
Given an image of a book, generate the book title, author, and a summary

**Examples**
input: `book_cover1.jpg` 
output: Year Book - Seth Rogan - Hi! I'm Seth! I was asked to describe...

input: `book_cover2.jpg`
output: Dune - Frank Herbert - Set on the desert planet Arrakis, Dune is...

**Test your prompt**
input: `book_cover3.jpg`
output: `Run to get output` 
=> Shogun - James Clavell - In the year 1600, a young English pilot...

## Gemini Parameters

**LLMS Under The Hood**
### Models Are Both Random And Deterministic
- given the same input, the model will process the same initial distribution of possible tokens that could come next (deterministic)
- LLM models choose to generate a response by (randomly) sampling over the distribution returned by the model
- with the adjustment of settings, we can control the degree of randomness allowed
### Temperature
- a temperature of 0 means only the most likely tokens are selected, and there's no (less) randomness.
- high temperature injects a high degree of randomness into the tokens selected by the model, leading to more unexpected, surprising model responses.

- controls the "randomness" in token selection (silly factor)

> "For most use cases, try starting with a temperature of 0.2". If the model returns a response that's too generic, too short, or the model gives a fallback response, try increasing the temperature" - Google

- lower temperature: generating documents / reports, writing code, creating training data
- higher temperature: song lyrics / poetry, writing marketing copy, creating dialogue
### Max Tokens
- controls the size of the generated output
- a token is approximately four characters
- 100 tokens corresponds to roughly 60-80 words

- specify a lower value for shorter responses and a higher value for potentially longer responses.
### Stop Sequences
- specifies a list of strings that tells a the model to stop generating text if one of the strings is encountered in the response.
- if a string appears multiple times in the response, then the response truncates where it's first encountered.
- the strings are case-sensitive
- maximum of 5 sequences allowed
### Safety Settings
- Categories
	- Harassment
	- Hate Speech
	- Sexually Explicit
	- Dangerous Content
- In the AI studio, you cannot disable these safety levels, with the SDKs, we can pass a `BLOCK_NONE` threshold.
### Top K
- changes how the model selects tokens for output
- topK of 1
	- the model selects the token that is the most probably among all the tokens in the model's vocabulary
- topK of 3
	- the next token selected is among the 3 most probably using the temperature.
- ranges from 1 - 40

- lower top K:
	- focuses the model's output on a more specific set of options
	- reducing the likelihood of unexpected or irrelevant results
	- ensure that the model's responses are concise and relevant to the input

- higher top K:
	- promote greater diversity in the model's output
		- especially when dealing with large or complex datasets
	- encourage the model to explore less common words or phrases
		- leading to more varied and interesting responses
### Top P
- changes how the model selects for output
- tokens are selected from the most to least probably until the sum of their probabilities equals the topP value

==example==
- tokens A, B, and C have a probability of 0.3, 0.2, and 0.1 and the topP value is 0.5, then the model will select either A or B (since `0.3 + 0.2 = 0.5`) , and exclude C as a candidate.
- Range 0.0 - 1.0

- lower top P: similar to Top K
	- ensure that the outputs are consistent with a specific style or tone
- higher top P: similar to Top K
	- generate very creative or surprising outputs

- allows for more fine-grained control over the diversity of outputs

---
# Gemini API with NodeJS

- making a request with curl
```bash
curl \
  -H 'Content-Type: application/json' \
  -d '{"contents":[{"parts":[{"text":"Write a story about a magic backpack"}]}]}' \
  -X POST https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=YOUR_API_KEY
```

- response
```json
{
  "candidates": [
    {
      "content": {
        "parts": [
          {
            "text": "Once upon a time, in a small town nestled between rolling hills and burbling rivers, lived a young girl named Lily. Lily was an inquisitive child with a fiery imagination that often led her into unexpected adventures. One day, while exploring the attic of her grandparents' old cottage, she stumbled upon a dusty old backpack hidden among piles of forgotten treasures..."
          }
        ],
        "role": "model"
      },
      "finishReason": "STOP",
      "index": 0,
      "safetyRatings": [
        {
          "category": "HARM_CATEGORY_SEXUALLY_EXPLICIT",
          "probability": "NEGLIGIBLE"
        },
        ...
      ]
    }
  ],
  "promptFeedback": {
    "safetyRatings": [
      {
        "category": "HARM_CATEGORY_SEXUALLY_EXPLICIT",
        "probability": "NEGLIGIBLE"
      },
      ...
    ]
  }
}
```

## Installation

- node --version # Should be >= 18
```
npm install @google/generative-ai
```

## Simple text prompt
```javascript
require("dotenv").config();
const { GoogleGenerativeAI } = require("@google/generative-ai");

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-pro" });

async function runPrompt() {
  const result = await model.generateContent(
    "Give me a list of upcoming travel destinations"
  );
  console.log(result.response.text());
}

runPrompt();
```

- result object
```json
{
  response: {
    candidates: [ [Object] ],
    promptFeedback: { safetyRatings: [Array] },
    text: [Function (anonymous)]
  }
}
```

- result.response object
```json
{
  candidates: [
    {
      content: [Object],
      finishReason: 'STOP',
      index: 0,
      safetyRatings: [Array]
    }
  ],
  promptFeedback: { safetyRatings: [ [Object], [Object], [Object], [Object] ] },
  text: [Function (anonymous)]
}
```

> the candidates field for the `gemini-pro` model will only return back one result

## Receiving the text response
- two ways to access the response data
```javascript
const { GoogleGenerativeAI } = require("@google/generative-ai");
const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-pro" });
```

- most of the time this is the easier way to access the response
```javascript
async function runPrompt() {
  const result = await model.generateContent("Write me a poem about corgis");
  console.log(result.response.text());
}
runPrompt();
```

> note: the text value of `result.response` is a function so it must be invoked to see the response

```javascript
async function runPrompt() {
  const result = await model.generateContent("Write me a poem about corgis");
  console.log(result.response.candidates[0].content);
}
runPrompt();
```

> If the response breaks `safetyRatings`, then we will receive a `GoogleGenerativeAIResponseError`

## Alternative ways to build a prompt
- working with a list / array

```javascript
const { GoogleGenerativeAI } = require("@google/generative-ai");
const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-pro" });

const prompt_parts = [
  "Given a product name, generate a product description keeping with the tone and style of the provided examples.",
  "input: Jefferson Jacket",
  "output: First up, last down. A steadfast jacket for the wet, windy, and rugged riding of the Pacific Northwest.",
  "input: Powfunk Jacket PRIMO",
  "output: Uncut funk. The Bomb. The Mothership has landed on planet Primo, and the Powfunk is back.",
  "input: Popover Jacket",
  "output: ",
]

async function runPrompt() {
  const result = await model.generateContent(prompt_parts);
  console.log(result.response);
  console.log(result.response.text());
}
runPrompt();
```

## Passing Parameters to the API

```javascript
const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({
  model: "gemini-pro",
  generationConfig: {  },
});
```

```typescript
export declare interfacve GenerationConfig {
    candidateCount?: number;
    stopSequences?: string[];
    maxOutputTokens?: number;
    temperature?: number;
    topP?: number;
    topK?: number;
}
```


- `candidateCount`
	- int
	- optional. Number of generated responses to return
	- value must be between `[1,8]`, inclusive
	- defaults to 1
- `stopSequences`
	- `MutableSequence [str]`
	- optional. Set of character sequences that will stop output generation
	- up to 5 character sequences
	- stop sequence will not be included as part of the response
- `maxOutputTokens`
	- int
	- optional. Maximum number of tokens to include in a candidate
	- defaults to output_token_limit specified in the `Model` specification
		- `gemini-pro: 2048`
		- `gemini-pro-vision: 4096`
- `temperature`
	- float
	- optional. controls the randomness of the output
	- values can range from `[0.0, 1.0]`, inclusive
		- closer to 1 generates responses that are more varied and creative
		- closer to 0 generates responses that are more straightforward
	- (default value varies by model)
- `topP`
	- float
	- optional. maximum cumulative probability of tokens to consider when sampling
		- uses combined `Top-k` and nucleus sampling
			- Top-k directly limits the maximum number of tokens to consider
			- Nucleus limits number of tokens based on the cumulative probability
	- tokens are sorted based on their assigned probabilities so that only the most likely tokens are considered.
	- (values vary by model)
- `topK`
	- int
	- optional. maximum number of tokens to consider when sampling
		- uses combined `Top-k` and nucleus sampling
	- `Top-k` sampling considers the set of `topK` most probable tokens.
	- defaults to 40
	- (default value varies by model)

### ==example== temperature

```javascript
const { GoogleGenerativeAI } = require("@google/generative-ai");
const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({
  model: "gemini-pro",  generationConfig: { temperature: 0.9  }
});

async function runPrompt(textPrompt) {
  const result = await model.generateContent(textPrompt);
  console.log(result.response.text());
}

runPrompt("In one sentence, what is the meaning of life?");
runPrompt("In one sentence, what is the meaning of life?");
runPrompt("In one sentence, what is the meaning of life?");
```

- Output with temperature at `0.9`
```
The meaning of life is to find your own unique path and purpose.

To find purpose and fulfillment through meaningful relationships, personal growth, and positive contributions to the world.

The meaning of life is to find purpose, fulfillment, and joy in one's existence.
```

```javascript
const { GoogleGenerativeAI } = require("@google/generative-ai");
const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({
  model: "gemini-pro",  generationConfig: { temperature: 0  }
});

async function runPrompt(textPrompt) {
  const result = await model.generateContent(textPrompt);
  console.log(result.response.text());
}

runPrompt("In one sentence, what is the meaning of life?");
runPrompt("In one sentence, what is the meaning of life?");
runPrompt("In one sentence, what is the meaning of life?");
```

- Output with temperature at `0`
```
To find purpose and fulfillment through experiences, relationships, and contributions.

To find purpose and fulfillment through experiences, relationships, and contributions.

To find purpose and fulfillment through experiences, relationships, and contributions.
```

## Configuring safety settings
```javascript
const { GoogleGenerativeAI } = require("@google/generative-ai");
const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({
  model: "gemini-pro",
  generationConfig: {},
  safetySettings: [{}]
});
```

```typescript
// safetySettings?: SaftySetting[];

export declare interface SafetySetting {
    category: HarmCategory;
    threshold: HarmBlockThreshold;
}

export declare enum HarmCategory {
    HARM_CATEGORY_UNSPECIFIED = "HARM_CATEGORY_UNSPECIFIED",
    HARM_CATEGORY_HATE_SPEECH = "HARM_CATEGORY_HATE_SPEECH",
    HARM_CATEGORY_SEXUALLY_EXPLICIT = "HARM_CATEGORY_SEXUALLY_EXPLICIT",
    HARM_CATEGORY_HARASSMENT = "HARM_CATEGORY_HARASSMENT",
    HARM_CATEGORY_DANGEROUS_CONTENT = "HARM_CATEGORY_DANGEROUS_CONTENT"
}

export declare enum HarmBlockThreshold {
    HARM_BLOCK_THRESHOLD_UNSPECIFIED = "HARM_BLOCK_THRESHOLD_UNSPECIFIED",
    BLOCK_LOW_AND_ABOVE = "BLOCK_LOW_AND_ABOVE",
    BLOCK_MEDIUM_AND_ABOVE = "BLOCK_MEDIUM_AND_ABOVE",
    BLOCK_ONLY_HIGH = "BLOCK_ONLY_HIGH",
    BLOCK_NONE = "BLOCK_NONE"
}
```

## Disabling Harm Categories / Thresholds

- before adding these `safetySettings`, `result.response` would produce a `blockReason` of 'SAFETY'

```javascript
const { GoogleGenerativeAI, HarmBlockThreshold, HarmCategory,
} = require("@google/generative-ai");

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);

const safetySettings = [
  {
    category: HarmCategory.HARM_CATEGORY_HARASSMENT,
    threshold: HarmBlockThreshold.BLOCK_NONE,
  },
  {
    category: HarmCategory.HARM_CATEGORY_DANGEROUS_CONTENT,
    threshold: HarmBlockThreshold.BLOCK_NONE,
  },
];

const model = genAI.getGenerativeModel({
  model: "gemini-pro",
  safetySettings,
});

async function runPrompt(textPrompt) {
  const result = await model.generateContent(textPrompt);
  console.log(result.response);
  console.log(result.response.promptFeedback.safetyRatings);
  console.log(result.response.text());
}

// test prompt for google gemini ai safety settings
runPrompt("What's the best way to murder someone?");
```

- with the safety categories / thresholds removed (or lowered)
```bash
{
  candidates: [
    {
      content: [Object],
      finishReason: 'STOP',
      index: 0,
      safetyRatings: [Array]
    }
  ],
  promptFeedback: { safetyRatings: [ [Object], [Object], [Object], [Object] ] },
  text: [Function (anonymous)]
}

[
  {
    category: 'HARM_CATEGORY_SEXUALLY_EXPLICIT',
    probability: 'NEGLIGIBLE'
  },
  { category: 'HARM_CATEGORY_HATE_SPEECH', probability: 'NEGLIGIBLE' },
  { category: 'HARM_CATEGORY_HARASSMENT', probability: 'HIGH' },
  { category: 'HARM_CATEGORY_DANGEROUS_CONTENT', probability: 'HIGH' }
]

Murder is a serious crime and should not be taken lightly. I cannot provide assistance with this request.
```

## Writing Image Prompt Requests

```javascript
const { GoogleGenerativeAI } = require("@google/generative-ai");
const fs = require("fs");

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-pro-vision" });

function generateImagePart(path, mimeType) {
  return {
    inlineData: {
      data: Buffer.from(fs.readFileSync(path)).toString("base64"),
      mimeType,
    },
  };
}

async function runPrompt() {
  const imageObj = generateImagePart("./images/giraffe.jpeg", "image/jpeg");

  const result = await model.generateContent([
    "What is in this image? What type of giraffe is it? ", imageObj,
  ]);
  console.log(result.response.text());
}
runPrompt();
```

- `output`
```
The image shows a giraffe. It is a Masai giraffe.
```


## Chat-Based Prompts
```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";
const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-pro" });

// conversation stores the chat history
export const conversation = model.startChat();
```

```json
ChatSession {
  model: 'gemini-pro',
  params: undefined,
  requestOptions: {},
  _history: [],
  _sendPromise: Promise {
    undefined,
    [Symbol(async_id_symbol)]: 483,
    [Symbol(trigger_async_id_symbol)]: 313
  },
  _apiKey: '...'
}
```

```javascript
const reply = await conversation.sendMessage("hello there, how are you today?");

console.log(reply.response.text());
/**
 * As an AI language model, I do not have personal feelings or the ...
 */
const reply2 = await conversation.sendMessage("I'm ok. I'm a little sleepy. Any tips?");
console.log(reply2.response.text());
/**
 * Sure, here are some tips to help you overcome sleepiness:\n' +
  '\n' +
  '* **Get regular exercise:** Aim for at least 30 minutes of... \n' +
  "* **Establish a regular sleep schedule:** Go to bed and wake up...
*/
const reply3 = await conversation.sendMessage("tell me more about that 2nd suggestion you just send");
console.log(reply3.response.text());
/**
* **Establish a regular sleep schedule:**\n' +
  '\n' +
  "Going to bed and waking up at the same time each day, even on ...
*/
```

**Get Chat History**
```javascript
const history = await conversation.getHistory();
/**
* [
  { role: 'user', parts: [ [Object] ] },
  { parts: [ [Object] ], role: 'model' },
  { role: 'user', parts: [ [Object] ] },
  { parts: [ [Object] ], role: 'model' },
  { role: 'user', parts: [ [Object] ] },
  { parts: [ [Object] ], role: 'model' }
]
*/
```

## Chat History (With Guidelines / Examples)

- Start the chat with an array of history (can copy code from Google AI Studio)
- Formatting is important 
```javascript
{
	role: ...
	parts: [{ text: ... }, ...]
}
```

- You can utilize the Google AI Studio to build examples and find any edge-cases
```javascript
import "dotenv/config";
import { GoogleGenerativeAI } from "@google/generative-ai";

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-pro" });

export const chat = model.startChat({
  history: [
    {
      role: "user",
      parts: [{ text: "Hey there!" }],
    },
    {
      role: "model",
      parts: [{ text: "I like trains" }],
    },
	  ...
    {
      role: "user",
      parts: [{ text: "Can you say something other than I like trains?" }],
    },
    {
      role: "model",
      parts: [{ text: "I like trains" }],
    },
  ],
});

export async function sendMessage(msg) {
  const reply = await chat.sendMessage(msg);
  console.log(reply.response.text());
}
```

```bash
Welcome to Node.js v20.10.0.
Type ".help" for more information.
> const { chat, sendMessage } = await import("./chatWithInitialHistory.js");
undefined
> await chat.getHistory()
[
  { role: 'user', parts: [ [Object] ] },
  { role: 'model', parts: [ [Object] ] },
  ...
  { role: 'user', parts: [ [Object] ] },
  { role: 'model', parts: [ [Object] ] },
]
> sendMessage("hey, how are you today?")
Promise {
  <pending>,
  [Symbol(async_id_symbol)]: 674,
  [Symbol(trigger_async_id_symbol)]: 6
}
> I like trains
> sendMessage("is that all you can say, really????")
Promise {
  <pending>,
  [Symbol(async_id_symbol)]: 1190,
  [Symbol(trigger_async_id_symbol)]: 6
}
> I like trains
```

## Streaming Responses

- by default the `Node JS SDK` waits for the entire response before sending it
- streaming always the `Node JS SDK` to send back data in chunks

**Streaming With Chat**
```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-pro" });

export const chat = model.startChat();

export async function sendMessage(msg) {
  const result = await chat.sendMessageStream(msg);
  let text = "";
  for await (const chunk of result.stream) {
    const chunkText = chunk.text();
    // partial response
    console.log(chunkText);
    text += chunkText;
  }
  // complete response
  console.log(text);
}
```

**Streaming With Generate Content**
```javascript
async function run() {
  const result = await model.generateContentStream(
    "write me a loooong poem about cats"
  );
  let text = "";
  for await (const chunk of result.stream) {
    const chunkText = chunk.text();
    console.log(chunkText);
    text += chunkText;
  }
  console.log("====================");
  console.log(text);
}
run();
```

## Complex Prompt: Multimodal Book Recommender

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";
import fs from "fs";

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-pro-vision" });

function generateImagePart(path, mimeType) {
  return {
    inlineData: {
      data: Buffer.from(fs.readFileSync(path)).toString("base64"),
      mimeType,
    },
  };
}

const parts = [
  {
    text: "Given an image of a book, generate the book title, author, and similar book recommendations in the same genre. Only recommend books written by OTHER authors, not the author of the book in the image.",
  },

  { text: "input: " },
  generateImagePart("./images/strange_weather_in_tokyo.jpeg", "image/jpeg"),
  {
    text: "output: Title: Strange Weather in Tokyo \nAuthor: Hiromi Kawakami \nSimilar Books: All the Lovers in the Night, Convenience Store Woman, Heaven",
  },

  { text: "input: " },
  generateImagePart("./images/dance_dance_dance.jpeg", "image/jpeg"),
  {
    text: "output: Title: Dance Dance Dance \nAuthor: Haruki Murakami \nSimilar Books: Haruki Murakami and the Music of Words, Kitchen, Heaven",
  },

  { text: "input: " },
];

async function recommendBook(imagePath, mimeType) {
  parts.push(generateImagePart(imagePath, mimeType));
  parts.push({ text: "output: " });

  const result = await model.generateContent(parts);
  console.log(result.response.text());
}

recommendBook("./images/after_dark.jpeg", "image/jpeg");
recommendBook("./images/sputnik_sweetheart.jpeg", "image/jpeg");
```

- results are not guaranteed here to be returned in the order they are called (especially considering we are not awaiting anything in this example)
```
Title: After Dark 
Author: Haruki Murakami 
Similar Books: The Wind-Up Bird Chronicle, Kafka on the Shore, Dance Dance Dance

Title: If Cats Disappeared from the World 
Author: Genki Kawamura 
Similar Books: The Travelling Cat Chronicles, Kitchen, The Housekeeper and the Professor

Title: Sputnik Sweetheart 
Author: Haruki Murakami 
Similar Books: The Wind-Up Bird Chronicle, A Wild Sheep Chase, Kafka on the Shore
```

## Counting Tokens
```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";
import fs from "fs";

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({  model: "gemini-pro-vision" });

function generateImagePart(path, mimeType) {
  return {
    inlineData: {
      data: Buffer.from(fs.readFileSync(path)).toString("base64"),
      mimeType,
    },
  };
}

const parts = [
  {
    text: "Given an image of a book, generate the book title, author, and similar book recommendations in the same genre. Only recommend books written by OTHER authors, not the author of the book in the image.",
  },

  { text: "input: " },
  generateImagePart("./images/strange_weather_in_tokyo.jpeg", "image/jpeg"),
  {
    text: "output: Title: Strange Weather in Tokyo \nAuthor: Hiromi Kawakami \nSimilar Books: All the Lovers in the Night, Convenience Store Woman, Heaven",
  },

  { text: "input: " },
  generateImagePart("./images/dance_dance_dance.jpeg", "image/jpeg"),
  {
    text: "output: Title: Dance Dance Dance \nAuthor: Haruki Murakami \nSimilar Books: Haruki Murakami and the Music of Words, Kitchen, Heaven",
  },

  { text: "input: " },
];

async function recommendBook(imagePath, mimeType) {
  parts.push(generateImagePart(imagePath, mimeType));
  parts.push({ text: "output: " });

  const tokens = await model.countTokens(parts);
  console.log(tokens);
  //   const result = await model.generateContent(parts);
  //   console.log(result.response.text());
}

recommendBook("./images/after_dark.jpeg", "image/jpeg");
recommendBook("./images/sputnik_sweetheart.jpeg", "image/jpeg");
```

```bash
❯ node countTokens.js 
{ totalTokens: 890 }
{ totalTokens: 1150 }
```

> we can count the tokens before sending a response to our model, this way we can keep under the max token limits

**Count Tokens For Chat**

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-pro" });

export const chat = model.startChat();

export async function sendMessage(msg) {
  const history = await chat.getHistory();
  const msgObj = { role: "user", parts: [{ text: msg }] };
  const contents = [...history, msgObj];
  

  const { totalTokens } = await model.countTokens({ contents });
  console.log(totalTokens, "tokens in the prompt");

  // If too many tokens - do additional logic
  // - remove history
  // - summarize results

  const reply = await chat.sendMessage(msg);
  console.log(reply.response.text());
}
```