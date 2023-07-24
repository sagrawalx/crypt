### Experiments and Events

In probability theory, the word "experiment" is used to talk abstractly and heuristically about processes which generate "outcomes" and which might be rather intricate. These experiments are formally modeled by *probability spaces*. The general definition requires a background in measure theory, which is not a prerequisite for this course, so we won't make the general definition. Instead, we'll use the following: 

<div class="element">
<span class="label">Definition</span>
A *(discrete) probability space* is a nonempty countable set $\Omega$ called the *sample space* and whose elements are called *outcomes*. Each outcome $x \in \Omega$ is assigned a real number $P[x]$ between 0 and 1 called its *probability*, and the probabilities of all of the outcomes must sum to 1.
$$ \sum_{x \in \Omega} P[x] = 1. $$
</div>

"Countable" means the the outcomes can be put into a list so that the summation $\sum_{x \in \Omega} P[x]$ makes sense. Any finite set is countable. Some infinite sets are also countable, but for now we'll focus on the finite case. 

The probability associated to each outcome should be thought of as some measure of our "confidence" that our experiment will produce that outcome. For example, it might be the percentage of times we expect the experiment to produce that outcome if the experiment were to be repeated many many times. 

Rolling a dice is an example of an experiment. The possible outcomes of this experiment are the numbers 1 through 6. In other words, the sample space is $\Omega = \{1, 2, 3, 4, 5, 6\}$. Assigning each outcome probability 1/6 means that the dice is "fair" and each outcome is equally likely. We have thus constructed a probability space: we have a finite set $\Omega$ that enumerates the possible outcomes, and we've assigned a probability to each outcome. 

A single experiment can also have "multiple parts." For example, flipping a fair coin twice can be thought of as a single experiment. Possible outcomes of this experiment might be something like "heads and then heads again," or "heads and then tails," and so forth. All of these outcomes taken together as a set form the sample space. If we abbreviate heads as $H$ and tails as $T$, we might write $\Omega = \{HH, HT, TH, TT\}$. If we assign each out these four outcomes probability 1/4, we're modeling the situation where the coin is fair and the result of each coin flip is unrelated to the other. 

In the two examples we've just seen, all of the outcomes have the same probability. This is a very common situation and so it has a special name: 

<div class="element">
<span class="label">Definition</span>
A probability space is *uniform* if all of its outcomes have equal probability. 
</div>

It is sometimes useful to group various outcomes together. This is accomplished by the following definition: 

<div class="element">
<span class="label">Definition</span>
Given a probability space, an *event* $E$ is a subset of the sample space. We define
$$ P[E] = \sum_{x \in E} P[x]. $$
</div>

Be careful: the words "event" and "outcome" feel somewhat similar in day-to-day in English, but have distinct definitions in probability theory!

Consider again the example of the rolling a dice. An "event" might be something like "the dice roll is odd." Formally, if we're thinking of the sample space $\Omega = \{1, 2, 3, 4, 5, 6\}$, the event "the dice roll is odd" corresponds to the event $E = \{1, 3, 5\}$. This event is also assigned a probability, just by summing together the probabilities of all of the outcomes that comprise that event: 
$$ P[E] = P[1] + P[3] + P[5] = \frac{1}{6} + \frac{1}{6} + \frac{1}{6} = \frac{3}{6} = \frac{1}{2}. $$

<div class="element">
<span class="label">Exercise</span>
Suppose you have 4 boxes (labeled 1, 2, 3, and 4), and you have 8 colors available (red, blue, green, yellow, pink, purple, teal, brown). Consider an experiment where each of the 4 boxes is assigned a color. For example, one possible outcome of this experiment might be the one where box 1 is colored red, box 2 is colored blue, box 3 is colored green, and box 4 is colored blue. 

a. How many possible outcomes are there? 
b. How many outcomes are in the event "no two boxes have the same color"?
</div>

<div class="element">
<span class="label">Exercise</span>
Suppose you have $k$ boxes and you have $n$ colors available. Consider again the same experiment where each of the $k$ boxes is assigned one of the $n$ colors "at random" (ie, construct a uniform probability space). What is the probability of the event that no two boxes have the same color? What is the probability that there are at least two boxes of the same color? Find expressions in terms of $n$ and $k$. 
</div>

