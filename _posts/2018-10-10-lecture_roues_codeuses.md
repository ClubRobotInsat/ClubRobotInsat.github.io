---
layout: post
published: true
title: "Lecture des roues codeuses"
mathjax: false
featured: true
comments: false
type: video
description: "Algorithme de lecture des roues codeuses"
headline: "Roues codeuses"
modified:
tags: [electronique]
categories: [Électronique]
share: true
author: bigey
---

# Lecture des roues codeuses

Le problème ici est de trouver un moyen de connaître l'état d'une roue codeuse capable de compter sur 16 bits.

À chaque rotation complète de la roue, ce capteur émets une interruption d'overflow, mais le but de l'algorithme présenté ici est de connaître en permanence la position de la roue codeuse *(5 tours et 10 crans atteints depuis le début, par exemple)* sans prendre en compte ces overflows.

## Données initiales

* Le capteur nous fournit une position codée sur un `u16`
* la roue codeuse est un compteur allant de `min = 0` à `max = pow(2, 16) - 1 = 65535`

---

## Résolution

On se donne les libertés suivantes :

* on a le droit d'avoir une variable `counter: i64` globale à l'algo
* on peut utiliser autant de variables que l'on veut dans le programme
* on peut imposer des contraintes physiques sur la vitesse de rotation de la roue codeuse

### Contraintes

> La roue tourne strictement moins d'un demi-tour entre deux prises d'échantillon

Cette contrainte impose une vitesse de rotation `v` maximale, mais elle permets que le sens de rotation de la roue codeuse soit décisable entre deux échantillons consécutifs.

### Variables utilisées

La position globale de la roue codeuse est décidée pour chaque couple d'échantillons consécutifs, donc on utilise les variables suivantes :

* ***variables***
	* `counter: i64` - Compteur global correspondant à l'état de la roue codeuse. Utiliser 64 bits permets un déplacement sur plus de 2000km dans la même direction sans overflow pour une roue codeuse de diamètre 6cm.
	* `current: u16` - Position de la roue codeuse lue dans l'itération en cours `N`
	* `previous: u16` - Position de la codeuse lue à l'itération `N - 1`
* ***constantes***
	* `min: u16 = 0x0` - Valeur minimale fournie par la roue codeuse
	* `max: u16 = pow(2, 16) - 1` - Valeur maximale fournie par la codeuse
	* `half: u16 = pow(2, 15)` - Valeur moyenne fournie par la codeuse et utilisée pour décider du sens de rotation

### Traitement des cas possibles

Le code doit être capable de gérer les overflows et les underflows pour mettre à jour le `counter`.

Il doit prendre en compte 4 cas différents : (2 sens de rotation différents) * (overflow ou non).

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;margin-left: auto;
margin-right: auto;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-7btt{font-weight:bold;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-c3ow"></th>
    <th class="tg-7btt">Rotation trigonométrique</th>
    <th class="tg-7btt">Rotation horaire</th>
  </tr>
  <tr>
    <td class="tg-7btt">over/under flow</td>
    <td class="tg-0pky"><span style="font-style:italic">représentation :</span> min--C---------P--max<br><span style="font-style:italic">décision :</span> (C &lt; P) &amp;&amp; (P - C &gt; half)<br><span style="font-style:italic">impact :</span> counter += (max - P + C + 1)</td>
    <td class="tg-0pky"><span style="font-style:italic">représentation : </span>min--P---------C--max<br><span style="font-style:italic">décision : </span>(P &lt; C) &amp;&amp; (C - P &gt; half)<br><span style="font-style:italic">impact : </span>counter -= (max - C + P + 1)</td>
  </tr>
  <tr>
    <td class="tg-7btt">comptage normal</td>
    <td class="tg-0pky"><span style="font-style:italic">représentation : </span>min------C--P------max<br><span style="font-style:italic">décision : </span>(P &lt; C) &amp;&amp; (C - P &lt; half)<br><span style="font-style:italic">impact : </span>counter += C - P</td>
    <td class="tg-0pky"><span style="font-style:italic">représentation :</span> min------P--C------max<br><span style="font-style:italic">décision : </span>(C &lt; P) &amp;&amp; (P - C &lt; half)<br><span style="font-style:italic">impact : </span>counter -= P - C</td>
  </tr>
</table>

---

## Implémentation possible

Finalement, voici une implémentation en considérant que la lecture se fait depuis `read_captor()` :

<script src="https://gist.github.com/Terae/29b0d55fcd01ac1ea1a79900562421ea.js"></script>
