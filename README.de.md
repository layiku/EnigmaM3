[English](README.md) | [简体中文](README.zh-CN.md) | Deutsch

# Enigma M3 — Interaktiver 3D-Simulator

`enigma_m3.html` ist eine browserbasierte 3D-Simulation der deutschen Chiffriermaschine **Enigma M3** aus dem Zweiten Weltkrieg in einer einzigen Datei. Das Modell wird prozedural mit Three.js erzeugt (keine externen 3D-Modelle), mit GSAP animiert und von einer kryptographisch authentischen M3-Engine angetrieben.

## Funktionen

**Chiffrier-Engine**
- Walzen **I–VIII** mit originaler Verdrahtung und Kerben: I=Q, II=E, III=V, IV=J, V=Z, VI/VII/VIII=Z+M (Doppelkerben).
- Umkehrwalzen **UKW-B** und **UKW-C**.
- Ringstellung und Grundstellung je Walze; authentische **Doppelschritt**-Anomalie.
- Steckerbrett mit bis zu **13 Paaren**.
- `encryptChar()` liefert den vollständigen Signalweg (Steckerbrett → Walzen → Umkehrwalze → Walzen → Steckerbrett → Lampe).

**3D-Modell & Darstellung**
- Prozedurales Nussbaumgehäuse, schwarzer Schrumpflack-Deckel mit Fenstern, Messingteile, Pergament-Buchstabenringe.
- 26 Tasten, 26 Lampen mit mattierten Buchstabenfenstern, die beim Aufleuchten glühen, 3 Walzen + Umkehrwalze + Eintrittswalze; jede Walze mit 26 inneren Kupferdrähten, Kontakten und Schaltklinke.
- Walzen bestehen aus Buchstabenring (mit Kerbe) und Verdrahtungskern; eine Änderung der Ringstellung dreht den Kern relativ zum Ring.
- Innenleben: Chassis, Schaltklinken und Hebel, Lampenfassungen, Tastenkontakte, 4,5-V-Batteriekasten und Kabelbaum.
- **Explosionsansicht**, die Deckel, Gehäuse, Tastatur, Lampenfeld, Steckerbrett, Walzen und Innenleben trennt.
- **Signalweg-Leuchten**: Bei jedem Tastendruck leuchten die aktiven Walzen-/Umkehrwalzendrähte, die aktiven Steckerkabel und die Kabelbaumdrähte auf.

**Steckerbrett-Interaktion**
- 26 Messing-Doppelbuchsen und 13 Steckerkabel mit durchhängenden Bézier-Kurven.
- Leere Buchse anklicken, Stecker zu einer anderen Buchse ziehen; **magnetisches Einrasten** mit bernsteinfarbenem Führungsring und Klickgeräusch.
- Gesteckten Stecker ziehen = umstecken; einfacher Klick = Kabel ziehen.
- Schaltflächen: **13 Stecker zufällig** (nacheinander einfliegend), **Historisch (10)** (`AV BS CG DL FU HZ IN KM OW RX`, Standard), **Alle ziehen**.
- Kameravoreinstellung **Steckerbrett** (Drehung gesperrt) und **Übersicht**.

**Bedienung**
- Physische Tastatur A–Z oder Klick auf 3D-Tasten; Klick auf obere/untere Walzenhälfte schaltet vor/zurück.
- **Automatik**: eingefügter Text wird mit 350 ms pro Zeichen getippt (Umlaute im Textfeld werden zu AE/OE/UE/SS umgeschrieben).
- Zwei Papierstreifen (Eingabe/Ausgabe) in Fünfergruppen.
- **Walzen zurücksetzen**: vollständig auf I-II-III, Ringe AAA, Grundstellung AAA, UKW-B.
- Mit Web Audio synthetisierte Geräusche: Tastenanschlag, Walzenratsche, Stecker-Einrasten.

