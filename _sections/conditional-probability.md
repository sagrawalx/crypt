We introduce a definition that looks very simple at first, but actually introduces many counter-intuitive results. So before getting to the definition, consider the following situation. Suppose Kambili and Amaka both secretly flip two fair coins. Kambili announces that her second flip was heads. Amaka announces that she had at least one heads. Who's more likely to have flipped two heads? In other words, if you had to make a bet about who flipped more heads, who would you bet on? We'll discuss the answer to this question in more detail below. For now, just think about what your intuition says. 

<div class="element">
<span class="label">Definition</span>
Fix a probability space. Given two events $A$ and $B$, we define the *conditional probability* $P[A \mid B]$ by
$$ P[A \mid B] = \frac{P[A \cap B]}{P[B]}, $$
where $A \cap B$ is the set of all outcomes that are in both $A$ and $B$. The conditional probability $P[A \mid B]$ is also called the probability of $A$ *given* $B$.
</div> 

The intuition here is that $P[A \mid B]$ represents how confident we are that $A$ happens *given that we already know that $B$ happens*. 

Let's now see this definition in action. For many people (but not everyone), the intuitive answer to the question posed at the beginning of this section is, "both are equally likely." If that's what you thought, you're in good company. Unfortunately, it's not correct!

To see why, let's formalize what's going on. The experiment we're considering involves two coin flips, so we're again dealing with the uniform probability space whose sample space is $\Omega = \{HH, HT, TH, TT\}$. The event of interest $A$ is the singleton event that both flips were heads, ie, $A = \{HH\}$. 

In Kambili's situation, we know that her second flip was heads. In other words, we're restricting ourselves to the event $B_1 = \{HH, TH\}$, and we have
$$ P(A \mid B_1) = \frac{P(A \cap B_1)}{P(B_1)} = \frac{P(A)}{P(B_1)} = \frac{1/4}{1/2} = \frac{1}{2}. $$
This tells us that the probability that Kambili has two heads is 1/2. In Amaka's situation, we only that one of her flips was heads. In other words, we're now restricting ourselves to the event $B_2 = \{HH, TH, HT\}$, and we have 
$$ P(A \mid B_2) = \frac{P(A \cap B_2)}{P(B_2)} = \frac{P(A)}{P(B_2)} = \frac{1/4}{3/4} = \frac{1}{3}. $$
This tells us that the probability that Amaka has two heads is 1/3. 

In other words, it's more likely that Kambili has two heads! If we had to bet on one of the two having more heads based on the information we've been given, we should bet on Kambili. 

Here are some exercise to practice:

<div class="element">
<span class="label">Exercise</span>
There are three coins in a bag. One is a normal quarter: one side is heads, the other side is tails. The second coin is almost identical except that both sides are heads; similarly, both sides of the third coin are tails. You shake the bag around to shuffle the coins. You then close your eyes, pull one coin out at random, put it down on a table, and then open your eyes. You see heads. What is the probability that the other side of the coin is also heads? 
</div>

<div class="element">
<span class="label">Exercise</span>
Explain what [this xkcd comic](https://xkcd.com/795/) has to do with conditional probability. 
</div> 

<div class="element">
<span class="label">Exercise</span>
Suppose 80% of people like peanut butter, 89% like jelly, and 78% like both. Given that a randomly sampled person likes peanut butter, what’s the probability that they also like jelly?
</div>

<div class="element">
<span class="label">Exercise</span>
Lupus is a medical phenomenon where antibodies that are supposed to attack foreign cells to prevent infections instead see plasma proteins as foreign bodies, leading to a high risk of blood clotting. It is believed that 2% of the population suffers from this disease. A test for lupus is 98% accurate if a person actually has the disease, and 74% accurate if a person does not have the disease. There is a line from the Fox television show *House* that is often used after a patient tests positive for lupus: "It's never lupus." Do you think there is truth to this statement? Use appropriate probabilities to support your answer.
</div>

<div class="element">
<span class="label">Harder Exercise</span>
Suppose you play the following game. You roll two fair dice. Let $U$ be the sum of the numbers on the two dice. 

* If $U = 7$ or $11$, you win. 
* If $U = 2, 3$, or $12$, you lose. 
* Otherwise, you roll both dice repeatedly until one of the following happens: 
    - If you roll $U$ again, you win. 
    - If you roll 7, you lose. 

What is the probability that you win?
</div>

The idea of conditional independence leads us to the following important definition that occurs frequently in probability theory: 

<div class="element">
<span class="label">Definition</span>
Fix a probability space. We say that two events $A$ and $B$ are *independent* if 
$$ P(A \cap B) = P(A) P(B). $$
</div>

It is often most convenient to reformulate this definition slightly. In the case that $P(B) > 0$, observe that $P(A \cap B) = P(A) P(B)$ is equivalent to $P(A \cap B)/P(B) = P(A)$. But the left-hand side of this equation is $P(A \mid B)$ by definition, so we see that independence of $A$ and $B$ is equivalent to asserting that
$$ P(A \mid B) = P(A). $$
This statement should be interpreted as follows: if $A$ and $B$ are independent, our confidence in $A$ happening does not change at all even if we're told that $B$ happened (or that it did not happen). More loosely, knowing whether or not $B$ happens tells us "nothing" about whether or not $A$ happens!

<div class="element">
<span class="label">Exercise</span>
The American Community Survey is an ongoing survey that provides data every year to give communities the current information they need to plan investments and services. The 2010 American Community Survey estimates that 14.6% of Americans live below the poverty line, 20.7% speak a language other than English at home, and 4.2% fall into both categories. Is the event that a randomly chosen American lives below the poverty line independent of the event that the person speaks a language other than English at home?
</div>

<div class="element">
<span class="label">Exercise</span>
A bag contains 5 red marbles and 3 blue marbles. Two marbles are drawn randomly from the bag: Alejandra takes the first one and Beatrice takes the second. Is the event that Alejandra's marble is blue independent of the event that Beatrice's marble is blue?
</div>


<div class="element">
<span class="label">Exercise</span>
Fix a probability space $\Omega$ and two events $A$ and $B$. Let $\neg B$ denote the event that $B$ does not happen, ie, $\neg B = \Omega \setminus B$. Show that $A$ and $B$ are independent if and only if $A$ and $\neg B$ are independent.
</div>

There is also a version of this definition for random variables. 

<div class="element">
<span class="label">Definition</span>
Fix a probability space. Two random variables $X$ and $Y$ are *independent* if the events $X = a$ and $Y = b$ are independent for all pairs $(a, b)$ where $a$ is a value of $X$ and $b$ is a value of $Y$. 
</div>

