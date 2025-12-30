# Probability & Statistics

- My statistics is so bad back in high school so I never attempted to touch it until now. However, an inevitable motivation is here. A proper perception and prediction computer vision engineer must be able to use them, so I must learn it! 
- This note contains what I’ve learned before (a small, small amount) and what I’m going to learn (a big amount I hope). 
- Created on 12/29/2025



## Fundamentals

- **Sample space** $S$: the total number of possibilities
- **Event** $E$: a **set**, the collection of outcomes (probability operates on events, not individual outcomes)
- **General workflow**: Identify sample space ➡️ Define events ➡️ Apply the laws



### Three axioms

1. Probabilities can’t be negative: $P(E) \geq 0$. 
2. Something must happen: $P(S) = 1$. 
3. If events can’t overlap, just add: $P(A \cup B) = P(A) + P(B)$. 



### Compliment & Addition

- the **NOT** operation: 

  - There’re only two possibilities, event A happens or not. 
  - **the compliment rule**: $P(A^c) = 1 - P(A)$. 

- the **AND** operation:

  - Find the two sets’ **intersection**: $P(A \cap B)$.

- the **OR** operation:

  - Mind the overlaps! 
  - **the addition rule**: $P(A \cup B) = P(A) + P(B) - P(A \cap B)$. 

  - If two events can’t overlap: $P(A \cap B) = 0$



### Factorials

- **n factorial** 
- $n! = n \times (n-1) \times (n-2) \times ... \times 2 \times 1$ 



### Permutation & Combination

```mermaid
graph TD
    %% Node Definitions
    Start["Can I repeat?"]
    Order1["Order matters?"]
    Order2["Order matters?"]
    Result1["Use n^r"]
    Result2["Permutation P(n, r)"]
    Result3["Combination C(n, r)"]

    %% Connections
    Start -- "YES" --> Order1
    Start -- "NO" --> Order2
    
    Order1 -- "Always YES" --> Result1
    
    Order2 -- "YES" --> Result2
    Order2 -- "NO" --> Result3

    %% Styling for Light Theme
    style Start fill:#f8fafc,stroke:#334155,stroke-width:2px,color:#0f172a
    style Order1 fill:#f8fafc,stroke:#334155,stroke-width:2px,color:#0f172a
    style Order2 fill:#f8fafc,stroke:#334155,stroke-width:2px,color:#0f172a
    style Result1 fill:#f1f5f9,stroke:#64748b,stroke-width:2px,color:#0f172a,stroke-dasharray: 5 5
    style Result2 fill:#f1f5f9,stroke:#64748b,stroke-width:2px,color:#0f172a,stroke-dasharray: 5 5
    style Result3 fill:#f1f5f9,stroke:#64748b,stroke-width:2px,color:#0f172a,stroke-dasharray: 5 5

    %% Link Styling
    linkStyle default stroke:#64748b,stroke-width:2px,color:#475569
```

- If **repetition allowed**: $P(E) = n^r$. 
  - e.g. 4-digit PIN from 0~9: $ r =4, n = 10, P(E) = 10^4 = 10000$

- If **no repetition + order matters**: Permutation $P(n,r)$. 
  - Formula: $P(n,r) = \frac{n!}{(n-r)!}$ 
  - But, $P(n,r)$ is basically multiply down $r$ times. 
  - e.g. $P(10,4) = 10 \times 9 \times 8 \times 7 = 5040$. 

- If **no repetition + order doesn’t matter**: Combination $P(n,r)$. 
  - Formula: $C(n,r) = \frac{n!}{r!\ (n-r)!}$ 
  - Also valid: $C(n,r) = P(n,r) \div r!$ (**Permutation divided by redundancy**)
  - e.g. $C(10,4) = P(10,4) \div 4! = 5040 \div 24 = 210$. 



Example 1: If I have a set of letters: AAAABBBBCC. What’s the probability that I picked 2 A from the set. 

- Solution 1: the sequential way
  - pick the letters one-by-one, so that 4/10 x 3/9 = 2/15
- Solution 2: **the combination way** ⭐️
  - pick two at the same time, the ratio between successful pairs and total pairs
  - C(4,2) / C(10/2) = 2/15



### Conditional Probability

- Knowing more conditions means to **shrink the probability universe** further. 
- $P(A|B) = P(A \cap B)/P(B)$  
  - (“|” means “Given that”). 
  - $P(A \cap B)$ = A **AND** B = all outcomes where both A and B happen, **within** the $P(B)$ universe. 
  - $P(B)$ = all outcomes where B happens. **This is the new, smaller universe**. 

