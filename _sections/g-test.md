### Example Setup

To motivate this section, consider the following situation. Suppose that every registered voters in an imaginary county in the United States is classified into the mutually exclusive and exhaustive racial groups "White," "Black," "Hispanic," and "Other." Before proceeding any further, let's stop to think through this: 

<div class="element">
<span class="label">Exercise</span>
Brainstorm as many reasons as you can about why it might be problematic (and/or overly simplistic) to categorize all of the registered voters of our imaginary county into one and only one of the racial categories "White," "Black," "Hispanic," and "Other." 
</div>

I urge you to think carefully about issues like the ones you may have identified in the above exercise whenever someone proposes some clear-cut categorization scheme to you. In any case, we'll continue this section with our simplistic modeling assumption that these four racial categories are mutually exclusive and exhaustive. We'll see later that there are situations where assuming a mutually exclusive and exhaustive categorization is not so problematic, including in problems related to cryptography. 

Suppose that, by inspecting the voter rolls, we find that racial distribution of this county is as follows: 

<table style="margin: auto; width: 75%; text-align: center;">
<tr>
<th width="35%"></th>
<th width="15%">White</th>
<th width="15%">Black</th>
<th width="15%">Hispanic</th>
<th width="15%">Other</th>
<th width="15%">Total</th>
</tr>

<tr>
<th>Distribution</th>
<td>72%</td>
<td>7%</td>
<td>12%</td>
<td>9%</td>
<td>100%</td>
</tr>
</table>

Since jurors are supposed to be drawn from the list of registered voters, we might hope that a random sample of jurors would follow this same racial distribution. Suppose that we actually sample 275 jurors and observe the racial distribution displayed in the second row of the table below. 

<table style="margin: auto; width: 75%; text-align: center;">
<tr>
<th width="35%"></th>
<th width="15%">White</th>
<th width="15%">Black</th>
<th width="15%">Hispanic</th>
<th width="15%">Other</th>
<th width="15%">Total</th>
</tr>

<tr>
<th>Distribution</th>
<td>72%</td>
<td>7%</td>
<td>12%</td>
<td>9%</td>
<td>100%</td>
</tr>

<tr>
<th>Observed</th>
<td>210</td>
<td>10</td>
<td>20</td>
<td>35</td>
<td>275</td>
</tr>

<tr>
<th>Expected</th>
<td>198</td>
<td>19.25</td>
<td>33</td>
<td>24.75</td>
<td>275</td>
</tr>
</table>

If our random sample of jurors followed the overall racial distribution of registered voters, we would expect that 72% of them would be White, which would be $0.72 \cdot 275 = 198$ people. We can calculate the expected numbers of jurors in the other groups similarly to fill in the third row above. 

Note that, even if the sample of jurors was representative of the county's electorate, there would be *some* deviation from the expected counts. For example, remember that we're assuming that the categories are mutually exclusive, so we could not possibly observe a sample with 19.25 Black jurors! But if we had observed something like 198 White jurors, 19 Black jurors, 33 Hispanic jurors, and 25 Other jurors --- or something very close to that --- we probably would not be very surprised with our results. Stated differently, the data we collected would feel consistent with the hypothesis that the racial distribution of jurors matches the racial distribution of the electorate. 

But it *feels* like what we observed was actually pretty far from the expected counts...! How can we quantify and make sense of this observation? 

### $G$-Test

The idea is to introduce a number that measures the difference between the observed and expected rows. There are a variety of numbers that can be used, but let us consider one that is often denoted $G$.[^chisq] It is defined as follows: 

[^chisq]: A common alternative is one denoted $\chi^2$. If this one is used instead, the resulting test would be "Pearson's chi-squared test." 

<div class="element">
<span class="label">Definition</span>
Suppose $X$ is a random variable with finitely many values $a_1, \dotsc, a_n$ and let $p_i = P[X = a_i]$. Suppose we make $N$ observations of the values $a_1, \dotsc, a_n$ and that $O_i$ is the number of observations of $a_i$ that we made. Let $E_i = N p_i$ and then define
$$ G = 2 \sum_i O_i \ln \left( \frac{O_i}{E_i} \right). $$
If $O_i = 0$ for some $i$, we set the corresponding summand $O_i \ln(O_i/E_i) = 0$. If there exists an $i$ such that $E_i = 0$ but $O_i \neq 0$, set $G = \infty$. 
</div>

