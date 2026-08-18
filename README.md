# Vorhersage

**Wie weit ein Rechner in die Zukunft sieht.** Zwei Doppelpendel, dieselben Gleichungen, derselbe
Rechner — Unterschied im Startwinkel: **ein einziges Bit**. Nach zweiundzwanzig Sekunden haben sie
nichts mehr miteinander zu tun.

→ **[Blatt öffnen](https://ssims437.github.io/vorhersage/)**

- **Zwei Doppelpendel nebeneinander**, im Startwinkel um 1 ULP bis 10⁻³ auseinander — mit Spuren,
  solange sie sich noch decken, sieht man nur eines
- **Abstandskurve** logarithmisch: eine Gerade heißt exponentielles Wachstum, die Steigung ist der
  Lyapunov-Exponent
- **λ messen** am Lorenz-System nach Benettin, mit Fortschrittsbalken — Literaturwert zum Vergleich
- **Der Horizont** — aus dem gemessenen λ die brauchbare Vorhersagedauer je Startgenauigkeit
- **Verfahren gegen Energie** — Euler, symplektischer Schritt und Runge-Kutta 4 über zwei Millionen
  Schritte
- **Prüflauf** — sieben Zeilen, die zwei scheinbar widersprüchliche Dinge nachweisen

## Deterministisch **und** unvorhersagbar

Das ist der Punkt des Blattes, und beides steht als Prüfzeile nebeneinander:

| | |
|---|---|
| **Determinismus** | zwei Läufe über 60 000 Schritte, alle vier Zahlen **bis aufs letzte Bit** identisch |
| **Unvorhersagbarkeit** | ein Bit Startunterschied (4,44·10⁻¹⁶) → nach **22,2 s** ist der Abstand auf 1 gewachsen |

Kein Zufall im Spiel, keine Messfehler, keine Physik — nur die Tatsache, dass der Startwert nie
exakt bekannt ist und der Fehler wie `e^(λt)` wächst.

## Was der Prüflauf zeigt

| Behauptung | Ergebnis |
|---|---|
| gleiche Eingabe, bitgleiche Ausgabe | 60 000 Schritte, alle vier Zahlen identisch |
| **ein Bit genügt** | Startunterschied 4,44·10⁻¹⁶ → Abstand 1 nach **22,2 s** |
| Lyapunov trifft die Literatur | Lorenz gemessen **λ = 0,900…0,903** gegen **0,9056** · Verdopplungszeit 0,77 |
| **die Grenze lässt sich vorhersagen** | gemessen gegen `ln(1/ε)/λ`: 30,2 s ↔ 30,7 · 24,2 ↔ 25,6 · 20,3 ↔ 20,5 · 12,7 ↔ 15,3 |
| Schrittweite halbieren, Fehler durch 16 | 2,08·10⁻⁶ → 1,36·10⁻⁷ → 8,67·10⁻⁹, Verhältnis **15,3 und 15,7** |
| symplektisch bleibt beschränkt, RK4 driftet | 2 Mio Schritte: Euler **8,9·10³** · symplektisch schwankt um **6,75·10⁻²** · RK4 wächst **4,3·10⁻⁵ → 8,5·10⁻⁵** |
| andere Klammern, andere Zukunft | `(a+b)+c ≠ a+(b+c)` in **25,6 %** der Fälle · dieselbe Bahn, nur umgestellte Summe, läuft nach **22,3 s** auseinander |

Die vierte Zeile ist die schönste: Das Blatt sagt **vorher, wo seine eigene Vorhersage endet** —
und die Messung bestätigt es auf ein bis zwei Sekunden genau.

## Warum bessere Messgeräte kaum helfen

Aus dem gemessenen λ folgt der Horizont direkt. Die Tabelle im Blatt rechnet ihn aus:

| Startgenauigkeit | brauchbar bis |
|---|---|
| Millimeter (10⁻³) | 7,6 Zeiteinheiten |
| Mikrometer (10⁻⁶) | 15,3 |
| Nanometer (10⁻⁹) | 22,9 |
| ein Bit im Rechner (2·10⁻¹⁶) | 39,9 |

**Tausendfach genauer messen verlängert die Vorhersage um 7,6 Zeiteinheiten** — nicht um das
Tausendfache. Jede weitere Stunde Vorhersage kostet eine Größenordnung Genauigkeit. Das ist der
Grund, warum Wettervorhersage bei etwa zwei Wochen endet und nicht daran, dass die Rechner zu klein
wären.

## Was mich das gekostet hat

**Meine eigene Messung war geschönt — durch den Zufallsgenerator.** Die Zeile „Fließkomma-Addition
ist nicht assoziativ" meldete im Blatt **0,1 %**, in meiner Vorprobe zuvor **25,6 %**. Kein Fehler
in der Prüfung, sondern in den Zahlen, mit denen sie füttert: mein Generator im Blatt teilte durch
2³² und lieferte damit Werte, deren Mantisse **unten mit Nullen aufgefüllt** ist — solche
Additionen sind oft exakt. Der Generator der Vorprobe teilte durch 2³¹−1 und besetzte die Mantisse
voll. Die Prüfzeile nennt jetzt **beide** Zahlen und den Grund, denn genau das ist die Lehre: ob
eine Rundung zuschlägt, hängt daran, wie viele Stellen die Zahlen wirklich belegen.

**Ohne Renormierung misst man nichts.** Der erste Ansatz für λ war der naive: zwei Bahnen starten
lassen und schauen, wie der Abstand wächst. Das funktioniert genau so lange, bis der Abstand die
Größe des Attraktors erreicht — danach wächst er nicht mehr, die Kurve knickt ab, und wer über den
ganzen Verlauf mittelt, bekommt eine viel zu kleine Zahl. Richtig ist das Verfahren von Benettin:
den Abstand messen, die zweite Bahn wieder auf den Ausgangsabstand **heranholen**, weitermachen,
und die Logarithmen mitteln. Erst damit steht 0,900 statt einer beliebigen kleineren Zahl.

**Der genaueste Integrator ist nicht der beste.** Über zwei Millionen Schritte:

| Verfahren | Energiefehler bei halber Strecke | am Ende |
|---|---|---|
| Euler | wächst ungebremst | **8,9·10³** |
| Runge-Kutta 4 | 4,3·10⁻⁵ | **8,5·10⁻⁵** — genau das Doppelte |
| symplektischer Schritt | schwankt | **≤ 6,75·10⁻²**, kein Trend |

RK4 ist tausendfach genauer und trotzdem der Verlierer, wenn man lange genug rechnet: sein Fehler
wächst **linear weiter**, während der symplektische schwankt und bleibt, wo er ist. Für eine
Planetenbahn über Jahrmillionen nimmt man deshalb das Verfahren, das pro Schritt schlechter ist.

**Ein Bit ist keine Metapher.** Die Störung „1 ULP" wird nicht als `1e-16` genähert, sondern über
die Bitdarstellung erzeugt: Mantisse um eins erhöhen, fertig. Beim Startwinkel 2,4 sind das
4,44·10⁻¹⁶ — der kleinstmögliche Unterschied, den dieser Rechner zwischen zwei Zahlen überhaupt
kennt. Alles Kleinere ist dieselbe Zahl.

**Was das Blatt nicht kann:** keine Intervallarithmetik (die den Fehler mitführen statt schätzen
würde), keine Ensemble-Vorhersage wie in der Meteorologie, kein adaptiver Schritt, keine
Fehlerabschätzung pro Schritt. Der Lyapunov-Exponent wird nur für das Lorenz-System gemessen, weil
dort ein Literaturwert existiert — beim Doppelpendel hängt er von der Energie ab und wäre ohne
Vergleichswert nicht überprüfbar.

## Technik

Eine einzelne HTML-Datei. Kein Build, keine Bibliothek, nichts verlässt den Browser.
Runge-Kutta 4, symplektischer Euler, Benettin-Verfahren, Canvas 2D, hell und dunkel.

## Die ganze Sammlung

Alle Blätter nach Feld geordnet, jedes mit eigenem Repo:
**[ssims437.github.io](https://ssims437.github.io/)**

## Lizenz

MIT
