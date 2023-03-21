# 

[comment]: <> (This is a comment, it will not be included)
[//]: <> (This is also a comment.)

### [zurück zum Index](index.md)


## Einen Quantencomputer programmieren

Einen Quantencomputer kann inzwischen jeder bequem von zu Hause aus programmieren. 
Aber was soll man programmieren? 
Und was ist überhaupt ein Quantenprogramm, ein Quantencomputer bzw Quantencomputing ?

Da mich das Thema interessiert, habe ich mir vorgenommen, diese und weitere Fragen im Rahmen einer Zertifizierung zum Quantencomputer-Entwickler zu vertiefen.
Die notwendigen Voraussetzungen für eine Zertifizierung sind hier

[IBM Certified Associate Developer - Quantum Computation using Qiskit v0.2X ](https://www.ibm.com/training/certification/C0010300)

Beschrieben.

Nach bestandener Prüfung erhält man dann das entsprechende Zertifikat:

[Credly: IBM Certified Associate Developer - Quantum Computation using Qiskit v0.2X ](https://www.credly.com/badges/65586d0d-ba5e-4698-ab0f-fd3448cdd6b2/public_url)



Das Verständnis über Quantencomputing und die Themen Quantenschaltungen, Qubit-Gates, Bloch-Sphären und das Qiskit SDK sind Teil der zu lernenden Themen zum  Quantencomputer-Entwickler. 

Quantencomputing hat es das Potenzial, das Computing zu revolutionieren und ein wichtiges technologisches Werkzeug für Entwickler, Ingenieure und Personalverantwortliche in Unternehmen zu werden. Sowohl im Bereich der Hardware, also der Technologie, auf der Quantencomputer basieren als auch im Bereich der Software, mit dem diese neuen Computersysteme programmiert werden, hat sich ein weltweiter Wettkampf entfacht. 



Auf den folgenden Seiten werden die verschiedenen Themen vorgestellt, einschließlich der Definition, Ausführung und Visualisierung von Quantenschaltungen mit Qiskit. In Notebook Beispielen werden auch Einzel- und Multi-QuBit-Gatter und die damit verbundenen Rotationen auf der Bloch-Sphäre. In den Bespielen wird dann einfacher verständlich, wie man das Qiskit SDK verwendet, um Quantencomputeranwendungen zu implementieren und mit der Programmierung eigener Quantencomputerprogramme zu beginnen.

Hierzu gibt es dann sowohl die Möglichkeit, die Beispiele in der Cloud Umgebung von IBM 

[IBM Quantum Lab ](https://quantum-computing.ibm.com/lab)

oder in seiner eigenen lokal installierten Python+Quiskit Entwicklungsumgebung.


# Table of contents

1. [My Personal Experience Taking the Certification ExamIBM Quantum Composer](#personalexperience)
2. [Creating Logic Gates with IBM Quantum Composer](#composer)
3. [Messung der Ausgaben im IBM Quantum Composer](#measuring)
4. [Vorhersage der Ausgabe von Quantenschaltungen] (#predicting)
[X Gate (NOT)]
[Y Gate]
[CNOT Gate (AND)]
[NAND Gate]
[XOR Gate]
[OR Gate]
[Quantum Gate Icons]
[Initializing Qubits Using State Vectors]
[Two Methods to Load a State Vector]
[Calculating the Depth of a Quantum Circuit]
[Drawing a Quantum Circuit]
[Plotting a Bloch Sphere]
[Initializing a Qubit in Superposition]
[Initializing a Qubit to a Variable Degree]
[Collapsing a Superposition]
[Quantum Circuit Identities]
[Creating an X-gate from HZH]
[Entanglement]
[Bell States]
[Swapping Two Qubits]
[Swapping N Qubits]
[GHZ State]
[A Unitary Matrix for a Quantum Circuit]
[Phase Kickback]
[Barrier Operations]
[Qiskit Version]
[Running on IBMQ]
[Monitoring the Status of a Job]
[Visualizing Quantum Circuits]
[OpenQASM]
[Plotting Gate Maps and Error Rates]
[Creating Phase on Qubits]
[Fidelity]
[Density Matrix]
[Creating Custom Gates]
[Composing Circuits from Other Circuits]
[Decomposing a Quantum Circuit]
[Adding Controls to Gates]


## Meine persönliche Erfahrungen mit Quiskit und dem Weg zum Quiskit Developer <a name="personalexperience"></a>

Meine eigene Quantum-Geschichte ist natürlich eng mit meiner eigentlichen beruflichen Aufgabe bei IBM verbunden, dh wo entwickeln sich neue Technologien und in welchen Bereichen werden diese Technologien möglicherweise zum Einsatz kommen. 


Ich bin fest davon überzeugt, dass das Quantencomputing das unglaubliche Potenzial hat, die Art und Weise, wie Computer eingesetzt werden, zu revolutionieren. Die Programmierer von heute werden sich weiterbilden müssen, um mit der sich ständig verändernden Welt der Technologie Schritt zu halten. Wir haben bereits gesehen, wie schnell sich die Dinge mit künstlicher Intelligenz und maschinellem Lernen ändern können. Mit chatGPT und openAI bieten sich neue und immer mächtigere Möglichkeiten im Zusammenhang mit Large Language Models . Möglicherweise erleben wir auch gerade den Beginn der Quantencomputer-Revolution.

Vor diesem Hintergrund macht es aus meiner Sicht, auch und gerade als Software Entwickler sich frühzeitig mit dem Thema zu beschäftigen , um die Potentiale für Aufgabenstellungen zu nutzen, die heute mit klassischen Computern nur mit sehr grossem Aufwand oder noch gar nicht mit vertretbarem Aufwand gelöst werden können.



## Erstellung von Quantenschaltungen im Webfrontend / Browser <a name="composer"></a>

Der IBM Quantum Composer [IBM Quantum Composer ](https://quantum-computing.ibm.com/composer/files/new) ist eine grafische, webbasierte Anwendung zur Visualisierung und Erstellung von Quantenschaltungen. 


Sie ermöglicht es, durch Ziehen und Ablegen von Quantencomputer-Schaltkreisen (d. h. Operationen) Quantenschaltungen zu erstellen und sie in einem Simulator oder auf einem echten Quantencomputer bei IBM auszuführen.


![image logo](images/quantum-composer.png)

Schauen wir uns kurz an, wie man mit Qubits (Quantencomputer-Bits) und der IBM Quantum Composer-Anwendung einige grundlegende logische Gatter erstellt.



## Erstellen von Logic Gates mit IBM Quantum Composer <a name="logic-gates"></a>

Quantencomputer-Bits werden als sog. "Qubits" bezeichnet. Genau wie ein klassisches Bit in einem Computer können Qubits den Wert 0 oder 1 darstellen. In einem Quantencomputer können Qubits jedoch überlagert werden, so dass sie gleichzeitig die Werte 0 und 1 oder jeden beliebigen Wert dazwischen annehmen können.


Wenn wir die Qubits nicht in Superposition bringen, verhalten sie sich wie klassische Bits, was zu einer direkten Messung von entweder 0 oder 1 führt. Das macht es uns leichter, mit Qubits und dem IBM Quantum Composer zu experimentieren, indem wir logische Gatter zur Durchführung von Operationen in einem Quantenschaltkreis erstellen. 


Zu den grundlegendsten Gattern gehören NOT, AND, OR, NAND und XOR. Im Folgenden schauen wir uns an, wie jedes dieser logischen Gatter im Quantencomputing mit Qubits und dem IBM Quantum Composer erstellt werden kann. Wir werden auch sehen, wie wir jedes dieser logischen Gatter sowohl auf dem IBM Quantum Composer als auch in Python mit Qiskit nachbilden können.


Wir beginnen mit den einfachsten Gattern, der Menge der Pauli-Gatter (X, Y, Z) (https://en.wikipedia.org/wiki/Quantum_logic_gate#Pauli_gates_%28X,Y,Z%29).


### X Gate (NOT)

The most simplest of the logic gates is the Pauli X-gate, also called the NOT gate. This gate simply inverts the value of a qubit from 0 to 1 or 1 to 0. Due to this simple inversion of the value, the Pauli X-gate is often referred to as a bit-flip, since it “flips the bits” of the affected qubits.

Der einfachste der logic gates, also Schaltkreise, ist das Pauli X-Gate, auch NOT-Gate genannt. Dieses Gate invertiert einfach den Wert eines Qubits von 0 auf 1 oder 1 auf 0. 
Aufgrund dieser einfachen Umkehrung des Werts wird das Pauli X-Gate oft als "Bit-Flip" bezeichnet, da es die Bits der betroffenen Qubits "umdreht".

Im Composer stellt sich das dann wie folgt dar:


![image logo](images/x-gate-composer.png)


Im obigen Screenshot ist zu sehen, wie ein einzelnes X-Gate auf ein Qubit angewendet wird, gefolgt von einer Messanweisung. 
Die sich daraus ergebenden Wahrscheinlichkeiten im blauen Balkendiagramm zeigen eine 100%ige Wahrscheinlichkeit, dass das Qubit eine 1 misst.

Wir können diese einfache Schaltung im IBM Quantum Composer erstellen, indem wir ein NOT-Gatter, gekennzeichnet durch das Pluszeichen mit einem Kreis, auf ein einzelnes Qubit ziehen, gefolgt von einem Messsymbol.

Da Qubits mit einem Standardzustand von 0 initialisiert werden, ergibt unsere Messung des Qubits nach Anwendung des NOT-Gatters den Wert 1.

Im folgenden Beispiel wird dies anhand von Python Qiskit gezeigt.

	from qiskit import qiskit, QuantumCircuit

	# Create a quantum circuit with 1 qubit and 1 classical bit.
	qc = QuantumCircuit(1, 1)

	# Apply a NOT gate on qubit 0.
	qc.x(0)

	# Measure qubit 0.
	qc.measure(0, 0)

	job = qiskit.execute(qc, qiskit.BasicAer.get_backend('qasm_simulator'))
	print(job.result().get_counts())

Im obigen Beispiel erstellen wir einen QuantumCircuit mit einem Qubit und einem klassischen Bit. 
Wir wenden das Pauli X-Gate an, um das Qubit von einem Standardwert von 0 in einen Wert von 1 umzuwandeln, und messen schließlich das resultierende Qubit in einem klassischen Register. 
Wir lassen die Schaltung im Simulator lassen uns die Ergebnisse jeweils ausgeben:


	1 {'1': 1024}

	1 {'0': 1024}


Wenn wir zwei X-Gates nacheinander schalten, sehen wir, dass das Qubit von 1 auf 0 wechselt.


In meinem folgenden Jupyter Notebook findet ihr weitere Beispiele für Single Gates und wie man die Ergebnisse im Quantum Lab visualisieren kann. In der Vorbereitung auf die Prüfung fand ich es hilfreich, die Funktionen in dem Quantum Lab einfach mal auszuprobieren, d.h. durch Verändern der Parameter direkt zu sehen, wie sich das auf das jeweilige Ergebnis auswirkt. Das macht die Sache wenigstens etwas greifbarer, da das Arbeiten mit einer simplen Quanten Schaltung als solches noch nicht so wahnsinnig spannend ist :-)

[Single Gates - X, Y, Z-Gate und Hadamard](quiskit-python-samples/prep-single-gates-xyz/prep-single-gates-xyz.md)




## Messung der Ausgaben im IBM Quantum Composer <a name="measuring"></a>

Da wir gerade das X-Gate zur Invertierung eines Qubits vorgestellt haben, ist dies ein guter Zeitpunkt, um zu zeigen, wie man das Wahrscheinlichkeits-Balkendiagramm des IBM Quantum Composers liest, um die Ausgabe eines Quantenschaltkreises zu verstehen.

Im obigen Beispielwerden in dem blauen Balkendiagramm die Wahrscheinlichkeiten für 0 und 1 für ein einzelnes Qubit anzeigt. 
In diesem Fall war das Ergebnis eine 100%ige Wahrscheinlichkeit, eine 1 zu messen (oder einfach ein Wert von 1).

Die Ausgangswahrscheinlichkeiten für einen Quantenschaltkreis werden in diesem Balkendiagramm auf IBM Quantum Composer angezeigt. 
Würden zwei Qubits in der Schaltung verwendet, würde das Diagramm Striche für 00, 01, 10, 11 enthalten. 
Bei drei Qubits wären es 000, 001, 010, 011, 100, 101, 110, 111. 
Auf diese Weise können Sie sehen, zu welchem Ergebnis das Qubit nach Anwendung einer Reihe von Quantengattern führen würde.

Sie können die Änderungen in der Ausgabe der Qubits auch mit dem IBM Quantum Composer Inspector Tool schrittweise nachvollziehen. 
Aktivieren Sie dazu Inspect Button.

![image logo](images/inspect.png)


Sie können dann die Ausführung jedes logischen Gatters im Quantenschaltkreis durchspielen und die Änderungen Schritt für Schritt sehen.


![image logo](images/composer-inspect.png)


Es ist wichtig zu beachten, dass die im Balkendiagramm angezeigten Bits von rechts nach links gelesen werden, entsprechend jedem Qubit im Schaltkreis (von 0 ... i). 
Wenn der Quantenschaltkreis zum Beispiel zwei Qubits (q0, q1) hat und das erste Qubit den Zustand 1 hat (q0=1, q1=0), würde das Ausgangsdiagramm 01 = 1 anzeigen. 
Von rechts nach links gelesen hat q0 den Wert 1 und q1 den Wert 0, und dieses Ergebnis (01) hat eine Wahrscheinlichkeit von 100 % - was bedeutet, dass die Schaltung einen Ergebniswert von 01 hat.


![image logo](images/reading-output.png)


Im dem Screenshot sieht man, dass der Quantenschaltkreis drei Qubits enthält. 
Die ersten beiden Qubits haben den Standardwert 0 und das dritte Qubit ist invertiert und hat den Wert 1. 
Die resultierende Ausgabewahrscheinlichkeit in IBM Quantum Composer wird als 100 mit einer Wahrscheinlichkeit von 100 % angezeigt. 
Von rechts nach links gelesen: 

q0=0, q1=0, q2=1.

Nachdem wir jetzt gesehen haben, wie man die Ausgabe eines Quantenschaltkreises in IBM Quantum Composer ablesen kann, sehen wir uns als nächstes an, wie man die Ausgabe eines beliebigen Quantenschaltkreises vorhersagen kann.



## Vorhersage der Ausgabe von Quantenschaltungen <a name="predicting"></a>

Quantenschaltungen werden mit Qubits aufgebaut, die einem beliebigen Wert zwischen 0 und 1 entsprechen. 
Befindet sich ein Qubit nicht in der Superposition (Überlagerung), gibt es einen eindeutigen Wert von 0 oder 1 aus. 
Befindet sich ein Qubit jedoch in der Superposition, so hält es gleichzeitig den Wert 0 und 1. 
Aus diesem Grund kann es etwas schwierig sein, die Ausgabe eines Quantenschaltkreises vorherzusagen, insbesondere wenn er Qubits in superpositions enthält.

Sehen wir uns also ein paar Beispielschaltungen für Quantencomputer an und lernen, wie man ihr Ergebnis vorhersagen kann. 
Wir beginnen mit einer Quantenschaltung, die ein einzelnes Qubit enthält.

### Vorhersage des Outputs eines Qubits

Nehmen wir an, wir haben einen Quantenschaltkreis, der aus einem einzigen Qubit besteht. 
Wenn sich das Qubit nicht in Superposition befindet, ist es einfach, seinen Wert als 0 oder 1 vorherzusagen.

```python
qc = QuantumCircuit(1, 1)
qc.measure(0, 0)
backend = BasicAer.get_backend('qasm_simulator')
result = execute(qc, backend, shots=1000).result()
print(result.get_counts())
display(qc.draw('mpl'))
```

Alle Messungen (**shots**) an dieser Schaltung ergeben eine 0. 
Wenn wir den Bit-Flip-Operator (**X-Gate**) anwenden würden, wäre der Wert 1. Das Ergebnis der obigen Schaltung ist unten dargestellt.


	{'0': 1000}


![image logo](images/prdict-single.png)


Versetzen wir das Qubit in Superposition und schauen die Vorhersageergebnisse an: 
Da das Qubit nun sowohl den Wert 0 als auch den Wert 1 enthält, wäre zu erwarten, dass das Ergebnis 50/50 aufgeteilt wird, d.h.: 
{'0': 500, '1': 500}. 
Schauen wir uns das Ergebnis an.

```python
qc = QuantumCircuit(2, 2)
qc.measure([0,1], [0,1])
result = execute(qc, backend, shots=1000).result()
print(result.get_counts())
display(qc.draw('mpl'))
```

Wie man sieht, ist das Ergebnis gleichmässig verteilt:


	{'0': 503, '1': 497}

![image logo](images/prdict-multi.png)


### Vorhersage des Outputs von zwei Qubits


Gehen wir jetzt einen Schritt weiter und verwenden als nächstes zwei Qubits. 
Es ist wichtig, mit der Vorhersage der Messwerte von Quantenschaltungen vertraut zu sein, um Quantencomputerschaltungen genau aufbauen und analysieren zu können 
(insbesondere für die Zertifizierungsprüfung!).











