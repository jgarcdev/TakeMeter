# TakeMeter

The community I chose is the Discord server for a game called SoulWorker. I chose this community because I am a member of it and I love the game.

This community is a good fit for a classification task 
as the types of messages are varied and moreso, the server 
is laid out in a way that makes it easy to categorize messages into different channels based on their content.

## Label Taxonomy

### Label 1: Lore

This label will be used for messages that discuss the lore of the game, such as character backstories, world-building, and plot details.

Example 1:
```
By the way, I believe that Lord is the main reason SFL knows so much about SoulWorkers and stuff, and why it could even be established. His “weapon” is a book, so maybe when he was thrown out of the Void and lost his memories like the others, he still had his book to guide him. Also, as the GCC leader, he possibly knew other SoulWorkers in his own and other camps, so when he got outside, he already had a list of people to search for that might help his cause.
```

Example 2:
```
Most DesireWorkers want simple things such as revenge, death, or they simply hate something, they're obsessed with something etc.. Meanwhile SoulWorkers seem to have aspirations that require various hurdles to get there 
```


### Label 2: Build

This label will be used for messages that provide tips and advice on how to build characters in the game, such as which skills to prioritize, which equipment to use, and how to optimize stats.

Example 1:
```
Chainsaw meltdown state buffs damage aoutout substantialy to skill already casted on field
```

Example 2:
```
Valor/x/enlighten x5 on Chii
```

## Data Collection & Annotation

The source of the data is the SoulWorker (Gobal) Discord server, specifically the `lore` channel and the different character channels.
There are 10 characters, so 10 different channels. However, I chose to use 4 channels: `dhana-opini`, `lee-nabi`, `ephnel`, and `chii-aruel`.
There is no particular reason to choose the 4 channels. An even number of messages were chosen from the 4 channels, meaning each channel 
contributed 25% of the total `build` messages. The `lore` channel contributed 100% of the `lore` messages. There are a total of 200 messages, 100 for each label.

It is to be noted that the character channels are not necessarily for build tips as it contained other types of messages.
Thus, the messages were manually gathered, making sure the collected ones pertained to building said character.

For both labels, the gathering had to go back by around 3 years to get enough relevant messages.

## Fine-Tuning Approach

The base model is `distilbert-base-uncased`, a lightweight 66M-parameter transformer pre-trained on English text. A sequence classification head was added on top for binary classification (`lore` vs `build`).

The dataset (200 examples, balanced 100/100) was split with train/validation/test splits of 70/15/15. Training used the following hyperparameters (unchanged from the notebook defaults):

| Hyperparameter | Value |
|---|---|
| Epochs | 3 |
| Batch size (train) | 16 |
| Batch size (eval) | 32 |
| Learning rate | 2e-5 |
| Weight decay | 0.01 |
| Warmup steps | 50 |
| Max token length | 256 |

## Baseline Description

The zero-shot baseline used `llama-3.3-70b-versatile` via the Groq API. The model was given no training examples from the dataset — only a system prompt describing the task and label definitions, plus one illustrative example per label:

```
You are classifying Discord messages from the SoulWorker Discord Server.
Assign each post to exactly one of the following categories.

lore: Discusses the lore of the game, such as character backstories, world-building, and plot details.
Example: "Most DesireWorkers want simple things such as revenge, death, or they simply hate something, they're obsessed with something etc.. Meanwhile SoulWorkers seem to have aspirations that require various hurdles to get there "

build: Provides tips and advice on how to build characters in the game, such as which skills to prioritize, which equipment to use, and how to optimize stats.
Example: "Chainsaw meltdown state buffs damage aoutout substantialy to skill already casted on field"

Respond with ONLY the label name.
Do not explain your reasoning.

Valid labels:
lore
build
```

Each of the 30 test-set posts was sent individually with `temperature=0` and `max_tokens=20` with all of them being parseable.

## Evaluation Report

Both models were evaluated on the same 30-example held-out test set (15 `lore`, 15 `build`).

### Overall Accuracy

| Model | Accuracy |
|---|---|
| Zero-shot baseline (Groq / llama-3.3-70b-versatile) | 0.967 |
| Fine-tuned DistilBERT | **1.000** |
| Improvement | +0.033 |

### Per-Class Metrics

**Fine-tuned DistilBERT**

| Label | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| lore | 1.00 | 1.00 | 1.00 | 15 |
| build | 1.00 | 1.00 | 1.00 | 15 |
| **macro avg** | **1.00** | **1.00** | **1.00** | **30** |

**Zero-shot baseline (Groq)**

| Label | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| lore | 0.94 | 1.00 | 0.97 | 15 |
| build | 1.00 | 0.93 | 0.97 | 15 |
| **macro avg** | **0.97** | **0.97** | **0.97** | **30** |

### Confusion Matrix (Fine-Tuned Model)

|  | Predicted: lore | Predicted: build |
|---|---|---|
| **Actual: lore** | 15 | 0 |
| **Actual: build** | 0 | 15 |

### Sample Classifications

| Message | True Label | Predicted | Confidence |
|---|---|---|---|
| "" |  |  | 0 |
| "" |  |  | 0 |
| "" |  |  | 0 |

<!-- Sentence -->

## Reflection

What the model learned aligns with what was intended.
This was reasonable as each label was clearly defined and each group had distinguished characteristics. The model learned to identify the key features of each label and classify the posts accordingly.


The planning helped by indicating the task that I should be doing and creating a theoretical baseline scoring.
Implementation did not diverged.

## AI Usage

Usage 1: I prompted Copilot to do some cleanup on the data. Even though I had already done some cleanup as I was gathering the data,
it turned out while running the code that Python's CSV parser had some trouble with some of the lines.
Copilot did the changes manually, which turned out to be only 3 lines.

Usage 2: I had Gemini (built into Google Colab) to fix the way the code was reading the CSV file. Even though I did the cleanup,
the parser still had some trouble, especially with how some lines were formatted. I passed the error message to Gemini and it wrote the fix,
which managed to fix the issue, allowing me to parse the CSV file for all 100 lines.