In our example, the random variable $X$ represents observing the race of a randomly drawn voter from our county. It has 4 possible values (White, Black, Hispanic, Other), so $n = 4$. The values $p_i$ are the percentages of the electorate in each racial group, the values $O_i$ are the observed counts, and the values $E_i$ are the expected counts. We have 
$$ G = 2 \left( 210 \ln \left( \frac{210}{198} \right) + 10 \ln \left( \frac{10}{19.25} \right) + 20 \ln \left( \frac{20}{33} \right) + 35 \ln \left( \frac{35}{24.75} \right) \right) \approx 15.84 $$

Here is a fundamental observation about this quantity: 

<div class="element">
<span class="label">Gibbs' Inequality</span>
We always have $G \geq 0$. Moreover, $G = 0$ if and only if $O_i = E_i$ for all $i = 1, \dotsc, n$.
</div>

Observe that if $O_i = E_i$ for all $i$, then $\ln(O_i/E_i) = \ln(1) = 0$ for all $i$, so $G = 0$. This proves one small part of the above theorem; we will omit the rest of the proof, but if you're a proof-inclined person, you can find some accessible proofs [on Wikipedia](https://en.wikipedia.org/wiki/Gibbs%27_inequality). The key idea to take away is that $G$ is 0 when the observed counts exactly match up with the expected counts, and that $G$ gets big when the observed counts differ significantly from the expected counts. 

How big is "big"? In particular, in our example, is 15.84 a "big" value of $G$? The answer to this question is provided by the following difficult theorem from probability theory, which we state slightly imprecisely and explain in a bit more detail after the statement. 

<div class="element">
<span class="label">Wilks' Theorem</span>
Suppose the $N$ observations of the values $a_1, \dotsc, a_n$ that we make are in fact independent observations of the random variable $X$. For large values of $N$, the values of $G$ are well approximated by a chi-square distribution with $n-1$ degrees of freedom. 
</div>

Several points of explanation are now in order. First, a "chi-square distribution with $k$ degrees of freedom" is a certain function $f_k$ defined on $[0, \infty)$ and taking non-negative values everywhere with total integral equal to 1.[^distribution] In other words, we have $f_k(x) \geq 0$ for all $x \geq 0$ and  
$$ \int_0^\infty f_k(x)\, dx = 1. $$
The formula for $f_k(x)$ is complicated and also unimportant for our purposes, because as we will note below, most well-developed programming languages have built-in functions that know how to calculate relevant integrals of $f_k$. Still, it's useful to at least have some geometric intuition about what the functions $f_k$ look like for varying values of $k$. This is what the following SageCell does. 

[^distribution]: For the experts reading these notes, I'm sloppily identifying the word "distribution" with "probably density function" rather than with "probability measure," in order to avoid having to expand the definition of "probability space" I introduced earlier! I hope this will be forgiven.

<div class="element">
<span class="label">SageCell</span>
The following code plots a graph of the function $f_k$ described above. Hit "Run." It defaults to $k = 3$, but then change the first line of the code to see what happens for other positive integers $k$! Make sure to pay attention to the scale on the axes as you explore. 
<div class="sage">
<script type="text/x-sage">
k = 3 # must be positive!
chisq = RealDistribution("chisquared", k)
chisq.plot(xmin = 0, xmax = 4*k)
</script>
</div>
</div>

Second, to say that "the values of $G$ are well approximated by a chi-square distribution with $n-1$ degrees of freedom" is to say that, for any (not necessarily finite) interval $(a, b)$, the probability that $G$ lands inside the interval $(a, b)$ is approximately
$$ \int_a^b f_k(x)\, dx. $$
Sage can compute these types of integrals for us when $a = 0$ and $b$ is finite, but we can always reduce the general case to this special case.