- E.g. 10 Marbles, 5 blue, 5 red. First marble drawn: blue. Now what’s the probability that second is red?
- P(second is Red | first was Blue) = P(Blue first AND Red second) / P(Blue first) = (5/10 x 5/9) / (5/10) = 5/9



### Marginal Probability

- A marginal probability is the sum of the joint probabilities. 

|          | Popcorn | Candy  | Soda   | (margin) |
| -------- | ------- | ------ | ------ | -------- |
| Adult    | 40      | 20     | 30     | **90**   |
| Child    | 50      | 40     | 20     | **110**  |
| (margin) | **90**  | **60** | **50** | 200      |



### Independent Events

- Independence means: A doesn’t cause B, B doesn’t cause A, no hidden factors affects both. 
- **Always check independence** before calculating **AND**. 
- 3 ways to test **independence**: 
  1. Check if $P(A \text{ AND } B) = P(A) \times P(B)$. 
  2. Check if $P(A|B) = P(A)$. 
  3. Check if knowing B doesn’t change your belief about A. 

- If independent: just multiply $P(A) \times P(B)$. 
- If dependent: use conditional probability: $P(A) \times P(B|A)$. 



### Random Variables

- A random variable is **a function that assigns a number** to every outcome in your sample space. 
- Example: flip a coin twice, head wins \$3, tail lose \$2. 
  - Sample space $S = \{\text{HH, HT, TH, TT}\}$
  - HH = \$6, HT or TH = \$1, TT = -\$4. 
  - So random variable X takes values $\{6,1,-4\}$. 



### Expected Value

- E[X], the average value in the long run. 
- **Expected value always add**, whenever they’re dependent or not. 
  - E[X + Y] = E[X] + E[Y], for any random variables. 
- **Linearity of Expectation**
  - No matter if X and Y are dependent, when we break apart the math and regroup it, the dependencies automatically washed out. We end up with the original marginal probabilities. 
  - Example 1: Food delivery charges \$1 per mile traveled and distance varies uniformly between 1~5 miles. Let X = total charge of a random delivery. 
    - If each delivery are independent: E[20 deliveries] = 20 x E[1 delivery] = 20 x \$1x(1+5)/2 = \$60
    - If each delivery are dependent (like a short one will follow a long one): E[20 deliveries] still = $60. 



### Discrete and Continuous

- Discrete variables
  - use **PMF - Probability Mass Function**. 
  - Add up probabilities in each condition. 
  - P(X = x): probability equals specific value. 
  - E.g. picking marbles, rolling dices
- Continuous variables 
  - use **PDF - Probability Density Function**. 
  - Integrate to find areas - measuring a continuous range. 
  - We can’t ask P(X = x), they’re always **zero** (infinite results gives infinite small probabilities). 



### Law of Total Probability

$P(A) = \sum{P(A|B_i) \times P(B_i)}$ for all possible scenario $B_i$. 

- This basically means break the problem into all possible paths, calculate each path’s probability, add them up. 



### Baye’s Rule

$$P(H|E) = \frac{P(H) \times P(E|H)}{P(E)} = \frac{P(H) \times P(E|H)}{P(H)P(E|H)+P(\text{not }H)P(E|\text{not }H)} = \frac{\text{Prior belief } \times \text{ Likelihood}}{\text{Evidence}}$$ 

- **Likelihood**: the probability of seeing the **Evidence ($E$ )** assuming the **Hypothesis ($H$)** is true
- **Prior belief**: the **base rate**, how common **the condition ($H$)** is *before* you look at any new evidence.
- **Evidence**: the **Total Probability** of the observation ($E$) occurring universally. 

- **Posterior**: the calculated result $P(H|E)$
- The Baye’s Rule means to always account the **base rate**. 



Example 1: Bag A has 5 red, 5 blue; Bag B has 1 red, 7 blue. 70% chance pick Bag A, 30% chance pick Bag B. What’s P(Bag A|blue)

- Initial chance: **P(Bag A)** = 70%

- Chance of blue given Bag A: **P(blue|Bag A)** = 50%

- Total chance of blue: **P(blue)** = P(blue|Bag A) + P(blue|Bag B) = 70% x 50% + 30% x 87.5% = 61.25% (law of probability)

- So, P(Bag A|blue) 

  = total chance of the “**Bag A then blue” pathway** / total chance of blue 

  =  (**P(blue|Bag A) x P(Bag A)**) / P(blue) 

  = 50% x 70% / 61.25% 

  ~= 57.1%



Example 2: A rare condition affects 0.5% of the population. A screening test has 95% sensitivity (correctly identifies those WITH condition) and 98% specificity (correctly identifies those WITHOUT condition). If someone tests positive, what’s the probability they actually have the condition? 

