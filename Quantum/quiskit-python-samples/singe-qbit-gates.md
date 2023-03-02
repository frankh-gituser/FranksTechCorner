### Summary: Single qubit gates - Pt 1 <a class="anchor" id="single_qubit_gates_pt1"></a>

#### Visualizing qubits

The states $\ket{0} = \begin{pmatrix}1\\0\end{pmatrix}$ and $\ket{1} \begin{pmatrix}0\\1\end{pmatrix}$ form an orthonormal basis. 
Commonly this is also used as computational basis.


The states $$ \ket{0} = \begin{pmatrix}1\\0\end{pmatrix}$$ and $\ket{1} $$\begin{pmatrix}0\\1\end{pmatrix}$$

$$ \begin{bmatrix}a & b \\ c & d\end{bmatrix} $$
$$ \begin{pmatrix}a & b \\ c & d\end{pmatrix} $$

$\sum_{n=1}^{10} n^2$

$\sqrt{a^2+b^2} = 1$

The states $$ \begin \\ket{0} = $$

The states $\ket{0} = \begin{pmatrix}1\\0\end{pmatrix}$ and $\ket{1} \begin{pmatrix}0\\1\end{pmatrix}$ form an orthonormal basis. 
Commonly this is also used as computational basis.

Any state $\ket{\psi}$ can be represented as a complex linear combination of these two states, i.e. 

$$\ket{\psi} =  a \ket{0} + b \ket{1} \; \text{with} \; a,b \in \mathbb{C}$$

Any state $\ket{\psi}$ can be represented as a complex linear combination of these two states, i.e. $$\ket{\psi} =  a \ket{0} + b \ket{1} \; \text{with} \; a,b \in \mathbb{C}$$

**Note:**
- we can only measure phase difference between $\ket{0}$ and $\ket{1}$, i.e. $\ket{\psi} = a \ket{0} + e^{i \phi} b \ket{1} \; \text{with} \;  a,b,\phi \in \mathbb{R}$

Using the fact that the qubit state must be normalized, i.e. $\sqrt{a^2+b^2} = 1$ and the trigonometric identity $\sqrt{\sin^2x+\cos^2x} = 1$, allows to describe a state as
$$\ket{\psi}= \cos\frac{\theta}{2} \ket{0} + e^{i\phi}\sin\frac{\theta}{2} \ket{1}\; \text{with} \;  \theta,\phi \in \mathbb{R}.$$

This mapping allows to visualize any single qubit state on a sphere [Bloch Sphere](https://javafxpert.github.io/grok-bloch/). 


#### Single qubit gates

A qubit is the fundamental unit of quantum computation. Applying gate operations on a qubit manipulates the qubit state. Runing a quantum circuit means applying gate operations consecutively.

The single qubit gates $\textbf{X}$, $\textbf{Y}$, $\textbf{Z}$ perform a $180^{\circ}$ rotation of the state around  the corresponding axis. 

The $\textbf{H}$ (Hadamard) gate can be used to create an equal superposition between the $\ket{0}$ and $\ket{1}$ state. 