<div class="element">
<span class="label">SageCell</span>
The following Sage code calculates for us that 
$$ \int_0^{15.84} f_3(x)\, dx \approx 0.999. $$
Notice where the degrees of freedom 3 goes, and also where the upper endpoint of integration 15.84 goes. The `cum_distribution_function` stands for "cumulative distribution function," which means that the output is the total area from 0 up to the input value. 
<div class="sage">
<script type="text/x-sage">
k = 3
chisq = RealDistribution("chisquared", k)
chisq.cum_distribution_function(15.84)
</script>
</div>
</div>

It follows that 
$$ \int_{15.84}^\infty f_3(x)\, dx = 1 - \int_0^{15.84} f_3(x) \approx 1 - 0.999 = 0.001. $$

The number 0.001 is our *p-value*. It is telling us that the probability of observing a value of $G$ that is bigger than 15.84 is only about 0.1%! That is quite a small probability, so our calculation suggests that the value of $G$ that we saw is in fact quite large. 

Stated differently, our p-value of 0.001 tells us that, if jurors in this county were truly representative of the county's electorate, there would only be roughly a 0.1% chance of seeing a sample that deviated at least as much from the expected counts as the data that we saw. Because that's such a small probability, this suggests that it's very unlikely that our sample of jurors is actually representative of the county's electorate. We have quantified the observation we made informally above. 

If you go back and look at the statement of Wilks' Theorem, you'll see one detail that we've ignored so far, which brings us to the third point of explanation. The theorem only works for "large values of $N$." Was our value of $N = 275$ large enough to justify what we did above? 

The answer to this question depends on how well you want the values of $G$ to be approximated by a chi-square distribution. The better an approximation you want, the higher a value of $N$ you need. That being said, statisticians have found through practice that the following heuristic generally works well: 

<div class="element">
<span class="label">Heuristic Addendum to Wilks' Theorem</span>
The approximation of $G$ by a chi-square distribution with $n-1$ degrees of freedom is "good enough" as long as the vast majority of the expected counts $E_1, \dotsc, E_n$ are all at least 5. 
</div>

In our situation, all of the expected counts are well above 5, so we do not need to worry. 

This process (of computing expected counts, finding an observed value of $G$, and using a chi-square approximation to find a p-value, ie, the probability of observing a larger value of $G$ than what we observed if the observations do in fact come from the theoretical distribution) is called a *$G$-test*. It is a very useful technique for a lot of problems in statistics, and below, we will apply it to codebreaking! Meanwhile, here are some problems to practice with the ideas you've encountered here. 

<div class="element">
<span class="label">Exercise</span>
Use the `cum_distribution_function` in Sage to calculate the following integrals. 

a. $\displaystyle \int_0^{20} f_{10}(x)\, dx$ 
b. $\displaystyle \int_{10}^{\infty} f_{3}(x)\, dx$ 
c. $\displaystyle \int_{10}^{20} f_{15}(x)\, dx$ 
</div>

<div class="element">
<span class="label">Exercise</span>
A professor using an open source introductory statistics book predicts that 60% of the students will purchase a hard copy of the book, 25% will print it out from the web, and 15% will read it online. At the end of the semester she asks her students to complete a survey where they indicate what format of the book they used. Of the 126 students, 71 said they bought a hard copy of the book, 30 said they printed it out from the web, and 25 said they read it online. How well does this data fit the professor's predictions? Run a $G$-test to find out!
</div>

We conclude this section with a technical remark regarding the definition of $G$. Feel free to skip over this at first pass, but make sure to come back to it at some point soon!

<div class="element">
<span class="label">Technical Remark</span>
You'll notice that $G$ is defined to be $\infty$ if there is a nonzero observed count corresponding to an expected count of 0. This makes theoretical sense: if there's a value we theoretically expect to be *impossible* to observe, but then we observe it, the distance from the observed distribution to the expected distribution should be massive! 

In practice, if you see an expected count of 0, it's worth thinking carefully to decide if this really makes sense. We'll see in examples below that sometimes, the way we compute expected counts will lead to certain expected counts being 0 when they maybe "shouldn't" be, in which case it may be useful to "fudge" the expected count a little bit.
</div> 




