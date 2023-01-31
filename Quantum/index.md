# 

### [zurück zum Index](../index.md)

## Quantencomputing in Kürze

Quantencomputing stellt ein neues Paradigma in der Datenverarbeitung dar, bei dem die grundlegenden Prinzipien der Quantenmechanik
zur Durchführung von Berechnungen genutzt werden. 
Wenn Sie dies lesen, haben Sie zweifellos gehört, dass das Versprechen der Quanteninformatik in der Möglichkeit liegt, 
eine Handvoll Aufgaben wie Primfaktorzerlegung, Quantensimulation, Suche, Optimierung und algebraische Programme wie maschinelles 
Lernen effizient durchzuführen; 
Berechnungen, die in ihrer Größe selbst die Fähigkeiten der größten klassischen Computer übersteigen.



## Wo stehen wir heute ?

Wir wissen jetzt, dass Quantencomputer effizientere Algorithmen ausführen können, aber die Quantencomputer, die wir heute haben, 
sind zu klein und instabil, um einen Vorteil gegenüber herkömmlichen Computern zu bieten.

Auf einer sehr einfachen Ebene gibt es zwei Faktoren, die die Größe der Probleme begrenzen, die unsere Quantencomputer lösen können. 
Der erste ist die Datenmenge, die sie speichern und verarbeiten können, was wir normalerweise in Qubits messen
Ein "Qubit" ist ein "Quantenbit". 

Wenn wir nicht genügend Qubits haben, können wir Probleme ab einer bestimmten Größe einfach nicht mehr speichern und bearbeiten. 
Der zweite Punkt ist die Fehlerquote unseres Quantencomputers. 
Da wir das Verhalten von Quanten nur in aufwändigen Laborexperimenten beobachten können, ist die Entwicklung von Quantencomputern ein 
aufwändiger Prozess. 
Die Quantencomputer, die wir derzeit haben, sind verrauscht, was bedeutet, dass sie oft Fehler machen und "Rauschen" erzeugen.
Rauschen ist eine nutzlose Information, die nur schwer von nützlicher Information zu unterscheiden ist. 
Es ist zum Beispiel schwer, jemanden zu verstehen, der mit einem spricht, wenn in der Nähe viele andere Leute laut reden.

Im Moment sind die Quantencomputer, die wir haben, noch experimentell. 
Sie sind durch die Anzahl der Qubits und die Fehlerraten begrenzt, so dass die größten Probleme, die sie derzeit lösen können, 
für herkömmliche Computer noch leicht zu bewältigen sind.

Irgendwann in der Zukunft wird sich das ändern. 
Wir werden einen "Quantenvorteil" erreichen, d. h. es wird wirtschaftlich sinnvoller sein, ein Problem mit einem Quantencomputer 
zu lösen als mit einem herkömmlichen Computer. 


Schnelles Quiz

# Quantencomputer werden irgendwann...

	...die Geschwindigkeit von herkömmlichen Computern erhöhen.
	...herkömmliche Computer ersetzen.
	...Berechnungen durchführen, die für herkömmliche Computer zu schwierig sind.

Lösung: A


## Einen Quantencomputer programmieren

Einen Quantencomputer zu programmieren, kann jetzt jeder bequem von zu Hause aus tun. 
Aber was soll man programmieren? 
Was ist überhaupt ein Quantenprogramm? 
Und was ist eigentlich ein Quantencomputer?

Diese Fragen lassen sich durch Vergleiche mit herkömmlichen digitalen Computern beantworten. 
Leider wissen die meisten Menschen auch nicht, wie herkömmliche Digitalcomputer funktionieren. 
Auf dieser Seite werden wir uns mit den grundlegenden Prinzipien dieser traditionellen Geräte befassen. 
Um uns später den Übergang zum Quantencomputer zu erleichtern, werden wir die gleichen Werkzeuge verwenden, 
die wir auch für Quantencomputer einsetzen.


## Quanten Schaltkreise bzw Diagramme

Eine Berechnung benötigt einige Eingabedaten und Operationen, die darauf ausführt werden, um  Ausgabedaten zu erzeugen. 
Bei den Quantencomputern, liegen diese Daten immer in Form von Bits vor. 

Es ist oft nützlich, diesen Prozess in einem Diagramm darzustellen, das als Schaltplan bekannt ist. 
Diese Diagramme haben Eingänge auf der linken Seite, Ausgänge auf der rechten Seite und dazwischen Operationen, 
die durch Gatter-Symbole dargestellt werden. 
Diese Operationen werden "Gatter" genannt, meist aus historischen Gründen. 
Hier ist ein Beispiel dafür, wie ein Schaltkreis für Standard-Bitcomputer aussieht. 

![image logo](/images/logic_gates.png)

Es wird nicht von Ihnen erwartet, dass Sie verstehen, was sie tut. 
Es soll Ihnen lediglich eine Vorstellung davon vermitteln, wie diese Schaltungen aussehen.

Bei Quantencomputern verwenden wir dieselbe Grundidee, haben aber andere Konventionen für die Darstellung von 
Eingängen, Ausgängen und den für Operationen verwendeten Symbolen. 
Hier ist die "Quantenschaltung", die den gleichen Prozess wie oben darstellt.

![image logo](/images/quantum_circuit.png



### Quantum Computer programmieren mit Quiskit

Das Hauptziel von Qiskit ist es, einen Software-Stack zu entwickeln, der es jedem leicht macht, Quantencomputer zu benutzen, 
unabhängig von seinen Fähigkeiten oder seinem Interessengebiet; 
Qiskit ermöglicht es, Experimente und Anwendungen einfach zu entwerfen und sie auf echten Quantencomputern und/oder 
klassischen Simulatoren auszuführen. 
Qiskit wird bereits auf der ganzen Welt von Anfängern, Bastlern, Lehrern, Forschern und kommerziellen Unternehmen genutzt.

### Schaltkreise mit Qiskit erstellen

#### Basics

Wir wissen, dass wir alle Informationen mit einem Bündel von Bits beschreiben können. 
So speichern und verarbeiten Computer alles, auch Quantenschaltungen! 
Aber für uns Menschen ist es schwierig, darüber nachzudenken, wie wir dies tun und wie wir diese Bits manipulieren, 
um die gewünschten Schaltungen darzustellen.

Ein  "QuantumCircuit" ist ein Satz von Anweisungen zur Darstellung von Quantenschaltungen als Bits. 
Die Zeile 

	qc = QuantumCircuit(4, 2) 

ist ein Konstruktor, der Python anweist, einige Bits in Ihrem Computer bereitzustellen, die wir zur Darstellung eines 
Quantenschaltkreises verwenden werden. 
Wenn wir uns auf diesen Quantenschaltkreis beziehen wollen (oder besser gesagt, auf die Bits, die diesen 
Quantenschaltkreis darstellen), verwenden wir die Variable "qc". 

Wir sagen, dass sich 'qc' auf ein "QuantumCircuit-Objekt" bezieht.


Dies ermöglicht es uns Menschen, über Quantenschaltungen auf einer hohen, abstrakten Ebene nachzudenken; 
wir können Anweisungen  wie 
	"füge ein X-Gate hinzu" 
und Qiskit kümmert sich darum, was wir mit den Bits in unserem Computer machen müssen, um diese Änderung zu reflektieren.

