<!-- slide -->

# Quantum Computing

2019/10/25

<!-- slide -->

# Qubit

We define the quantum state of one qubit as

$ |\phi\rangle = \alpha|0\rangle + \beta|1\rangle$

Where we consider

$|0\rangle = \begin{bmatrix} 0 \\ 1 \end{bmatrix} $

$|1\rangle = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$

and

$\alpha ^ 2 + \beta ^ 2 = 1$

<!-- slide -->

Two qubits

$|\phi_1\phi_2\rangle = |\phi_1\rangle \otimes | \phi_2 \rangle $ =  
$(a_0|0\rangle + a_1|1\rangle)(b_0|0\rangle+b_1|1\rangle)$ =  
$a_0b_0|00\rangle + a_0b_1|01\rangle + a_1b_0|10\rangle + a_1b_1|11\rangle$

Where

$|00\rangle = \begin{bmatrix} 1 \\ 0 \\ 0 \\ 0 \end{bmatrix}$
$|01\rangle = \begin{bmatrix} 0 \\ 1 \\ 0 \\ 0 \end{bmatrix}$
$|10\rangle = \begin{bmatrix} 0 \\ 0 \\ 1 \\ 0 \end{bmatrix}$
$|11\rangle = \begin{bmatrix} 0 \\ 0 \\ 0 \\ 1 \end{bmatrix}$

<!-- slide -->

# Single Qubit Gates

<!-- slide -->

# Identity Gate

$\bold{I}|0\rangle = |0\rangle$  
$\bold{I}|1\rangle = |1\rangle$

$\bold{I}|\phi\rangle=\bold{I}(\alpha|0\rangle + \beta|1\rangle)$  
$ = \alpha|0\rangle + \beta|1\rangle$

$\bold{I} = \begin{bmatrix} 1 &  0 \\ 0 & 1 \end{bmatrix}$

<!-- slide -->

# Not Gate

$\bold{X}|0\rangle = |1\rangle$  
$\bold{X}|1\rangle = |0\rangle$

$\bold{X}|\phi\rangle=\bold{X}(\alpha|0\rangle + \beta|1\rangle)$  
$ = \beta|0\rangle + \alpha|1\rangle$

Where

$\bold{X} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix} $

<!-- slide -->

# Hadamard Gate

Convert a single basis state to a superposition of states

$\bold{H}|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)$  
$\bold{H}|1\rangle = \frac{1}{\sqrt{2}}(|0\rangle-|1\rangle)$

Where

$\bold{H} = \frac{1}{\sqrt{2}}\begin{bmatrix} 1 & 1 \\ 1 & -1 \end{bmatrix}$

<!-- slide -->

As $\bold{H}$ is self inverse, $\bold{H} = \bold{H}^{-1}$

That means

$\bold{H}(\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)) = |0\rangle$  
$\bold{H}(\frac{1}{\sqrt{2}}(|0\rangle-|1\rangle)) = |1\rangle$

<!-- slide -->

# Two Qubits and Higher Dimensional Gates

<!-- slide -->

# C-Not Gate

$$M_{CNOT}=\begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{bmatrix}$$

Thus $M_{CNOT}$ maps $|00\rangle \rightarrow |00\rangle$, $|01\rangle \rightarrow |01\rangle$,$|10\rangle \rightarrow |11\rangle$, $|11\rangle \rightarrow |10\rangle$

Where the 1st qubit is the control, and the 2nd qubit is the target.

<!-- slide -->

# Usage of C-Not gate

To create **quantum entanglement**

Consider we apply inputs A (control) and B (target) to the C-Not gate:

$$A = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$$

$$B = |0\rangle$$

Apply A and B to the CNote gate we will get:

$$\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$

<!-- slide -->

# What is Quantum Entanglement

> Aka **Bell States**

$\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$

cannot decompose to state

$a_0b_0|00\rangle + a_0b_1|01\rangle + a_1b_0|10\rangle + a_1b_1|11\rangle$

<!-- slide -->

# Usages

<!-- slide -->

- Quantum Teleportation

