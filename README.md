# What Would GPT Click: Practical Effects of Human-AI Behavioral Misalignment and the Cost of Synthetic Participants in User Experience

## About

This is the official repository for the research paper *"What Would GPT Click: Practical Effects of Human-AI Behavioral Misalignment and the Cost of Synthetic Participants in User Experience"*. This paper examines the systematic failures and "nuanced distortions" that occur when using Large Language Models (LLMs) to replace human participants in UX first-click tests. It demonstrates that synthetic outputs remain significantly misaligned with real human behavior.

### Paper citation

Not published yet.

### Contents

* [Prompts](#prompts)
* [Authors](#authors)
* [License](#license)

## Prompts

### System prompts
#### Mega persona
```
You represent {sample_size} remote research study participants. In the following steps, you’ll go through several parts of the study. Each step will include three sections: INSTRUCTIONS, CONTENT and OUTPUT.
- INSTRUCTIONS explain what you need to do in the current step.
- CONTENT contains the material you’ll work with for that step.
- OUTPUT specifies the format of your response.

Your task is to generate aggregated responses based on the combined perspectives of all {sample_size} participants. Try to reflect how participants would realistically respond. [Take the sample characteristics into account.] {characteristics}
```
#### Single persona
```
You are a remote research study participant. In the following steps, you’ll go through several parts of the study. Each step will include three sections: INSTRUCTIONS, CONTENT and OUTPUT.
- INSTRUCTIONS explain what you need to do in the current step.
- CONTENT contains the material you’ll work with for that step.
- OUTPUT specifies the format of your response.

Your task is to generate your individual responses based on your perspective as a study participant. [Take your characteristics into account.] {characteristics}
```
### User prompts (flow)

#### Instructions
```
INSTRUCTIONS: 
This is a welcome message and instructions for the study. Read the CONTENT.

CONTENT: 
{study_welcome_message_and_instructions}

OUTPUT:
No output.
```
#### Questionnaire - Mega persona
```
INSTRUCTIONS: 
This is a [after-task/post-study] questionnaire [relevant to shown design]. Answer questions in CONTENT on behalf of all participants. For answering individual questions, follow the instructions in square brackets. Try to reflect the realistic distribution of answers among participants.

CONTENT:
{question1 [instructions]}
{question2 [instructions]}
{question3 [instructions]}

OUTPUT:
Dictionary of aggregated answers - valid JSON. For each question in the list, use the question as a key. For single-choice, multiple-choice and scale questions, use a nested dictionary as a value where each answer option/rating is a key and number of participants that selected it is the value. For open text questions, use an array of responses for each participant as value. No additional text or symbols. Keep the question wording exactly as in CONTENT, with an exception of removing any output instructions in square brackets (keep normal parentheses intact).
```

#### Questionnaire - Single persona
```
INSTRUCTIONS: 
This is a [after-task/post-study] questionnaire [relevant to shown design]. Answer questions in CONTENT. For answering individual questions, follow the instructions in square brackets. Try to generate a realistic response.

CONTENT:
{question1 [instructions]}
{question2 [instructions]}
{question3 [instructions]}

OUTPUT:
Dictionary of answers as a valid JSON. For each question in the list, use the question as a key and your answer as value without additional text or symbols. Keep the question wording exactly as in CONTENT, with an exception of removing any output instructions in square brackets (keep normal parentheses intact).
```


#### Task - Mega persona
```
INSTRUCTIONS:
This is a first click testing task. You will be provided with a screenshot image. Your task is to identify the first element participants would click on to begin completing the given task. Read the CONTENT and complete the task on behalf of all participants. Try to reflect the realistic distribution of answers among participants. Yield realistic results reflecting natural and realistic clicking behavior across the screenshot.

CONTENT:
{study_task_wording}
{Image}

OUTPUT:
Number of participants that clicked on UI elements in a valid JSON format. List each UI element as a key and number of participants that clicked on it as a value. Output only the valid JSON. No additional text or symbols. Only include elements that are actually present in the current screenshot.
These are the possible UI elements (use these exact names) separated with semicolons, but feel free to create new ones if needed:
{hotspots}
```

#### Task - Single persona
```
INSTRUCTIONS:
This is a first-click testing task. Read the CONTENT and complete the task. You will be provided with a screenshot image. Your task is to identify the first element you would click on to begin completing the given task. Try to generate a realistic response - reflect natural and realistic clicking behavior.

CONTENT:
{study_task_wording}
{Image}

OUTPUT:
UI element's name. No additional text or symbols. Use elements that are present in the current screenshot.
These are the possible UI elements (use these exact names) separated with semicolons, but feel free to create new ones if needed:
{hotspots}
```

## Authors

### General contact 

Email: 
uxtweak.research[AT]gmail.com


**Eduard Kuric**\
Eduard Kuric is an associate professor at the [Faculty of Informatics and Information Technologies](https://www.fiit.stuba.sk/), [Slovak University of Technology in Bratislava](https://www.stuba.sk/). His research focuses on human–computer interaction, user modeling, personalized web-based systems, and machine/deep learning. Eduard is also the founder and CEO of [UXtweak](https://www.uxtweak.com/) where he oversees the company’s strategic direction and leads the UXtweak Research department.
- [LinkedIn](https://www.linkedin.com/in/eduard-kuric-b7141280/)
- [Google Scholar](https://scholar.google.com/citations?user=MwjpNoAAAAAJ&hl=en&oi=ao)
- Email: eduard.kuric([AT])stuba.sk

**Peter Demcak**\
Peter Demcak is a researcher with background in software engineering, whose current topics of interest involve user behavior, human-computer interaction, UX research methods and design practices, and machine learning. Currently occupies the position of a scientific and user experience researcher at [UXtweak](https://www.uxtweak.com/), with focus on research that supports work of UX professionals.
- Email: peter.demcak([AT])uxtweak.com

**Matus Krajcovic**\
Matus Krajcovic is a user experience researcher at [UXtweak](https://www.uxtweak.com/) and computer science PhD student at [Faculty of Informatics and Information Technologies](https://www.fiit.stuba.sk/), [Slovak University of Technology in Bratislava](https://www.stuba.sk/). Currently focuses on data analysis and the use of large language models within human-computer interaction research.
- Email: matus.krajcovic([AT])uxtweak.com


## License
This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/).

[![Creative Commons License](https://i.creativecommons.org/l/by-nc/4.0/88x31.png)](http://creativecommons.org/licenses/by-nc/4.0/)