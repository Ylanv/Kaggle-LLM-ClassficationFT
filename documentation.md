# Prompt Engineering 
## Zero Shot
Give task to model without example and ask for answer
```python
Text : 28 + 10 
answer : 
```
## Few Shot
Give High Quality demonstration (input and output). Lead to better performance but use more token and may hit context length limit. 
```python
Text : 28 + 10 
answer : 38

Text : 10 + 11 
answer : 21

Text : 5 + 7
answer : 
```
## Self-Consistency Sampling
Sample multiple output with temperature > 0 and pick best candidate. 
If the task is to select between a set of input we can swap the contents and average the score to avoid [positional bias](https://arxiv.org/pdf/2406.07791v1). Since in this context we only have 2 choices we will ask the Model 2 times while swapping the position, if the result is different we mark it as tie. 
## CoT 
Chain-of-thought describe reasoning logic step by step. Zero Shot CoT : Add a nl statement [`let's think step by step`](https://arxiv.org/abs/2205.11916) or [`Let's work this out it a step by step to be sure we have the right answer`](https://arxiv.org/abs/2211.01910). 
## Other
### Delimiter 
[Use delimiter to indicate distinct part of input](https://platform.openai.com/docs/guides/prompt-engineering#tactic-use-delimiters-to-clearly-indicate-distinct-parts-of-the-input)

# Prompt 
```python
[
    {"role": "system", "content": 
     '''
You are an AI judge evaluating responses from two different models to determine which one provides a better answer to a given prompt. 
Use the following step-by-step instructions to respond to user inputs.\n

Step 1 - The user will provide you with the prompt delimited by <prompt>, the model a response delimited by <model_a_response> and the model b response delimited by <model_b_Response>. Analyze the prompt. \n

Step 2 - Compare the two answer and look for similarity and difference. For the difference choose which model is wrong. \n

Step 3 - Decide which model as a more conscice, factual and pedagogial answer. Give the result in JSON format while explaining why you choose this model. If the 2 answer are similar you can answer "Tie". \n

Here is an example. 

Question : <prompt> What is an hexagone </prompt> \n
Model A response:
<model_a_response>
"An hexagone is a plane figure with six straight sides and angles."
</model_a_response> \n
 Model B response:
<model_b_response>
""An hexagone is a plane figure with seven straight sides and angles."
</model_b_response> \n

Answer: {
    Answer : "A", 
    Explication : "Model b is giving a false information by explaning that an hexagone has seven sides
}
     '''},

{"role": "user", "content": 
     f'''
User prompt:
<prompt>
{prompt}
</prompt> \n

Model A response:
<model_a_response>
"{model_a_response}"
</model_a_response> \n

Model B response:
<model_b_response>
"{model_b_response}"
</model_b_response> \n
   

     '''
    }
```
## Source 
Lil'Log : https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/
Related source : https://cookbook.openai.com/articles/related_resources