![Quantum_teleportation_diagram](https://i.loli.net/2019/10/25/QABfPq8DutsFWYS.png)

<!-- slide -->

# My Project

[qubit.js](https://github.com/shd101wyy/qubit.js)  
A quantum computing circuit simulator by Yiyi Wang 2017

[presentation ](https://rawgit.com/shd101wyy/qubit.js/master/docs/qc.html)

<!-- slide -->

# Shor's Algorithm

Shor’s algorithm is an integer factorization algorithm designed by Perter Shor in 1994.  
 The fastest known classical factoring algorithm, the general number sieve works in
subexponential time, specifically $O(e^{logN}(log log N)^{2/3})$.  
Shor’s algorithm achieves the
same task in $O((logN)^3)$

<!-- slide -->

Wikipedia

> This algorithm finds the prime factorization of an n-bit integer in $O(n^3)$ time whereas the best known classical algorithm requires $2^{O(n^{1/3})}$ time and the best upper bound for the complexity of this problem is $O(2^{n/3+o(1)})$.

<!-- slide -->

How we use Shor's algorithm to break **RSA**

<!-- slide -->

# RSA

1. **We pick two prime number $p$ and $q$**  
   Lets take $p=3$ and $q=11$

2. **We compute value of $n$ and $\phi$**  
   $n = p * q$ and $\phi = (p-1)\times(q-1)$

Here we have

$$n = 3 \times 11 = 33$$  
$$\phi = (3-1)\times(11-1) = 2 \times 10 = 20$$

<!-- slide -->

3. **Find the value of $e$ (public key)**  
   Choose $e$ such that $e$ should be co-prime with $\phi$. (Co-prime means it should not mulitiply by factors of $\phi$ and also not divide by $\phi$)

Factors of $\phi$ are, $20 = 5 \times 4 = 5 \times 2 \times 2$ so $e$ should not mutiply by $5$ and $2$ and shouldn't divid by $20$

So primes are 3, 7, 11, 17, 19 ..., as 3 and 11 are taken, we pick 7 as e for example.

$$e = 7$$

<!-- slide -->

4. **Compute the value of $d$ (private key)**

$$e \times d = 1\text{ }mod\text{ }\phi$$

In our case we have
$$p=3$$
$$q=11$$
$$n=3\times11=33$$
$$\phi=(3-1)(11-1)=20$$
$$e=7$$  
We get
$$d=3$$

<!-- slide -->

5. **Encryption and Descryption**

Encrypting a message $m$ (number) with the public key $(n, e)$ is calcualted:

$$m' = m^{e} (mod\text{ }n)$$

Decrypting with the private key $(n,d)$ is done analogously with

$$m'' = m'^d(mod\text{ }n)$$  
This is
$$m''=m^{e \times d}(mod \text{ } n)$$

<!-- slide -->

RSA now exploits the property that
$$x^a = x^b(mod \text{ } n)$$
if
$$a = b (mod\text{ }\phi)$$
As $e$ and $d$ were chosen appropriately, it is
$$m'' = m$$

<!-- slide -->

So in order to crack the private key $d$, one needs to know $\phi = (p-1)(q-1)$.  
This in turn requires factoring $n = p \times q$, for which there is no known polynomial time algorithm.

<!-- slide -->

Here comes the Shor's algorithm, which is a quantum computer algorithm for integer factorization that runs in polynormial time.

Traditional algorithm might take 1 million years to factor a number, will Shor's algorithm only takes 1 second.

<!-- slide -->

Quantum circuit of Shor's algorithm

![369px-Shor's_algorithm.svg](https://i.loli.net/2019/10/25/BHFnoWmhcOSqZkP.png)

<!-- slide -->

# References

- https://www.cryptool.org/en/cto-highlights/rsa-step-by-step
- https://warwick.ac.uk/fac/sci/physics/research/cfsa/people/pastmembers/charemzam/pastprojects/mcharemza_quant_comp.pdf
- http://physlab.org/wp-content/uploads/2016/03/Abdullah-Khalid.pdf
