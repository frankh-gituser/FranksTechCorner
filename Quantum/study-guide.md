# 

[comment]: <> (This is a comment, it will not be included)
[//]: <> (This is also a comment.)

### [zurück zum Index](../index.md)


## Einen Quantencomputer programmieren

Einen Quantencomputer kann inzwischen jeder bequem von zu Hause aus programmieren. 
Aber was soll man programmieren? 
Und was ist überhaupt ein Quantenprogramm, ein Quantencomputer bzw Quantencomputing ?

Da mich das Thema interessiert, habe ich mir vorgenommen, diese und weitere Fragen im Rahmen eines Ausbildungsprogramms zum Quantencomputer-Entwickler zu vertiefen.
Die notwendigen Voraussetzungen für eine Zertifizierung sind hier: 

[IBM Certified Associate Developer - Quantum Computation using Qiskit v0.2X ](https://www.ibm.com/training/certification/C0010300)

Beschrieben.

Nach bestandener Prüfung erhält man dann das entsprechende Zertifikat:

[Credly: IBM Certified Associate Developer - Quantum Computation using Qiskit v0.2X ](https://www.credly.com/badges/65586d0d-ba5e-4698-ab0f-fd3448cdd6b2/public_url)



Das Lernen über Quantencomputing und die Themen Quantenschaltungen, Qubit-Gates, Bloch-Sphären und das Qiskit SDK sind Teil der zu lernenden Themen zum  Quantencomputer-Entwickler. 

Quantencomputing hat es das Potenzial, das Computing zu revolutionieren und ein wichtiges technologisches Werkzeug für Entwickler, Ingenieure und Personalverantwortliche in Unternehmen zu werden. Sowohl im Bereich der Hardware, also der Technologie, auf der Quantencomputer basieren als auch im Bereich der Software, mit dem diese neuen Computersysteme programmiert werden, hat sich ein weltweiter Wettkampf entfacht. 



Auf den folgenden Seiten werden die verschiedenen Themen vorgestellt, einschließlich der Definition, Ausführung und Visualisierung von Quantenschaltungen mit Qiskit. In Notebook Beispielen werden auch Einzel- und Multi-QuBit-Gatter und die damit verbundenen Rotationen auf der Bloch-Sphäre. In den Bespielen wird dann einfacher verständlich, wie man das Qiskit SDK verwendet, um Quantencomputeranwendungen zu implementieren und mit der Programmierung eigener Quantencomputerprogramme zu beginnen.

Hierzu gibt es dann sowohl die Möglichkeit, die Beispiele in der Cloud Umgebung von IBM 

[IBM Quantum Lab ](https://quantum-computing.ibm.com/lab)

oder in seiner eigenen lokal installierten Python+Quiskit Entwicklungsumgebung.


# Table of contents

1. [My Personal Experience Taking the Certification ExamIBM Quantum Composer](#personalexperience)
2. [Creating Logic Gates with IBM Quantum Composer](#composer)
3. [Measuring Outputs in the IBM Quantum Composer](#logic-gates)
[Predicting Output From Quantum Circuits]
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

![image logo](/images/quantum_composer.png)

Schauen wir uns kurz an, wie man mit Qubits (Quantencomputer-Bits) und der IBM Quantum Composer-Anwendung einige grundlegende logische Gatter erstellt.



## Erstellen von Logic Gates mit IBM Quantum Composer <a name="logic-gates"></a>

Quantencomputer-Bits werden Qubits genannt. Genau wie ein klassisches Bit in einem Computer können Qubits den Wert 0 oder 1 darstellen. In einem Quantencomputer können Qubits jedoch überlagert werden, so dass sie gleichzeitig die Werte 0 und 1 oder jeden beliebigen Wert dazwischen annehmen können.


Wenn wir die Qubits nicht in Superposition bringen, verhalten sie sich wie klassische Bits, was zu einer direkten Messung von entweder 0 oder 1 führt. Das macht es uns leicht, mit Qubits und dem IBM Quantum Composer zu experimentieren, indem wir logische Gatter zur Durchführung von Operationen in einem Quantenschaltkreis erstellen.


Zu den grundlegendsten Gattern gehören NOT, AND, OR, NAND und XOR. Im Folgenden schauen wir uns an, wie jedes dieser logischen Gatter im Quantencomputing mit Qubits und dem IBM Quantum Composer erstellt werden kann. Wir werden auch sehen, wie wir jedes dieser logischen Gatter sowohl auf dem IBM Quantum Composer als auch in Python mit Qiskit nachbilden können.


Wir beginnen mit den einfachsten Gattern, der Menge der Pauli-Gatter (X, Y, Z) (https://en.wikipedia.org/wiki/Quantum_logic_gate#Pauli_gates_%28X,Y,Z%29).

















