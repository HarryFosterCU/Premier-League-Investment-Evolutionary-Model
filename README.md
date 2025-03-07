# README: Evolutionary Model of Investment Strategies in the Premier League  

## Overview  
This report presents an evolutionary model that analyzes investment strategies in the English Premier League. It examines how clubs' financial decisions evolve over time based on the strategic decisions and external influences of the competition. The study models club investments as a dynamic system using the Moran process, exploring how different spending strategies perform in a simulated environment.  

## Contents  
- **Introduction**: Explains the motivation behind the study and the importance of investment strategies in football.  
- **Model Formulation**: Defines the Premier League-inspired model, sources of revenue, and the mathematical framework used for investment evolution.  
- **Analysis**: Examines the stability of different spending strategies and their long-term impact on club profitability.  
- **Mutation Model**: Introduces financial penalties and ownership changes to simulate external influences on club strategies.  
- **Final Remarks**: Summarizes key findings, highlighting the financial instability of football club ownership.  
- **Appendix**: Lists sources used for revenue estimations and investment trends.  

## Key Findings  
- Budget investment strategies become obsolete over time, with mid-to-high spending strategies dominating.  
- The majority of Premier League clubs are expected to operate at a financial loss in the long term.  
- Random financial shifts, such as ownership changes or financial fair play penalties, temporarily disrupt but do not significantly alter overall investment patterns.  

## The Moran Process
A Moran Process models evolution in a fixed population by the following process:

1) Begin with a population $\textbf{v} = (v_1, v_2, ..., v_n)$
2) Each $v_i \in \textbf{v}$ represents the number of individuals in the population following strategy i
3) Each strategy i has a fitness depending on the current population, given by some function $F_i(\textbf{v})$
4) In each generation, one individual in the population is selected for duplication with a probabilitiy proportional to it's fitness. 
This value is calculated by: $\newline$
$P_{selection}(i, \textbf{v}) = \frac{v_i F_i(\textbf{v})}{\sum_{j=1}^n (v_j F_j(\textbf{v}))}$
5) In each generation, one individual is selected to be removed with probabilitiy proportional to it's presence in the population, given by $\newline$
$P_{Removal}(i, \textbf{v}) = \frac{v_i}{\sum_{j=1}^n (v_j)}$
6) One individual may mutate, with a probability given by the values in a predetermined mutation matrix M.

## How to use the code
- The code can be altered to simulate the model differently.
- The payoff matrix "A" gives the average points. It can be updated to alter the effect that spending has on games. As standard it is set to
$\begin{bmatrix}
1.6 & 1.3 & 1.0 & 0.8 & 0.5 \\
1.9 & 1.6 & 1.3 & 1.0 & 0.8 \\
2.1 & 1.9 & 1.6 & 1.3 & 1.0 \\
2.3 & 2.1 & 1.9 & 1.6 & 1.3 \\
2.5 & 2.3 & 2.1 & 1.8 & 1.5
\end{bmatrix}$

The lower down the rows, the more you spend. The further along the collums, the more your opponent has spent.

- The Cost array gives the average amounts spent per year in the different spending brackets. As default it is set to $\newline$[15.91, 37.1, 63.47, 110.47, 209.89]

- Runs is the total generations (seasons) we run the simulation for. Default set to 500 as to produce nice graphs which reach valid conclusions. 100, 1000, and 5000 are other standard values

- Get_Profit returns the profit as calculated by the regression model $P(i,\textbf{v}) = -(1059.558838 + i) + 51.712382X_i(\textbf{v}) - 0.953450X_i(\textbf{v})^2 + 0.009183X_i(\textbf{v})^3 - 0.000034X_i(\textbf{v})^4$, for population $\textbf{v}$ when spending £i. The Fitness of a strategy is given by $F_{i}(\textbf{v}) = e^{0.04 P_{i}(\textbf{v})}$ and is calculated in the Get_Fitness function

- Mut_Matrix gives the base probabilities of certain mutations. The areas with "1" will be replaced with $\newline$
$p_{i,j}$ = $0.005(1 - e^{\frac{F_j(\textbf{v})}{F_j(\textbf{v})}}$) for (i,j) such that ${F_i(\textbf{v})} < F_j(\textbf{v})$

