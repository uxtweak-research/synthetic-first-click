# What Would GPT Click: In-depth Practical Investigation of Human-AI Behavioral Misalignments of Synthetic Participants in a User Experience Testing Method

## About

This is the official repository for the research paper *"What Would GPT Click: In-depth Practical Investigation of Human-AI Behavioral Misalignments of Synthetic Participants in a User Experience Testing Method"*. This paper examines the systematic failures and "nuanced distortions" that occur when using Large Language Models (LLMs) to replace human participants in UX first-click tests. It demonstrates that synthetic outputs remain significantly misaligned with real human behavior.

### Paper citation

Not published yet.

### Contents

* [Prompts](#prompts)
* [Extended results](#extended-results)
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

## Extended results

Table: Statistical analysis results, comparing simulations to real data and to the GPT-4.1 baseline model. Adjusted statistical significance, evaluated using Chi-squared tests of independence and Mann-Whitney U tests, is indicated by asterisks: \*p < .05, \*\*p < .01, and \*\*\*p< .001.

| Variable | Real | Baseline | GPT-5.2 (medium reasoning) | Lower temperature | Lower top\_p | Single-persona | Nondescript personas |
|---|---|---|---|---|---|---|---|
| *Tasks* |  |  |  |  |  |  |  |
| Distribution Difference |  | 53\% | 44\% <br>  $\chi^2$=0.40, V=.07 | 53\% <br> $\chi^2$=0, V=0 | 62\% <br> $\chi^2$=0.41, V=.07 | 96\% <br> $\chi^2$=18.92***, V=.46 | 53\% <br> $\chi^2$=0, V=0 |
| First-choice Agreement |  | 71\% | 69\% <br> $\chi^2$=0, V=0 | 71\% <br> $\chi^2$=0, V=0 | 71\% <br> $\chi^2$=0, V=0 | 76\% <br> $\chi^2$=0.06, V=.03 | 71\% <br> $\chi^2$=0, V=0 |
| Rank-order Agreement |  | .16 ± .14 | .19 ± .16 <br> z=-0.67, r=-.07 | .15 ± .14 <br> z=0.08, r=.01 | .14 ± .14 <br> z=0.49, r=.05 | .11 ± .08 <br> z=1.15, r=.12 | .15 ± .14 <br> z=0.18, r=.02 |
| Jensen-Shannon Distance |  | .48 ± .16 | .45 ± .16 <br> z=0.65, r=.07 | .48 ± .16 <br> z=0.16, r=.02 | .48 ± .16 <br> z=-0.11, r=-.01 | .58 ± .21 <br> z=-2.16, r=-.23 | .48 ± .17 <br> z=-0.07, r=-.01 |
| Entropy | .61 ± .18 | .43 ± .19 | .54 ± .14 <br> z=-2.77*, r=-.29 | .45 ± .19 <br> z=-0.6, r=-.06 | .45 ± .19 <br> z=-0.53, r=-.06 | .02 ± .06 <br> z=7.97***, r=.84 | .45 ± .19 <br> z=-0.33, r=-.03 |
| Entropy gap (vs. real) |  | $\Delta$=-.18, z=-4.09*** | $\Delta$=-.07, z=-1.63 | $\Delta$=-.16, z=-3.80** | $\Delta$=-.15, z=-3.57** | $\Delta$=-.59, z=-8.16*** | $\Delta$=-.16, z=-3.77** |
| Unique Selection Count | 9.00 ± 4.75 | 5.38 ± 2.44 | 6.51 ± 1.93 <br> z=-3.49**, r=-.37 | 5.31 ± 2.50 <br> z=0.31, r=.03 | 5.60 ± 2.72 <br> z=-0.21, r=-.02 | 1.16 ± 0.37 <br> z=8.14***, r=.86 | 5.4 ± 2.84 <br> z=0.56, r=.06 |
| Count gap (vs. real) |  | $\Delta$=-3.62, z=-4.35*** | $\Delta$=-2.49, z=-2.22 | $\Delta$=-3.69, z=-4.54*** | $\Delta$=-3.40, z=-3.97*** | $\Delta$=-7.84, z=-8.17*** | $\Delta$=-3.60, z=-4.49*** |
| *Close-ended questions* |  |  |  |  |  |  |  |
| Distribution Difference |  | 45\% | 62\% <br> $\chi^2$=3.71, V=.15 | 45\% <br> $\chi^2$=0, V=0 | 46\% <br> $\chi^2$=0, V=0 | 85\% <br> $\chi^2$=25.27***, V=.40 | 41\% <br> $\chi^2$=0.10, V=.03 |
| First-choice Agreement |  | 50\% | 38\% <br> $\chi^2$=1.66, V=.10 | 51\% <br> $\chi^2$=0, V=0 | 64\% <br> $\chi^2$=2.62, V=.13 | 73\% <br> $\chi^2$=7.83*, V=.22 | 62\% <br> $\chi^2$=1.66, V=.10 |
| Rank-order Agreement |  | .36 ± .27 | .30 ± .29 <br> z=1.72, r=.14 | .39 ± .31 <br> z=-0.44, r=-.04 | .40 ± .30 <br> z=-0.84, r=-.07 | .33 ± .28 <br> z=0.82, r=.07 | .39 ± .32 <br> z=-0.42, r=-.03 |
| Jensen-Shannon Distance |  | .28 ± .13 | .30 ± .14 <br> z=-1.74, r=-.14 | .28 ± .15 <br> z=-0.18, r=-.01 | .27 ± .15 <br> z=0.61, r=.05 | .43 ± .19 <br> z=-5.66***, r=-.45 | .26 ± .15 <br> z=1.01, r=.08 |
| Entropy | .72 ± .15 | .76 ± .08 | .77 ± .11 <br> z=-1.52, r=-.12 | .76 ± .09 <br> z=0.21, r=.02 | .71 ± .15 <br> z=1.01, r=.08 | .34 ± .25 <br> z=9.64***, r=.77 | .71 ± .14 <br> z=2.1, r=.17 |
| Entropy gap (vs. real) |  | $\Delta$=.04, z=0.75 | $\Delta$=.05, z=3.22** | $\Delta$=.04, z=1.03 | $\Delta$=-.01, z=-0.15 | $\Delta$=-.38, z=-9.2*** | $\Delta$=-.01, z=-0.77 |
| Unique Selection Count | 5.15 ± 1.67 | 4.55 ± 1.13 | 4.78 ± 1.29 <br> z=-1.58, r=-.13 | 4.35 ± 1.07 <br> z=1.56, r=.13 | 4.18 ± 1.08 <br> z=2.40*, r=.19 | 2.47 ± 1.00 <br> z=8.60***, r=.69 | 4.18 ± 1.42 <br> z=2.77**, r=.22 |
| Count gap (vs. real) |  | $\Delta$=-0.60, z=-2.87** | $\Delta$=-0.37, z=-1.92 | $\Delta$=-0.80, z=-3.60*** | $\Delta$=-0.97, z=-4.10*** | $\Delta$=-2.68, z=-8.44*** | $\Delta$=-0.97, z=-3.72*** |
| *Open-ended questions* |  |  |  |  |  |  |  |
| Lexical Similarity |  | .21 ± .11 | .22 ± .10 <br> z=-0.24, r=-.03 | .21 ± .11 <br> z=0.21, r=.02 | .20 ± .11 <br> z=0.46, r=.05 | .22 ± .11 <br> z=-0.17, r=-.02 | .21 ± .10 <br> z=0.07, r=.01 |
| Semantic Similarity |  | .70 ± .15 | .68 ± .19 <br> z=0.08, r=.01 | .68 ± .15 <br> z=0.95, r=.10 | .67 ± .16 <br> z=1.16, r=.12 | .61 ± .20 <br> z=2.17, r=.23 | .71 ± .15 <br> z=-0.24, r=-.03 |
| Semantic Diversity | .76 ± .09 | .70 ± .14 | .64 ± .14 <br> z=3.0**, r=.32 | .66 ± .14 <br> z=2.9*, r=.31 | .65 ± .14 <br> z=3.09**, r=.33 | .30 ± .14 <br> z=7.55***, r=.80 | .69 ± .13 <br> z=0.83, r=.09 |
| Semantic Diversity gap (vs. real) |  | $\Delta$=-.06, z=-2.54* | $\Delta$=-.12, z=-4.93*** | $\Delta$=-.10, z=-4.51*** | $\Delta$=-.11, z=-4.77*** | $\Delta$=-.46, z=-8.07*** | $\Delta$=-.07, z=-3.39** |
| Lexical Repetitiveness | 5606.37 ± 3943.40 | 1579.30 ± 1652.30 | 4486.25 ± 2603.92 <br> z=-5.43***, r=-.57 | 1770.63 ± 2269.06 <br> z=0.03, r=0 | 2139.92 ± 2255.83 <br> z=-0.71, r=-.07 | 7655.14 ± 3183.16 <br> z=-6.83***, r=-.72 | 1529.34 ± 1524.90 <br> z=0.13, r=.01 |
| Lexical Repetitiveness gap (vs. real) |  | $\Delta$=-4027.07, z=-5.62*** | $\Delta$=-1120.12, z=-1.22 | $\Delta$=-3835.74, z=-5.36*** | $\Delta$=-3466.45, z=-4.67*** | $\Delta$=2048.77, z=3.04** | $\Delta$=-4077.03, z=-5.69*** |
| Readability | 67.40 ± 16.27 | 54.04 ± 20.76 | 53.07 ± 21.66 <br> z=0.13, r=.01 | 59.21 ± 20.84 <br> z=-1.84, r=-.19 | 60.60 ± 21.44 <br> z=-2.17, r=-.23 | 49.42 ± 23.92 <br> z=1.03, r=.11 | 55.99 ± 18.69 <br> z=-0.15, r=-.02 |
| Readability gap (vs. real) |  | $\Delta$=-13.36, z=-3.13** | $\Delta$=-14.33, z=-3.05** | $\Delta$=-8.19, z=-1.62 | $\Delta$=-6.80, z=-1.17 | $\Delta$=-17.98, z=-3.86** | $\Delta$=-11.41, z=-3.10** |
| Word Count | 13.31 ± 8.26 | 9.08 ± 4.76 | 13.74 ± 6.59 <br> z=-4.80***, r=-.51 | 8.31 ± 4.09 <br> z=1.24, r=.13 | 8.20 ± 3.55 <br> z=0.68, r=.07 | 33.72 ± 17.24 <br> z=-6.63***, r=-.70 | 8.62 ± 3.50 <br> z=-0.10, r=-.01 |
| Word Count gap (vs. real) |  | $\Delta$=-4.23, z=-1.92 | $\Delta$=0.43, z=0.44 | $\Delta$=-5.00, z=-2.51* | $\Delta$=-5.11, z=-2.46* | $\Delta$=20.41, z=6.19*** | $\Delta$=-4.69, z=-2.07 |

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