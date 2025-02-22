build-lists: true
slidenumbers: true
slidecount: true
slide-transition: true
autoscale: true


# How to create your AI Assistant applying for jobs
### Artur Kuzmin

---

# Agents - iPhones?

![fit|inline](img/iphone1.png) ![fit](img/iphone2.jpg)

^ iphone 1 on the left, iphone 3 on the right

---

# Agenda

- [10 min] From LLM to Agents
- [10 min] Demo
- [30 min] Practice
- [10 min] Showcase

---

# Goals

- This is not a deep technical workshop.
- The goal is to get you curious about AI Agents.
- And play with them with as little setup as possible.
- And to come up with your own ideas of how they can be used.

---

![left](img/Aussie.jpeg)

# Artur Kuzmin

Director of Engineering at Squire,
Co-founder of Talent Agent
Google Developer Expert

[linkedin.com/in/arturkuzmin/](linkedin.com/in/arturkuzmin/)
[twitter.com/ArtursTwit](twitter.com/ArtursTwit)
![inline|20%](img/pxl-linkedin.png)

---

# Talent Agent

### Free 7-day resume building bootcamp.

![inline](img/ta-bootcamp.png)

#### https://a.gettalentagent.com/bootcamp/ar

---

# From LLM to Agents

---

## Introduction: What are AI Agents?

- AI Agents are software systems that can perform tasks autonomously or with minimal human intervention.
- They can process information, make decisions, and take actions.

---

## Early Days of LLMs - Text Input/Output

- Initial LLMs could only process text input and generate text output.
- Used for tasks like text completion and question answering.

---

## Limitations of Early LLMs

- Limited to predefined tasks and data.
- No ability to interact with external systems or perform actions outside of text processing.

---

## Introduction of Function Calling in LLMs

- LLMs gained the ability to call functions, allowing them to interact with external systems.
- Requires Structured Output.

---

## Advantages of Function Calling

- You've got a function? LLM now can use it.
- Ability to integrate with APIs, browser network, etc.

---

## AI Agents

- Get a goal.
- Plan how to achieve it.
- Use tools or tool sets as needed.
- Decide, when the goal was achieved.

---

# Hugging Face's take on Agents

[.table]
[.table-separator]

| Agency Level | Description | How that's called | Example Pattern |
| --- | --- | --- | --- |
| ☆☆☆ | LLM output has no impact on program flow | Simple Processor | process\_llm\_output(llm\_response) |
| ★☆☆ | LLM output determines an if/else switch | Router | if llm\_decision(): path\_a() else: path\_b() |
| ★★☆ | LLM output determines function execution | Tool Caller | run_function(llm\_chosen\_tool, llm\_chosen\_args) |
| ★★★ | LLM output controls iteration and program continuation | Multi-step Agent | while llm\_should\_continue(): execute\_next\_step() |
| ★★★ | One agentic workflow can start another agentic workflow | Multi-Agent | if llm\_trigger(): execute\_agent() |

---

# Free Agents course

### https://huggingface.co/learn/agents-course/

---

# Free APIs! 🎉

1. Google Gemini. 15 RPM for fast model
2. Groq. 30 RPM. Mistral / LLama / Qwen / Deepseek distilled
3. Hugging Face. 1000 RPD. Qwen / Deepseek distilled

---

# Demo Use Cases

- Deep Research of a topic
- Creating activity on LinkedIn
- Connecting to people with a lot of followers
- Finding you the best deal for a laptop on Facebook Marketplace
- Booking a time slot at DriveTest or a country embassy

---

# Demo

### https://github.com/Gaket/agents-workshop

^If many tech folks, show:

---

# Practice

1. Generate Gemini API key: https://aistudio.google.com
2. Create SmollAgents CodeAgent with network access and Gemini as an LLM
3. Create Browser Use agent that would work with a browser on your behalf

---

![left|fit](img/pxl-presentations.png)

# Artur Kuzmin

Director of Engineering at Squire,
Co-founder of Talent Agent,
Google Developer Expert

[linkedin.com/in/arturkuzmin/](linkedin.com/in/arturkuzmin/)
[twitter.com/ArtursTwit](twitter.com/ArtursTwit)

---

# Bonus

Do you want to have a deep dive how to move from LLM to Agents? Go through these examples:

https://github.com/ashishpatel26/AIAgentWorkshop/tree/main