- Solve with sample size assumption: 

  - Assume there’re 10000 people, 50 with condition, 9950 without. 

  - Of the 50 with condition, 50 x 95% = 47.5 test positive.

  - Of the 9950 without condition, 9950 x 2% = 199 test positive.

  - Total positive = 199 + 47.5 = 246.5, **most of the positive tests are false alarms!** 

  - P(condition yes|test positive) = 47.5 / 246.5 ~= 19.3%

- Solve with direct Baye’s Rule:

  - P(condition yes|test positive) 

    = P(test positive|condition yes) x P(condition yes) / P(test positive) 

    = (95% x 0.5%) / (0.5% x 95% + 99.5% x 2%)

    ~= 19.3%



Example 3: Customers arrive uniformly between 7-9AM. Before 8AM, 80% order coffee. After 8AM, 60% order coffee. 

- What’s the overall probability a customer orders coffee? 

  - Knowing ordering during 7~8AM and during 8~9AM is 50:50 chance, we calculate:

  - P(coffee)

    = P(coffee | before 8) x P(before 8) + P(coffee | after 8) x P(after 8)

    = 80% x 50% + 60% x 50%

    = 70%

- If someone orders coffee, what’s the probability they arrived before 8AM?

  - Likelihood: P(coffee | before 8AM) = 80%

  - Prior belief: P(before 8AM) = 50%

  - Evidence: P(coffee) = 70%

  - so, P(before 8AM | coffee) 

    = (likelihood x prior belief)/evidence 

    = 80% x 50% / 70% 

    = 57.1%





### Recursive Problems

Example 1: By flipping a coin, how many flips do you expect to need before getting your first heads?

- E = 1 flip you definitely take + expected additional flips

​	= 1 + (1/2)(0) + (1/2)(E)

​	= 1 + E/2

- E = 2, meaning that I expect exactly 2 flips to get the first heads. 

```mermaid
graph LR
    %% Nodes
    Start(["Start: Calculate E"])
    Flip[/"Take 1 Flip"/]
    Result{"Outcome?"}
    
    Heads["Heads (1/2)"]
    Tails["Tails (1/2)"]
    
    End["0 additional flips needed"]
    Recurse["Back to start: E additional flips"]

    %% Connections
    Start --> Flip
    Flip --> Result
    
    Result -- "Success" --> Heads
    Heads --> End
    
    Result -- "Failure" --> Tails
    Tails --> Recurse
    
    %% The Recursive Loop
    Recurse -.->|Recursive Loop| Flip

    %% Styling
    classDef default fill:#ffffff,stroke:#334155,color:#0f172a,stroke-width:2px
    classDef result fill:#f0fdf4,stroke:#22c55e,color:#14532d,font-weight:bold
    
    class Start,Flip,Result,Heads,Tails,End,Recurse default
```

Example 2: How many flips do you expect to need before getting HH?

- E = expected flips starting from scratch
- E_H = expect additional flips after getting one H

- From scratch: E = 1 + (1/2)(E) + (1/2)(E_H)
- From one H: E_H = 1 + (1/2)(E) + (1/2)(0)

```mermaid
graph LR
    %% Direction
    direction LR

    %% Nodes
    E(("<b>State E</b><br/>(Scratch)"))
    EH(("<b>State E_H</b><br/>(1 Head)"))
    Success{{"<b>Success</b><br/>(0 flips left)"}}

    %% Transitions from E
    E -- "Tails (1/2)<br/>+1 flip" --> E
    E -- "Heads (1/2)<br/>+1 flip" --> EH

    %% Transitions from EH
    EH -- "Tails (1/2)<br/>+1 flip" --> E
    EH -- "Heads (1/2)<br/>+1 flip" --> Success

    %% Styling
    classDef default fill:#ffffff,stroke:#334155,color:#0f172a,stroke-width:2px
    classDef state fill:#f8fafc,stroke:#64748b,color:#0f172a,font-weight:bold
    classDef goal fill:#f0fdf4,stroke:#22c55e,color:#14532d,font-weight:bold

    class E,EH state
    class Success goal
```



## Reference

- [Give Me 1 Hour, I'll Make Probability Click Forever by Zachary Huang - Youtube](https://youtu.be/H6pWY2VQ9xI?si=rPtFx9PvDhdGJzZo)
- [1 Hour of Probability Practice. Nothing Else Matters. by Zachary Huang - Youtube](https://youtu.be/7vb8a0kA-fw?si=2eaISuQzoLfT7qwD)
- [Bayes theorem, the geometry of changing beliefs by 3Blue1Brown - Youtube](https://youtu.be/HZGCoVF3YvM?si=ik5iyEqaaDDUAdj)

