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



### Basic operations

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



### Basic conditions

```mermaid
graph TD
    %% Node Definitions
    Start["Can I repeat?"]
    Order1["Order matters?"]
    Order2["Order matters?"]
    Result1["Use n^k"]
    Result2["Permutation P(n, k)"]
    Result3["Combination C(n, k)"]

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

- If **repetition allowed**: $P(E) = n^k$. 
  - e.g. 4-digit PIN from 0~9: $ k =4, n = 10, P(E) = 10^4 = 10000$

- If **no repetition + order matters**: Permutation $P(n,k)$. 
  - Formula: $P(n,k) = \frac{n!}{(n-k)!}$ 
  - But, $P(n,k)$ is basically multiply down $k$ times. 
  - e.g. $P(10,4) = 10 \times 9 \times 8 \times 7 = 5040$. 

- If **no repetition + order doesn’t matter**: Combination $P(n,k)$. 
  - Formula: $C(n,k) = \frac{n!}{k!\ (n-k)!}$ 
  - Also valid: $C(n,k) = P(n,k) \div k!$ (**Permutation divided by redundancy**)
  - e.g. $C(10,4) = P(10,4) \div 4! = 5040 \div 24 = 210$. 









## Reference

- [Give Me 1 Hour, I'll Make Probability Click Forever by Zachary Huang - Youtube](https://youtu.be/H6pWY2VQ9xI?si=rPtFx9PvDhdGJzZo)
- 