+++
date = '2025-02-11T08:26:07-05:00'
draft = false
title = 'Llm Parameters'
+++
 LLM Call Parameters: A Complete Guide

Large Language Models (LLMs) have become essential in powering applications like chatbots, content generators, code assistants, and more. When interacting with an LLM, understanding the various call parameters can help fine-tune the model’s behavior, improve the quality of responses, and control the output effectively. This guide dives deep into the most important parameters and how to use them effectively.

---

## What Are LLM Call Parameters?

LLM call parameters are configuration settings that determine how the model generates output. These parameters allow you to control factors like creativity, relevance, and structure of the output.

Different LLM providers (like OpenAI, Hugging Face, etc.) offer various parameters, but common ones include:

- **Temperature**
- **Top-p (nucleus sampling)**
- **Min-p (minimum probability)**
- **Max tokens**
- **Frequency penalty**
- **Presence penalty**
- **Top-k sampling**
- **Top-a (additional sampling adjustment)**
- **Typical-p (typical decoding)**
- **Repetition penalty**
- **Stop sequences**

Let’s break down each of these in detail.

---

## 1. **Temperature**

The `temperature` parameter controls the randomness of the model’s output. It affects how the model selects words when generating a response.

- **Range:** 0 to 1 (sometimes higher)
- **Low temperature (e.g., 0.0 - 0.3):** Produces deterministic, precise outputs with less variation. Useful for tasks requiring accuracy, like coding.
- **High temperature (e.g., 0.7 - 1.0):** Encourages creativity and diversity in responses. Ideal for creative writing, brainstorming, and story generation.

### **Mathematical Expression**
When using temperature, logits (raw prediction scores) are transformed into probabilities using the softmax function:

\[ P(x_i) = \frac{\exp(\frac{logit_i}{T})}{\sum_j \exp(\frac{logit_j}{T})} \]

Where:
- \( logit_i \) is the raw score for token \( i \)
- \( T \) is the temperature value
- The denominator ensures that the probabilities sum to 1.

**Example:**
```json
{
  "temperature": 0.7
}
```

---

## 2. **Top-p (Nucleus Sampling)**

`top_p` (also known as **nucleus sampling**) defines the probability threshold for selecting words. Instead of considering the entire vocabulary, the model only selects from the top words that together have a cumulative probability close to `p`.

- **Range:** 0 to 1
- **Low top-p (e.g., 0.1 - 0.3):** Restricts output to highly probable words, resulting in more predictable responses.
- **High top-p (e.g., 0.8 - 1.0):** Allows a broader selection of words, enhancing creativity and diversity.

**Tip:** When using `top_p`, consider reducing `temperature` to balance control and creativity.

**Example:**
```json
{
  "top_p": 0.9
}
```

---

## 3. **Min-p (Minimum Probability)**

`min_p` sets a lower bound on the probability of tokens considered during generation. This ensures that only words with a probability higher than the specified threshold are selected, filtering out extremely low-probability options.

- **Range:** 0 to 1
- **Higher min-p (e.g., 0.1 - 0.3):** Eliminates low-probability words, leading to more focused and confident outputs.
- **Lower min-p (e.g., 0.0 - 0.05):** Allows for more diversity and creativity but may introduce noise or less relevant responses.

**Tip:** `min_p` can be useful when combined with `top_p` to balance diversity and coherence.

**Example:**
```json
{
  "min_p": 0.05
}
```

---

## 4. **Max Tokens**

`max_tokens` controls the maximum length of the generated output. The token count includes both the input and output tokens.

- **1 token:** Approximately 4 characters (depending on the language).
- **Limit:** Different models have varying token limits (e.g., OpenAI’s GPT-4 supports up to 8,192 tokens in some configurations).

**Tip:** Be mindful of the token limit to avoid truncated responses.

**Example:**
```json
{
  "max_tokens": 500
}
```

---

## 5. **Frequency Penalty**

`frequency_penalty` discourages the model from repeating words or phrases by penalizing high-frequency terms.

- **Range:** -2.0 to 2.0
- **Positive values:** Encourage more variety by reducing repetition.
- **Negative values:** Encourage repetition of key phrases or words.

