# Wordle Solver: Optimizing First Guesses with Shannon Entropy

## 1. Introduction

This project explores how Shannon entropy and information theory can be used to optimize the first guess in Wordle. By maximizing the information gained from the initial feedback, the goal is to reduce the number of guesses needed to solve the puzzle. The approach models Wordle as a two-player Renyi-Ulam game and uses entropy calculations to evaluate possible opening moves.

## 2. Background: Renyi-Ulam Game & Information Theory

### 2.1 The Renyi-Ulam Game

The Renyi-Ulam game is a two-player game where one player secretly selects an element from a set *E*, and the other tries to identify it by asking questions. In the context of Wordle:

- **Player 1:** Selects a secret Wordle word (*X*) from the dictionary.
- **Player 2:** Makes guesses and receives feedback (correct letters in the correct place, correct letters in the wrong place, incorrect letters).

### 2.2 Shannon Entropy & Information

Shannon entropy quantifies the average information gained by asking a question. For a binary question "x ∈ A?" where *A* is a subset of *E*, the information content is:

$$
I(A) = -\log_2(|A|/|E|) = -\log_2(P(X \in A))
$$

The entropy of a question is the expected value of this information:

$$
H(A) = E(I(A)) = E(-\log_2(P(X \in A)))
$$

For a Bernoulli distribution with probability *p*, the entropy simplifies to:

$$
H(A) = -p \log_2(p) - (1-p) \log_2(1-p)
$$

## 3. Formalizing Wordle for Analysis

### 3.1 Variables and Functions

- **X:** Random variable representing the secret word chosen by Player 1, drawn from the dictionary.
- **P:** A guess made by Player 2, effectively a set of binary questions about *X*.
- **E(P, X):** Function evaluating guess *P* against secret word *X*, returning an array of 5 digits (0, 1, or 2) as feedback. For example:


    $$
    E(\text{ADIEU}, \text{IDEAL}) = \{1, 2, 1, 1, 0\}
    $$

    ![ADIEU](/adieu.png)
    
    *Figure 1: Explanation of feedback array [1,2,1,1,0] for guess 'ADIEU' against target 'IDEAL'.  
    Green (2): 'I' is in correct position  
    Yellow (1): 'A', 'D', 'E' are correct letters in wrong positions  
    Gray (0): 'U' is not in the target word*

### 3.2 Entropy of a Word

The entropy of a word *M* is the expected information gained from evaluating it:

$$
H(M) = E(I(E^{-1}(\{E(M, X)\})))
         = E\left(-\sum_{\{\epsilon_1,...,\epsilon_5\}} \log_2(P(X \in E^{-1}(\{\epsilon_1,...,\epsilon_5\})))\right)
$$

![entropie](/entropie.png)
    
*Figure 2: Depth enhancement in the algorithm to consider more possibilities.*

## 4. Methodology: Greedy Algorithm & Improvements

### 4.1 Initial Approach: Greedy Algorithm

The initial method uses a greedy algorithm to select the first guess, choosing words that maximize entropy based on expected information gain from feedback.

### 4.2 Dictionaries and Frequency Considerations

Two dictionary are considered:

- **Full Dictionary (8000 words):** All possible words.
- **Wordle-Used Dictionary (1000 words):** Focuses on commonly used Wordle words for a more realistic assessment.

### 4.3 Addressing Depth Issues

The greedy algorithm may converge on difficult words due to short-sighted optimization. The project explores increasing the search "depth" to address this.

![profondeur](/profondeur.png)
    
*Figure 3: Depth enhancement in the algorithm to consider more possibilities.*

## 5. Results and Comparison

Performance is compared between:

- **Greedy Algorithm:** Based solely on entropy maximization.
- **Depth-Enhanced Algorithm:** Considers a broader range of possibilities.

| Greedy Algorithm |         | Depth-Enhanced Algorithm |         | Comparison      |                        |
|:---------------:|:-------:|:-----------------------:|:-------:|:---------------:|:----------------------:|
| Word            | Entropy | First Word              | Entropy | Word            | Avg. Steps to Solution |
| TARIE           | 6.1778  | RAIES                   | 6.9690  | TARIE           | 4.1065                 |
| RAIES           | 6.1256  | TAIRE                   | 6.9571  | RAIES           | 4.1698                 |
| TAIRE           | 6.0902  | AIRES                   | 6.9428  | TAIRE           | 4.1216                 |
| AIRES           | 6.0895  | TARES                   | 6.9233  | AIRES           | 4.1779                 |
| TARES           | 6.0670  | TIRES                   | 6.8756  | TARES           | 4.1126                 |
| TIARE           | 6.0345  | RATES                   | 6.8741  | TIARE           | 4.1281                 |
| RAINE           | 6.0144  | CARIE                   | 6.8740  | RAINE           | 4.1397                 |
| TIRES           | 6.0057  | TIREE                   | 6.8181  | TIRES           | 4.1266                 |
| CARIE           | 6.0036  | RITES                   | 6.8170  | CARIE           | 4.1226                 |



![comparaison ouvreur](/Figure_1.png)
    
*Figure 4: comparaison of first guesses between greedy and depth-enhanced algorithms.*