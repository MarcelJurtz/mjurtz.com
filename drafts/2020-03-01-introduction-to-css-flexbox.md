---
title: 'Introduction to CSS Flexbox'
date: 2019-03-01T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2020/03/introduction-to-flexbox
categories:
  - blog
---

Flexbox is short for _Flexible Box Layout Module Level 1_ which describes a collection of css-properties that can be used to align content and divide space between containers.

You can find the full specification [here](https://www.w3.org/TR/css-flexbox-1/).

Flexbox can be used for several usecases which include the following:

* Equal Heights
* Vertical centering
* Media objects
* Sticky footers
* Column layouts

To achieve these, flexbox introduces three new components to css:

* Flex containers
* Flex items
* Flex lines

Flexbox also adds the two following new values for the _display_-attribute:

* flex
* inline-flex

The core of the flexbox-feature are the flex-items. These are wrapped within a flex-containers and are responsible for the spacing. Every direct child element of a flex-container is a flex-child. Grandchildren of a flex-container however are not considered flex-items. Furthermore, their size is not relevant for the algorithm that calculates the size of the flex-items.

Everything you need to do to mark an HTML-element as flex-container is to set its display-property to _flex_.

Let's get into detail of all the flexbox-features next.

## Flexbox Wrapping & Directions

You can use flexbox wrapping to vertically align items over multiple lines. To specify the wrapping behaviour, use the _flex-wrap_-property. It defaults to _nowrap_ and also supports the values _wrap_ and _wrap-reverse_. If you want to allow the container to put elements in the next line, use _wrap_. _Wrap-reverse_ basically does the same, but breaks in reverse order (e.g. above the current line when using horizontal alignment).

The stacking only takes place when there is not enough space left in the current line. If you also want to apply the wrapping, regardless of the items size, you can use the _flex-direction_ property.

This property defaults to _row_, but also supports the values _row-reverse_, _column_ and _column-reverse_.
_Flex-direction_ specifies the object-flow of the flexbox-container. The default is row-layout, which means that elements will align horizontally from left to right. Using this property, you can for example easily create a 100%-height-layout with a sticky footer:

HTML:
```html
<body>
    <header>...</header>
    <main>...</main>
    <footer>...</footer>
</body>
```

CSS
```css
body {
    display: flex;
    flex-direction: column;
}

html, body, main {
    height: 100%;
}
```

------------------ CONTINUE

## Distributing Space Along Flex Lines

Flex-Items werden entlang von Achsen angeordnet. Die Haupt-Achse wird durch _flex-direction_ definiert.

Bsp: flex-direction: row -> Start der Achse ist am linken Ende des Containers, Ende der Achse am rechten Ende des Containers.
Achsen verfügen außerdem über einen Center-Point.

justify-content

Mit justify-content wird definiert, wie Platz auf der Haupt-Achse verteilt wird. 

-> Default: flex-start
-> Optionen: flex-start | flex-end | center | space-between | space-around

Mit flex-start, center und flex-end werden die Elemente innerhalb des Containers linksbündig, zentriert bzw. rechtsbündig angeordnet (bei horizontaler, ltr Haupt-Achse).
Space-Between sorgt dafür, dass jeweils ein Element bei Beginn und Ende der Haupt-Achse positioniert wird, und der Raum dazwischen mit den restlichen Elementen aufgefüllt wird.
Space-Around funktioniert ähnlich, sorgt aber dafür, dass ALLE Flex-Items (Auch Start und Ende) jeweils den gleichen Abstand links und rechts haben.

## Reihenfolge

Mit dem _order_-Property kann die Reihenfolge von Flex-Items entlang der Hauptachse bestimmt werden.

order
-> Default: 0
-> Optionen: Positive & negative Zahlen

Alle Werte identisch -> Ursprungsreihenfolge (HTML). Kleiner Zahl -> Beginn der Achse. Größere Zahl -> Ende der Achse.

## Distributing Space on the Cross Axis

Neben der Main-Axis existiert eine weitere Achse, die sog. Cross Axis. Diese verläuft senkrecht zur Main Axis.
Bei einer Row-Direction ist die Main Axis also Horizontal, die Cross Axis vertikal.
Als Pendant zu Justify-Content zur Positionierung auf der Main Axis existiert für die Cross Axis das Property align-items.

align-items
-> Default: stretch
-> Optionen: stretch | flex-start | flex-end | center | baseline

* stretch: Füllt verfügbaren Platz
* flex-start: Start der cross-axis
* flex-end: Ende der cross-axis
* center: zentriert
* baseline: items werden mit matchender baseline positioniert

## Größenanpassung

Mit flex-grow kann angegeben werden, in welchem Maß ein flex-item relativ zu einem anderen wachsen kann.

flex-grow

-> Default: 0 (Don't grow)
-> Optionen: Numerische Werte

1 -> So viel Platz wie möglich (= Gesamter Platz - Notwendiger Platz für andere Flex-Items)
1 Auf mehreren -> Aufteilung zwischen diesen
Zwei auf 1, Eins auf 2 -> Doppelte Skalierung für Element mit 2.

```
------ ----- ----- ------
|  1  |     2     |  1  |
------ ----- ----- ------
```

Achtung: Bei leeren Containern funktioniert die Aufteilung wie erwartet. Obiges Beispiel bei einer Gesamtbreite von 1000px würde also einzelne Containerbreiten von 250px, 500px und 250px ergeben.

Wenn Container 1 und 3, die beide mit flex-grow 1 markiert sind, unterschiedliche Inhalte besitzen, resultiert das in unterschiedlichen Gesamtbreiten dieser.

-> Sonderlösung für diesen Fall folgt!


Problem: Bei Verkleinerung soll zwar Text neu skaliert werden, Bilder aber nicht.
Der Schrumpf-Faktor eines Flex-Items kann mit flex-shrink definiert werden.

flex-shrink

-> Default: 1 (Schrumpfen erlaubt)
-> Optionen: Numerische Werte

0 -> Schrumpfen verhindern


Problem: Setzen einheitlicher Größen nicht möglich. 2-Spalten-Layout mit Bildern zB nicht konsistent, Breiten zB unterschiedlich.

flex-basis
-> Default: auto
-> Optionen: auto | Größenangaben in %, px, em, rem, ... (Standard-CSS-Units)

flex-basis kann gut mit flex-shrink kombiniert werden, da die Größe als Mindestgröße angesehen wird und das Item mit ausgewiesener flex-basis nicht verkleinert wird.

Elemente mit flex-basis in absoluter Einheit (zB px) wachsen standardmäßig nicht über den angegebenen Wert hinaus. Relative Größenangabe hingegen ermöglicht hinauswachsen.

## Property Plotting

_align-self_ wird verwendet, um einzelne flex items durch überschreiben des align-items-Werts zu positionieren.

align-self
-> Default: strech
-> Optionen: stretch | flex-start | flex-end |center | baseline

Problem: align-items ist nicht für wrapped flex-items verwendbar.

_align-content_ wird dafür genutzt.

align-content
-> Default: strech
-> Optionen: stretch | flex-start | flex-end |center | baseline

## Shorthands

-> Achtung, teilweise inkonsistente Implementierung (IE)

Einzelne Zahl: grow

```css
.flex-item {
    flex-grow: 1;
    flex-shrink: 1; /* Default */
    flex-basis: 0;
}

/* Entspricht */
.flex {
    flex: 1;
}
```

Zwei Zahlen: grow, shrink

```css
.flex-item {
    flex-grow: 1;
    flex-shrink: 0;
    flex-basis: 0;
}

/* Entspricht */
.flex {
    flex: 1 0;
}
```

Einzelne Zahl + Prozensatz oder Maßangabe (px etc): grow + basis
Gleiches gilt für 3: grow - shrink - basis.

```css
.flex-item {
    flex-grow: 1;
    flex-shrink: 1; /* Default */
    flex-basis: 47.5%;
}

/* Entspricht */
.flex {
    flex: 1 47.5%;
}
```

None: Setzt shrink-Wert, was flex-behaviour entfernt

```css
.flex-item {
    flex-grow: 0; /* Default */
    flex-shrink: 0;
    flex-basis: auto; /* Default */
}

/* Entspricht */
.flex {
    flex: none;
}
```

flex-flow: Setzt Werte für flex-direction und flex-wrap. Default: row nowrap.

```css
.flex-item {
    flex-direction: row; /* Default */
    flex-wrap: nowrap; /* Default */
}

/* Entspricht */
.flex {
    flex-flow: row nowrap;
}
```

Parameter für beide können auch einzeln angegeben werden. Nur direction value -> default nowrap.
Nur wrap-value: Default row.

## Zusammenfassung

* Methode zur Anordnung von Elementen
* Alternative zu float
* Neue Display-Werte: flex, inline-flex
    * Unterscheiden sich nicht inhaltlich, sondern beziehen sich auf den Container
    * Flex: Container ist block-Element, Inline-Flex: Container ist inline-Element
    * Muss auf Container gesetzt werden, um Flexbox-Basisfunktionalität zu aktivieren

Anwendungsfälle:
* Equal Heights
* Vertikale Zentrierung
* Media Objects
* Sticky Footers
* Column Layouts

Neue Komponenten:
* Flex Containers
* Flex Items
* Flex Lines

Nur **direkte** Kind-Elemente eines Flex-Containers sind Flex-Items. Grandchild-Elementen können durch Wrapping hierarchisiert werden.

flex-wrap
* Erzwingt Halten auf einer Flex-Line (nowrap) oder erlaubt Umbruch bei Bedarf (wrap) bzw. kehrt die Reihenfolge (Verlauf der Flexline) hier um
* Default: nowrap
* Optionen: nowrap | wrap | wrap-reverse

flex-direction
* Orientierung des Flexbox-Containers
* Default: row
* Optionen row | row-reverse | column | column-reverse

justify-content
* Definition der Platzverteilung auf der Main-Axis
* Default: flex-start
* Optionen: flex-start | flex-end | center | space-between | space-around

order
* Anpassung der Reihenfolge von Flex-Items entlang der Main-Axis
* Default: 0
* Optionen: Numerische Werte (negativ -> Beginn der Achse, positiv -> Ende der Achse)

align-items
* Pendant zu justify-content auf der Cross-Axis
* Default: stretch
* Optionen: stretch | flex-start | flex-end | center | baseline

flex-grow
* Relative Angabe zur Wachstumsfähigkeit eines Flex-Items entlang der Main-Axis
* Default: 0 (Nicht wachsen)
* Optionen: Numerische Werte

flex-shrink
* Analog zu flex-grow
* Default: 1 (schrumpfen)
* Optionen: Numerische Werte

flex-basis
* Ausgangsgröße von Flex-Items, Absolute Angabe -> Kein darüber Hinauswachsen
* Default: auto
* Optionen: auto | Größenangaben in %, px, em, rem, ... (Standard-CSS-Units)