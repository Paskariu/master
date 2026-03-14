#Klausurrelevant 
# Kausale Ordnung

> Sei ⊰ eine strikte totale Ordnung von Ergebnissen einer Menge E.
> Falls es eine Happens-before Relation gibt, so dass stets aus (a $\rightarrow$ b) => (a ⊰ b) folgt, nennt man ⊰ auch eine kausale Ordnung der Ergebnismenge E

Daraus folgt, dass Happens-before keine zwingende Kausalität impliziert, aber Kausalität erfordert Happens-before in informationstechnischen Systemen.

## Happens-before Relation
Ergebnis $a$ findet vor Ergebnis $b$ statt, wenn:
+ $a$ und $b$ auf dem gleichen Knoten stattfinden und $a$ bzgl. der physikalischen Ausführungsreihenfolge vor $b$ ausgeführt wurde
+ $a$ das Senden und $b$ das Empfangen derselben Nachricht m ist
+ es ein Ergebnis $c$ gibt, sodass $a \rightarrow b$ und $c \rightarrow b$ gilt ([[Transitivität]])
Immer nur eine partielle Ordnung:
+ Wenn $a$ und $b$ nebenläufig sind: $a || b$
Weiteres:
+ $a$ muss nicht zwingend zeitlich früher sein, sondern es **könnte** ein kausaler Zusammenhang im lokalen Bereich bestehen
+ Es ist eine strikte partielle Ordnung, weswegen Abbildungen von potentiellen Kausalität in Verteilten Systemen möglich ist

### Strikte partielle Ordnung
> Relation $R$ ist strikt partiell geordnet, wenn sie irreflexiv und transitiv ist
+ Ergebnise sind entweder $a \rightarrow b$ oder $b \rightarrow a$ oder $a || b$
# Logische Uhren
> Logische Uhren($T$) zählen Ereignisse anstatt von Zeit im Format $(a \rightarrow b) => (T(a) < (b))$

+ Physikalische Uhren zählen Sekunden
+ Logische Uhren zählen Ereignisse

## Lamport-Uhren
> Eine Uhr, die anstelle der Zeit, Ereignisse zählt
### Algorithmus
+ Jeder Knoten hat seinen eigenen Zähler $t$, der bei jedem lokalen Ereignis um 1 erhöht wird
+ Beim Senden einer Nachricht wird dieser zusammen mit der Nachricht $(t,m)$ verschickt
+ Beim Empfangen von $(t', m)$ wird das Maximum zwischen eigenen Zähler $t$ und $t'$ bestimmt und um 1 inkrementiert. Dies ergibt den neuen Wert des lokalen Zählers

Wenn $L(x)$ der durch diesen Algorithmus bestimmten Lamport-Uhrwert für das Ergebnis x ist, dann gelten die folgenden Aussagen über eine Lamport-Uhr

+ Falls $(a \rightarrow b) => (L(a) < L(b))$
+ Aber, aus $(L(a)) < L(b)$ folgt nicht $(a \rightarrow b)$
+ Außerdem ist es möglich, dass $L(a) = L(b)$ für $a \neq b$

## Beispiel

![[lamport_uhr.jpg]]
+ Sei $N(e)$ der Name des Knotens, auf dem Ereignis e stattgefunden hat. Das Paar $(L(e), N(e))$ bestimmt dann eindeutig das Ergebnis e.
+ Man kann dadurch die totale Ordnung ⊰ auf der Ereignismenge unter der Nutzung der Lamport-Zeitstempel der lexikographischen Ordnung der Knotennamen wie folgt definieren.#
$$
(a ⊰ b) \Leftrightarrow (L(a) < L(b) \lor (L(a) = L(b) \land N(a) < N(b)))
$$
+ Die Resultierende Ordnung ist dann kausal
+ Nebenläufigkeit wird willkürlich basierend auf den Knoten eingereiht
## Vektoruhren
> Selbe Logik wie Lamport-Uhr, nur mit dediziertem Counter pro Knoten
+ Warum?
	+  Mit Lamport-Zeitstempeln können wir nicht zwischen $(a \rightarrow b)$ oder $(a || b)$ unterscheiden

## Ordnung
Mit Vektor-Uhren können folgende Ordnungen $V$ definiert werden
+ $T = T'$ falls $T[i] = T'[i]$ für alle $i \in \{1, ... n\}$
+ $T \leq T'$ falls $T[i] \leq T'[i]$ für  alle $i \in \{1, ... n\}$
+ $T < T'$ falls  $T \leq T'$ und  $T \neq T'$
+ $T||T$ falls  $T \nleq T'$ und  $T \neq T'$
$$
V(a) \leq V(b) \text{ bei } (\{a\} \cup \{e| e \rightarrow a\} \subseteq \{b\} \cup \{e| e \rightarrow b\})
$$
Die resultierende Ordnung $V$ hat somit dieselben Eigenschaften wie Happens-before und ist genau eine Implementierung dieser
## Beispiel
![[vektor_uhr.jpg]]
Vektor-Zeitstempel eines Ergebnisses $e$ beschreibt eine Menge von Ereignissen: zum einen $e$ selbst, sowie seine kausalen Abhängigkeiten $\{e\} \cup \{a | a \rightarrow e\}$
So ist $[2,2,0]$ z.B. oben die ersten zwei Ereignisse von A sowie die ersten 2 von B und keine von C.