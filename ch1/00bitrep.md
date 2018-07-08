# Computation & Representation

> Objectives:
> - Refamiliarise yourself with functions as sets and their properties (total, partial, injective, surjective, bijections)
> - Understand that we can represent objects as a string (in our case, a string of ones and zeroes)
> - Familiarity with bit representations for integers, rationals, vectors, graphs etc.
> - Evaluate different representations for the same set of objects

### What do you mean by a _Representation_?

I've always taken for granted how far abstracted my mental model for programming is
from what is actually computed on. When we specify our procedures by phrases of the sort,
_"program $$P$$ takes in $$x$$ as input"_, what is actually happening is _"program $$P$$ takes in
$$x$$ in the representation of a binary string"_. Suppose the representation of $$x$$ were to change,
then the implementation details would also have to change (imagine instead of binary, the program
interpreted an integer with elements from the set $$\{0,1,;\}$$ - we'd have to reflect those changes in the implementation). This
wishy-washy example shows at least one benefit of first formally specifying your programs,
as implementations change with representations, yet specifications are impervious to these changes. We'll
now explore representing common objects as binary strings.

***

### Binary Representations of Common Objects

#### Natural Numbers

For all $$n\in\mathbb{N}$$, we can encode $$n$$ as a binary string by an injective function $$E:\mathbb{N}\rightarrow \{0,1\}^*$$
$$
E(n) = x_0x_1...x_{k-1}~~~~\text{s.t.}~~~n=\sum_{i=0}^{k-1} 2^ix_i
$$

Where $$x_i \in \{0,1\}$$. If you're anything like me and went through MATH1081 thinking that injective functions
were _just_ an $$f:X\rightarrow Y$$ such that, $$\forall x\in X(\exists y\in Y (f(x)=y))$$, then you'll
need a refresher. An injective function $$f$$ follows,
$$
\forall x,x'\in X (x \neq x' \Rightarrow f(x) \neq f(x'))
$$
Intuitively, no two elements in the domain of $$f$$ map to the same element in the codomain. An injective function does not have to be total, where we define a partial function injective, if we restrict the domain to those elements who have mappings in $$f$$ and then test injectivity. Why should this encoding function
$$E$$ be injective? Well, suppose if it were not, then there would exist two distinct objects such that their encodings were the same - leading to some questionable ambiguity on how to decode such a string!

For this $$E:\mathbb{N} \rightarrow \{0,1\}^*$$, we'll need a surjective $$D:\{0,1\}^* \rightarrow \mathbb{N}$$ to decode the binary string. Why surjective? Recalling the definition of being surjective,
$$\forall y\in Y(\exists x\in X(f(x) = y))$$, which intuitively captures that the range of $$f$$ is
the entire codomain $$Y$$. This is important for $$D$$, as we want every natural number to have some bit string that represents it. This $$D$$ can be easily defined by noting that with an injective $$f:X\rightarrow Y$$, there exists a surjective function $$g:Y\rightarrow X$$, where $$g(f(x)) = x,~~x\in X~$$. Proof is left to the reader.

**Examples**
- $$E(0) = (0)$$
- $$E(1) = (1)$$
- $$E(63) = (1,1,1,1,1,1)$$
- $$E(10) = (0,1,0,1)$$

#### Integers

To represent potential negatives, we split a $$k$$-bit sequence into the first bit (which will represent the sign of the number) and the rest (representing the magnitude of the number). We define our encoding function $$E:\mathbb{Z} \rightarrow \{0,1\}^*$$ as, 
$$
E(n) = \sigma x_1x_2...x_{k-1}~~~~\text{s.t.}~~~n=(-1)^\sigma\sum_{i=1}^{k-1}2^{i-1}x_i
$$
Leaving the obvious statement - _hey this looks exactly like the natural number encoding! You're just changing the meaning associated with the bit string positions!_ That I am, it's up to the user to assign the representation associated with the bit string. This could be seen as a natural number, or an integer,
or any other bit representation of a set of objects, but in this situtation it's an integer.

> This isn't traditionally how integers are represented in your computer. Most computers use two's complement representation as it [requires no special logic for addition over all integers](https://stackoverflow.com/questions/1125304/why-prefer-twos-complement-over-sign-and-magnitude-for-signed-numbers). The encoding function is instead, $$E(n) = \sigma x_1x_2...x_{k-1}~~~~\text{s.t.}~~~n = -\sigma2^{k-1}+\sum_{i=1}^{k-1}2^{i-1}x_i$$. For negative integers, this is the absolute difference between
from the negative extremity $$-2^{k-1}$$, and for positive integers, the difference from 0. The contention between interpreting an integer as unsigned (without the sign bit) and signed (two's complement) can lead to many bugs. Refer to the [first few lectures](http://www.cs.cmu.edu/afs/cs/academic/class/15213-s16/www/schedule.html) of CMU's 15-213, _Introduction to Computer Systems_, for much more on number representation in a computer.

#### Rational Numbers

Representing rationals is a super cool trick. At a first glance we want to represent a pair of integers $$a$$ and $$b$$ as the numerator and denominator of our fraction respectively. However, concatenating the integer representation without any notion of a separator can lead to ambiguity when we try to decode the resulting bit string. Consider $$E(3)E(-4)$$ under an integer representation this is, $$(0,1,1,1,0,0,1)$$
which also can be interpreted as $$E(1)E(-5)$$. Hence, such a function that does this sort of concatenation cannot be injective.

We need some sort of notion of a seperator, let's denote it as the symbol $$(;)$$.