**Oberfläche**
- Sprachen **EN / 中文 / DE** (Standard EN, Auswahl wird gespeichert). EN und 中文 verwenden QWERTY; DE verwendet das authentische Enigma-Layout **QWERTZUIO / ASDFGHJK / PYXCVBNML**. Beim Wechsel *zu* DE werden die Streifen geleert.
- **FIX-Modus** (standardmäßig an, wird gespeichert): Das Modell behält bei jeder Fenstergröße die Bildschirmgröße einer 1920×1080-Darstellung, der Hintergrund füllt das ganze Fenster.
- Verschiebbares Walzenfenster mit Messing-**Steuerkreuz** (nach oben/unten/links/rechts schwenken, vorwärts/zurück auf der Bodenebene, Ansicht zurücksetzen; gedrückt halten für fortlaufende Bewegung).
- **Shift+V** kopiert die aktuelle Kameraansicht als JSON in die Zwischenablage.

## Starten

`enigma_m3.html` in einem modernen Desktop-Browser öffnen (Chrome, Edge, Firefox, Safari). Eine Internetverbindung ist nötig: Three.js r160 (über `unpkg.com`, ES-Modul-Importmap) und GSAP 3.12 (über `cdn.jsdelivr.net`) werden von CDNs geladen. Kein Build-Schritt und kein Server erforderlich.

## Bedienelemente

| Aktion | Bedienung |
|---|---|
| Buchstaben verschlüsseln | Tastatur A–Z oder Klick auf eine 3D-Taste |
| Walze weiterschalten | Klick auf obere (vor) / untere (zurück) Walzenhälfte oder Grundstellungs-Auswahl |
| Walzenlage / Ring / Umkehrwalze | Auswahllisten im Panel (eine bereits benutzte Walze wird getauscht) |
| Stecker verbinden | Leere Buchse anklicken, zu anderer Buchse ziehen |
| Umstecken / ziehen | Gesteckten Stecker ziehen / einfach anklicken |
| Kamera drehen / zoomen | Maus ziehen / Mausrad |
| Kamera schwenken | Steuerkreuz unter dem Walzenfenster |
| Kameraansicht kopieren | Shift+V |
| Walzenfenster verschieben | Rahmen ziehen; Doppelklick setzt Position zurück |

## Architektur

| Klasse | Aufgabe |
|---|---|
| `EnigmaEngine` | Reine Chiffrierlogik: Walzen, Kerben, Fortschaltung, Ringstellung, Umkehrwalze, Steckerbrett, Signalweg |
| `Enigma3DScene` | Prozedurales 3D-Modell, Materialien, Licht, Explosionsansicht, Leuchteffekte, Rendern bei Bedarf |
| `PlugboardInteraction` | 13 Steckerkabel, Ziehen, magnetisches Einrasten, Kabelkurven, Stecker-Animationen |
| `InteractionController` | Bedienfeld, Mehrsprachigkeit, Tastatur-/Zeigereingabe, Automatik, Streifen, Kameravoreinstellungen, Steuerkreuz |

## Prüfvektoren

- Walzen I-II-III, UKW-B, Ringe AAA, Grundstellung AAA, ohne Stecker: `AAAAA` → `BDZGO`.
- Doppelschritt: Grundstellung ADU → ADV, AEW, BFX.
- Umkehrbarkeit: Erneutes Verschlüsseln des Geheimtexts mit denselben Einstellungen ergibt den Klartext.

## Historische Hinweise

- Die Maschine hatte nur 26 Buchstaben: Funker schrieben Ä/Ö/Ü/ß als AE/OE/UE/SS, X diente als Punkt, Zahlen wurden meist ausgeschrieben; die Marine schrieb CH teils als Q.
- Walzen I–V wurden von Heer und Luftwaffe verwendet (3 aus 5); VI–VIII nur von der Kriegsmarine.
- Tastenkappen, Lampen und Steckerbrettschilder verwenden wie im Original schlichte serifenlose Großbuchstaben.

## Hinweise zur Leistung

- Statische Geometrie wird je Material zusammengefasst; Buchstabenringe und Schilder nutzen Textur-Atlanten; das Drahtleuchten nutzt temporäre Overlay-Meshes.
- Gerendert wird nur bei Änderungen (Kamera, Animationen, Eingabe), bei verborgenem Tab pausiert; Pixelverhältnis maximal 1,5; kleine Teile werfen keine Schatten.

## Lizenz

Noch keine Lizenz festgelegt — füge deine Lizenz hinzu.
