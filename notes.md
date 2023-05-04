# Prompt Engineering for Developers Course Notes

### Possible TODO's from this course
- Utilize the ChatGPT API to create queries
- Create dot files on Github that has all the extensions and things I need for starting up a new Ubuntu image.


### Instruction
- What are the types of arge Language models (LLM)
**Base LLM**
* Used to predict the next word on text training data. Requires very large models


**Instruction Tuned LLM**
* FIne-tune on instructions and good-attempts at following these instructions. Require a small, initial model. This can answer more complex questions like "What is the capital of India?".
* RLHF: Reinforcement Learning with Human Feedback
- Less toxic responses
- For practical applications


### Prompting
- Principle 1: Write clear and specific instructions
- Use delimeters (makes it more clear and it avoids prompt injections!).
- Prompt injections can make the user change What you intended to do in the first place.
- examples (triple quotes, triple backticks, triple dashes, angle brackets, xml tags (<tag></tag>))
```python
text = f"""
Some random text
"""
prommpt = f"""Summerize the text delimited by triple backticks into a single sentence. ```{text}```
"""

response = get_completion(prompt)
print(response)
```

- Another tactic is to ask for structured output (HTML/JSON/XML).

- Another is to check whether conditions are satsfied. Check assumptions required to do the task.
- Few-shot prompting. Give succesful examples of completing tasks. Then ask the model to perform.
```python
prompt = f"""
Your task is to answer in a consistent style.

<child>: Teach me about patience.

<grandparent>: The river that carves the deepest \ 
valley flows from a modest spring; the \ 
grandest symphony originates from a single note; \ 
the most intricate tapestry begins with a solitary thread.

<child>: Teach me about resilience.
"""
response = get_completion(prompt)
print(response)
```


- Principle 2: Give the model time to think
- Specify the steps required to complete a task and ask for output in a specified format.

=====================================
```python
prompt_2 = f"""
Your task is to perform the following actions: 
1 - Summarize the following text delimited by 
  <> with 1 sentence.
2 - Translate the summary into French.
3 - List each name in the French summary.
4 - Output a json object that contains the 
  following keys: french_summary, num_names.

Use the following format:
Text: <text to summarize>
Summary: <summary>
Translation: <summary translation>
Names: <list of names in Italian summary>
Output JSON: <json with summary and num_names>

Text: <{text}>
"""
response = get_completion(prompt_2)
print("\nCompletion for prompt 2:")
print(response)
```

- Instruct the model to work out its own solution before rushing to a conclusion. Tell it to work out the problem.
```python
prompt = f"""
Your task is to determine if the student's solution \
is correct or not.
To solve the problem do the following:
- First, work out your own solution to the problem. 
- Then compare your solution to the student's solution \ 
and evaluate if the student's solution is correct or not. 
Don't decide if the student's solution is correct until 
you have done the problem yourself.

Use the following format:
Question:
```
question here
```
Student's solution:
```
student's solution here
```
Actual solution:
```
steps to work out the solution and your solution here
```
Is the student's solution the same as actual solution \
just calculated:
```
yes or no
```
Student grade:
```
correct or incorrect
```

Question:
```
I'm building a solar power installation and I need help \
working out the financials. 
- Land costs $100 / square foot
- I can buy solar panels for $250 / square foot
- I negotiated a contract for maintenance that will cost \
me a flat $100k per year, and an additional $10 / square \
foot
What is the total cost for the first year of operations \
as a function of the number of square feet.
``` 
Student's solution:
```
Let x be the size of the installation in square feet.
Costs:
1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 100x
Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000
```
Actual solution:
"""
response = get_completion(prompt)
print(response)
```
=====================================


- Fabricated ideas = hallucination.
- You can first find relevant information and link it to get the correct information.