```python
import numpy as np

# Importing standard Qiskit libraries
from qiskit import QuantumCircuit, transpile, Aer, IBMQ
from qiskit.tools.jupyter import *
from qiskit.visualization import *
from ibm_quantum_widgets import *
from qiskit.providers.aer import QasmSimulator

# Loading your IBM Quantum account(s)
provider = IBMQ.load_account()
```

    <frozen importlib._bootstrap>:219: RuntimeWarning: scipy._lib.messagestream.MessageStream size changed, may indicate binary incompatibility. Expected 56 from C header, got 64 from PyObject


Import Basic Functions First


```python
import qiskit
from qiskit import ClassicalRegister, QuantumRegister, QuantumCircuit
from qiskit import execute, BasicAer, Aer
from qiskit.tools.visualization import plot_histogram, circuit_drawer

from qiskit.visualization import plot_state_qsphere
from qiskit.visualization import plot_bloch_multivector, array_to_latex
from numpy import sqrt, pi
```

https://slides.com/javafxpert/prep-qiskit-dev-cert-exam#/21

## Qiskit API

Sample Questions can be found here: 
https://slides.com/javafxpert/prep-qiskit-dev-cert-exam#/21


### Which statement will create a quantum circuit with four quantum bits and four classical bits?

A. QuantumCircuit (4,4)

B. QuantumCircuit (4)

С. QuantumCircuit (QuantumRegister (4, 'grO'),QuantumRegister (4, 'crI'))

D. QuantumCircuit ([4,4])




```python
qc = QuantumCircuit (4,4)
#QuantumCircuit (4)
#QuantumCircuit (QuantumRegister (4, 'grO'),QuantumRegister (4, 'crI'))
#QuantumCircuit ([4, 4])
qc.draw()
```




    
![png](output_5_0.png)
    



QuantumCircuit (4, 3) 

Ein QuantumCircuit mit 4 qubits und 3 klassischen Bits


https://qiskit.org/documentation/stubs/qiskit.circuit.QuantumCircuit.html

## Assuming the fragment below, which three code fragments would produce the circuit illustrated ?

inp reg = QuantumRegister (2, name='inp")

ancilla = QuantumRegister (1, name='anc')

qo = QuantumCircuit (in reg, ancilla)

# Insert code here

![image logo](/quiskit-python-samples/MyFirstSample/sampe-question-3.png)

A. go.h (inp reg)
qc.× (ancilla)
qc.draw ()

B. gc.h(inp reg [0:2])
qc.x(ancilla [0])
qc.draw ()

C. qc.h(inp reg [0:1])
qc.x (ancilla [0])
qc.draw ( )

D. go.h (inp reg [0]) qc.h (inp reg [1])
qc.× (ancilla 01)
qc.draw ()

E. qc.h(inp reg [1]) qc.h(inp reg [2])
qc.×(ancilla [1])
qc.draw ()

F. qc.h (inp reg) qc.h(inp reg)
qc.x (ancilla)
ac.draw ()


https://qiskit.org/documentation/stubs/qiskit.circuit.QuantumRegister.html




```python
from qiskit import QuantumRegister, ClassicalRegister, QuantumCircuit
qr = QuantumRegister (3,'g')
anc = QuantumRegister(1, 'ancilla')
cr = ClassicalRegister (3, 'c')
qc = QuantumCircuit (qr, anc, cr)
qc.x (anc [0])
qc.h (anc [0])
qc.h(qr [0:3])
qc.cx(qr[0:3], anc[0])
qc.x (anc [0])
qc.barrier (qr)
#qc = QuantumCircuit (qr, anc, cI)
qc.measure (qr,cr)
qc.draw ()
```




    
![png](output_8_0.png)
    



### Lösung:

A. 
    go.h (inp reg) qc.× (ancilla) qc.draw ()

B. 
    gc.h(inp reg [0:2]) qc.x(ancilla [0]) qc.draw ()

D. 
    go.h (inp reg [0]) qc.h (inp reg [1]) qc.× (ancilla 01) qc.draw ()


```python

```

## measure vs. measure_all
### 4. Given an empty QuantumCircuit object, q, with three qubits and three classical bits, which one of these code fragments would create this circuit?

    A. qc.measure ( [0,1,2], [0,1,2])
    B. qc.measure ([0,0], [1,1], [2,2] )
    C. qc.measure.all ()
    D. qc.measure (0,1,2 )



```python
qc = QuantumCircuit (3,3)
qc.measure([0,1,2],[0,1,2])
qc.draw()
```




    
![png](output_12_0.png)
    




```python
qc.measure ([0,0],[1,1],[2,2] )
```

    Traceback [1;36m(most recent call last)[0m:
    [1;36m  Input [1;32mIn [25][1;36m in [1;35m<cell line: 1>[1;36m[0m
    [1;33m    qc.measure ([0,0],[1,1],[2,2] )[0m
    [1;31mTypeError[0m[1;31m:[0m measure() takes 3 positional arguments but 4 were given
    
    Use %tb to get the full traceback.




<style>
.button {
  border: none;
  color: white;
  padding: 4px 8px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 12px;
  margin: 4px 2px;
  transition-duration: 0.2s;
  cursor: pointer;
}
.iqx-button {
  background-color: #0f62fe; 
  color: white; 
}
.iqx-button:hover {
  background-color: #0043ce;
  color: white;
}
</style>
<a href="https://stackoverflow.com/search?q=TypeError: measure() takes 3 positional arguments but 4 were given" target='_blank'><button class='button iqx-button'>Search for solution online</button></a>




```python
qc.measure_all ()
```


```python
qc.measure (0,1,2)
```

Solution: https://qiskit.org/documentation/stubs/qiskit.circuit.QuantumCircuit.html

![image logo](/images_samples/measure_vs_measure_all.png)

qc.measure ([0,1,2],[0,1,2])
