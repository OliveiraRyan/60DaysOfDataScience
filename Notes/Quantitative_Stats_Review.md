# Conditional Probability

$p(\text{what do we want to know}|\text{what we know}) = \cfrac{p(\text{what do we want to know})}{p(\text{what we know})}$

||Loves Candy|Doesn't Love Candy|Row Total|
|:-|:-:|:-:|:-:|
|**Loves Soda**|2</br>p=2/14|5</br>p=5/14|2+5=7</br>p=7/14|
|**Doesn't Love Soda**|4</br>p=4/14|3</br>p=3/14|4+3=7</br>p=7/14|
|**Column Total**|2+4=6</br>p=6/14|5+3=8</br>p=8/14||

</br>

$p(\text{not love \textbf{c}} | \text{loves \textbf{s}}) = \cfrac{5}{2+5} = 0.71$ (standard notation)

$\implies p(\text{not love \textbf{c} and loves \textbf{s}} | \text{loves \textbf{s}}) = \cfrac{5}{2+5} = 0.71$

$\implies p(\text{not love \textbf{c} and loves \textbf{s}} | \text{loves \textbf{s}}) = \cfrac{\cfrac{5}{14}}{\cfrac{2+5}{14}} = \cfrac{p(\text{not love \textbf{c} AND loves \textbf{s}})}{p(\text{loves \textbf{s}})} = 0.71$

</br>

Thus, generally:
$$p(A|B) = \cfrac{p(A \cap B)}{p(B)}$$


# Bayes' Theory

$p(A\&B | B) = \cfrac{p(A\&B|A) \cdot p(A)}{P(B)}$ (redundant notation)

$\implies p(A | B) = \cfrac{p(B|A) \cdot p(A)}{P(B)}$ (standard notation)