<div class="element">
<span class="label">Sage Exercise</span>
Write some code that takes as input a positive integer $n$ and returns the smallest positive integer $k$ such that, when $k$ boxes are assigned $n$ colors at random, the probability that there are at least two boxes of the same color is at least 50%. If you run your code with $n = 365$, you should get $k = 23$ as output. Explain what this has to do with the [birthday paradox](https://en.wikipedia.org/wiki/Birthday_problem). 
</div>

<div class="element">
<span class="label">Proof Exercise</span>
Prove formally that, if a discrete probability space is uniform, it must be finite. 
</div>

### Random Variables

A very common way that events show up is by considering *random variables*, which are mathematical gadgets that represent making an observation (or taking a measurement) on the outcome of an experiment. Each random variable has a set of possible values that the random variable can take. Letters like $X, Y, \cdots$ are used to denote random variables. If you like formal definitions, here it is:

<div class="element">
<span class="label">Definition</span>
Fix a probability space $\Omega$. A *random variable* is a function with domain $\Omega$, and its *set of (possible) values* is the range of this function. 
</div>

For example, consider again the "multi-part" experiment we discussed above where we flip a coin twice. Making an observation of the first coin flip can be thought of as a random variable, which we will call $X$. This observation can be either "heads" or "tails." If we abbreviate "heads" and "tails" as $H$ and $T$ again, then we can write things like $X = H$ to refer to the event that the first coin flip landed heads. In other words, inside the sample space $\Omega = \{HH, HT, TH, TT\}$, the notation $X = H$ describes the event $\{HH, HT\}$ and we have
$$ P[X = H] = P[HH] + P[HT] = \frac{1}{4} + \frac{1}{4} = \frac{1}{2}. $$
There are many other observations or measurements we could make with the same experiment. For example, we might instead be interested in the number of heads. This is another random variable; let's call it $Y$. This can take values 0, 1, and 2. The notation $Y = 1$ describes the event that we observe 1 heads out of the two coin flips, ie, the event $\{HT, TH\}$, and we have
$$ P[Y = 1] = P[HT] + P[TH] = \frac{1}{4} + \frac{1}{4} = \frac{1}{2}. $$
On the other hand, $Y = 0$ corresponds to the singleton event $\{TT\}$, so
$$ P[Y = 0] = P[TT] = \frac{1}{4}. $$
This leads us to the following definition:

<div class="element">
<span class="label">Definition</span>
A random variable is *uniform* if all of its values have equal probability. 
</div>

In our example above, the random variable $X$ is uniform but $Y$ is not. With $X$, the two possible values are $H$ and $T$, and both of those have probability 1/2. With $Y$, however, there are three possible values, with one of those values being twice as likely as either of the other two. 

<div class="element">
<span class="label">Proof Exercise</span>
Prove formally that, if $\Omega$ is a discrete probability space and $X$ is a uniform random variable on $\Omega$, then the set of values of $X$ must be finite. (This should be very similar to the last "Proof Exercise.")
</div>

Here is another useful notion that comes up often when dealing with random variables: 

<div class="element">
<span class="label">Definition</span>
Suppose $X$ is a random variable whose values are real numbers. The *expected value* (or *expectation*) of $X$, denoted $E[X]$, is defined by
$$ E[X] = \sum_{\text{values } a} a \cdot P[X = a]. $$
</div>

For example, in the experiment involving two coin flips, the random variable $Y$ which counts the number of heads has real number values. Its expectation is 
$$ E[Y] = 0 \cdot \frac{1}{4} + 1 \cdot \frac{1}{2} + 2 \cdot \frac{1}{4} = 1. $$

<div class="element">
<span class="label">Exercise</span>
Consider the experiment where you roll a pair of fair dice. Let the random variable $X$ denote the sum of the dice rolls. 

a. What are the possible values of $X$? 
b. What is $P[X = 7]$? 
c. What is $P[X = 7 \text{ or } 11]$?
d. What is $E[X]$? 
</div>

<div class="element">
<span class="label">Exercise</span>
Below are four versions of the same game. Your archnemesis gets to pick the version of the game, and then you get to choose how many times to flip a coin: 10 times or 100 times. Identify how many coin flips you should choose for each version of the game. It costs $1 to play each game. Explain your reasoning.

a. If the proportion of heads is larger than 0.60, you win $1.
b. If the proportion of heads is larger than 0.40, you win $1.
c. If the proportion of heads is between 0.40 and 0.60, you win $1.
c. If the proportion of heads is smaller than 0.30, you win $1.
</div>

<div class="element">
<span class="label">Exercise</span>
Let's say we play a game where we keep flipping a fair coin until we've seen either one heads or three tails overall. 

a. What is the probability that you see strictly more tails than heads?
b. What is the probability that you see strictly more heads than tails? 
c. How many flips are expected on average? 
d. How many tails are expected on average?
</div>
