# TakeMeter - planning.md

## Community

The community I chose is the Discord server for a game called SoulWorker. I chose this community because I am a member of it 
and I love the game.

This community is a good fit for a classification task 
as the types of messages are varied and moreso, the server 
is laid out in a way that makes it easy to categorize messages into different channels based on their content.

## Labels

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


### Label 2: Character Build Tips

This label will be used for messages that provide tips and advice on how to build characters in the game, such as which skills to prioritize, which equipment to use, and how to optimize stats.

Example 1:

```
Right click to keep red ring up
444 buffs
Right Click -> 444 -> 11 -> 222 -> 666 -> Ariel 3 2 -> 55 -> 33 <-- Really only cast 33 if Skills 11 isnt already off cooldown
Most if not all damage comes from
Skills 11 and 22
They need to be chained
Chainsaw meltdown state buffs damage aoutout substantialy to skill already casted on field
```

Example 2:

```
Valor/x/enlighten x5 on Chii
Gold weapon for general and red for ranking
Red is slightly better assuming you get at least 2+ enlighten procs (tera) per rotation
```

## Edge Cases

I believe that there would not be many edge cases in this classification task, as the messages are generally straightforward and easy to categorize. However, there may be some messages that are difficult to classify, such as those that contain both lore and character build tips. In these cases, I would classify the message based on the primary focus of the message.


## Data Collection Plan

As mentioned, the examples will be collected from the Discord server. I will collect about 100 message groups (as a single message may not necessary provide enough context) per label.
I believe that there will be enough data in the server to equally represent both labels, as the server has a large and active community of players who discuss both lore and character build tips.

## Evaluation

The metrics used to evaluate the model will be accuracy, precision, recall, and F1 score. I will also use a confusion matrix to visualize the performance of the model and identify any areas for improvement.

## Success Criteria

In terms of numbers, I would consider the model to be successful if it achieves at least an 80% for each score.
Considering the nature of the task, I believe that this is a reasonable goal to set for the model.

## AI Tool Plan

Label stress-testing: none

Annotation assistance: none

Failure analysis: none