**Use Case:** Creative writing, summarization, or any task where repetition is undesirable.

**Example:**
```json
{
  "frequency_penalty": 0.5
}
```

---

## 6. **Presence Penalty**

`presence_penalty` affects how likely the model is to introduce new topics or words that haven’t appeared in the input.

- **Range:** -2.0 to 2.0
- **Positive values:** Encourage the introduction of new content.
- **Negative values:** Restrict the model to existing topics, which is useful for focused conversations or technical explanations.

**Example:**
```json
{
  "presence_penalty": 0.3
}
```

---

## 7. **Top-k Sampling** (Less Frequently Used)

`top_k` limits the model to selecting from the top `k` most likely tokens at each generation step.

- **Range:** 1 to the size of the model’s vocabulary
- **Low top-k values (e.g., 10-50):** Promote conservative, focused responses.
- **High top-k values:** Allow more diverse outputs but may introduce irrelevant content.

**Example:**
```json
{
  "top_k": 50
}
```

---

## 8. **Top-a Sampling** (Less Frequently Used)

`top_a` adjusts the probability mass dynamically by focusing on tokens with adaptive probability constraints.

- **Range:** 0 to 1 (provider-specific implementation)
- **Higher values:** Promote more exploratory or creative outputs.
- **Lower values:** Keep outputs more controlled and predictable.

**Example:**
```json
{
  "top_a": 0.8
}
```

---

## 9. **Typical-p (Typical Decoding)** (Less Frequently Used)

`typical_p` ensures that the model selects tokens based on how typical they are within the overall distribution of predicted words.

- **Range:** 0 to 1
- **Low values:** Result in safer and more conservative responses.
- **Higher values:** Allow for more diverse generation without randomness.

**Example:**
```json
{
  "typical_p": 0.9
}
```

---

## 10. **Repetition Penalty**

`repetition_penalty` discourages the model from repeating the same phrases or words excessively.

- **Range:** >1.0 (values like 1.1 to 2.0 are common)
- **Higher values:** Stronger penalty on repeated tokens.

**Use Case:** Useful for longer conversations or tasks where variety is critical.

**Example:**
```json
{
  "repetition_penalty": 1.2
}
```

---

## 11. **Stop Sequences**

`stop` defines sequences of characters or words that signal the model to stop generating text.

- **Use Case:** To prevent the model from generating text beyond a certain point or after a specific phrase.

**Example:**
```json
{
  "stop": ["\n", "End"]
}
```

---

## **Tuning Parameters for Optimal Output**

Here are a few general tips for tuning LLM call parameters:

- **Structured outputs:** Use low `temperature` and specific `stop` sequences.
- **Creative outputs:** Increase `temperature`, `top_p`, or `typical_p` for variety.
- **Avoiding repetitive content:** Adjust `frequency_penalty`, `presence_penalty`, and `repetition_penalty` accordingly.
- **Token limits:** Ensure `max_tokens` is set appropriately for the task.

### Example Configuration for Creative Writing:
```json
{
  "temperature": 0.8,
  "top_p": 0.9,
  "min_p": 0.05,
  "max_tokens": 1000,
  "frequency_penalty": 0.2,
  "presence_penalty": 0.5,
  "top_k": 50,
  "typical_p": 0.9,
  "repetition_penalty": 1.2,
  "stop": ["END"]
}
```

### Example Configuration for Summarization:
```json
{
  "temperature": 0.3,
  "top_p": 0.8,
  "min_p": 0.0,
  "max_tokens": 300,
  "frequency_penalty": 0.5,
  "presence_penalty": 0.0,
  "top_k": 10,
  "repetition_penalty": 1.1
}
```

## Sources

- [Open Router parameters description](https://openrouter.ai/docs/api-reference/parameters)

---

## Conclusion

Understanding and optimizing LLM call parameters is key to getting the desired output from large language models. Whether you are generating code, writing creative stories, or providing technical summaries, fine-tuning parameters like `temperature`, `top_p`, `min_p`, `typical_p`, and `repetition_penalty` will help you get the best results. Experimentation is crucial, so don’t be afraid to try different configurations to see what works best for your application.

