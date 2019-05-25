---
title: Erste Schritte
layout: Standard
root: ../.....
idx: 1.1
description: Erfahren Sie, wie Sie einen Mod-Ordner erstellen, einschließlich init.lua, mod.conf und mehr.
redirect_from:
- /de/chapters/folders.html
- /de/basics/folders.html
---

## Einführung

Es ist sehr wichtig, die Struktur von einem Mod zu verstehen, damit man eigene Mods erstellen kann.

Inhaltsverzeichnis:

* [Was sind Spiele und Mods?](#Was-sind-Spiele-und-Mods)
* [Wo werden Mods gespeichert?](#Wo-werden-Mods-gespeichert)
* [Mod Directory](#Mod-Directory)
* [Abhängigkeiten](#Abhängigkeiten)
* [Mod-Pakete](#Mod-Pakete)
* [Beispiel](#Beispiel)


## Was sind Spiele und Mods?

Die Stärke von Minetest ist die Fähigkeit, Spiele einfach und ohne die Notwendigkeit 
 ihrer eigenen Voxel-Grafiken, Voxel-Algorithmen oder kompliziertem Netzwerkcode zu erstellen.

In Minetest ist ein Spiel eine Sammlung von Mods, die zusammenwirken, um die um den Inhalt und das Verhalten eines Spiels zu steuern.
Ein Modul, allgemein bekannt als Mod, ist eine Sammlung von Skripten und Ressourcen.
Es ist möglich, ein Spiel mit nur einer Mod zu machen, aber das wird selten getan, weil es sonst schwerer ist, Teile des Spiels unabhängig von anderen anzupassen und zu ersetzen.

Es ist auch möglich, Mods außerhalb eines Spiels zu benutzen, in diesem Fall sind sie auch außerhalb eines Spiels aktivierbar.
In diesem Falle sind *Mods* im traditionellen Sinne - Modifikationen. Diese Mods
 erweitern Funktionen eines Spiels.

Sowohl die in einem Spiel enthaltenen Mods als auch die Mods von Drittanbietern verwenden die gleiche API.

Dieses Buch behandelt die wichtigsten Teile der Minetest API,
und ist sowohl für Spieleentwickler als auch für Modder geeignet.


## Wo werden Mods gespeichert?

<a name="mod-locations"></a>

Jeder Mod hat sein eigenes Verzeichnis, in dem sich sein Lua-Code, seine Texturen, Modelle und seine
Sounds befinden. Minetest prüft an verschiedenen Stellen um Mods zu finden.  
Diese Orte werden allgemein als *mod load paths* (zu Deutsch "Pfade zum Mods Laden") bezeichnet.

Für ein bestimmtes Welt-/Speicherspiel werden drei Mod-Speicherorte überprüft.
Diese sind in, folgender Reihenfolge:

1. Spiel Mods. Dies sind die Mods, die das Spiel bilden und von der Welt geladen werden.
   Z.B.: `minetest/games/minetest_game/mods/`, `/usr/share/minetest/games/minetest/`
2. Globale Mods, der Ort/Pfad, an dem Mods fast immer installiert sind.
   Im Zweifelsfall werden sie hier platziert.
   Z.B.: `minetest/mods/`minetest/mods/`.
3. Welt Mods, der Ort, an dem Mods gespeichert werden können, die speziell für eine
   bestimmte Welt sind.
   Z.B.: `minetest/worlds/world/worldmods/`.

Minetest überprüft die Orte/Pfade in der oben angegebenen Reihenfolge. Wenn es auf einen Mod trifft,
der den gleichen Namen wie zuvor hat, wird der spätere Mod an Ort und Stelle geladen, anstatt
des früheren Mod.
Das bedeutet, dass du Spielmods übersteuern kannst, indem du einen Mod mit dem gleichen Namen im Globalen Mod Ordner platzierst.  

Der tatsächliche Standort der einzelnen Mod-Load-Pfade hängt davon ab, welches Betriebssystem verwendet wird und wie Minetest installiert wurde.

* * **Windows:
    * Für portable Builds, d.h.: von einer.zip-Datei, gehe einfach in das Verzeichnis, in das die Datei extrahiert wurde und such nach den `games`, `worlds` und `mods` Ordnern.
    * Für installierte Builds, d.h. z.B. von einer setup.exe,
      schau in C:\\\\Minetest or C:\\\\Games\\Minetest nach.
* **GNU/Linux:***
    * Für systemweite Installationen siehe `~/.minetest`.
      Beachten, dass `~` das Heimatverzeichnis des Benutzers bedeutet und, dass Dateien und Verzeichnisse
      die mit einem Punkt (`.`) beginnen, ausgeblendet werden.
    * Für portable Installationen suche im Build-Verzeichnis.
    * Für Flatpak-Installationen siehe `~/.var/app/net.minetest.Minetest/.minetest/mods/`.
* * **MacOS****
    * Sieh unter `~/Library/Application Support/minetest/` nach.
      Beachte, dass `~` den Home Benutzer bedeutet, d.h.: `Users/USERNAME/`.

## Mod-Verzeichnis

![Finde das Verzeichnis des Mods]({{ page.root }}}/static/folder_modfolder.jpg)

Ein *Mod-Name* wird verwendet, um auf einen Mod zu verweisen. Jeder Mod sollte einen einzigartigen Namen haben.
Mod-Namen können Buchstaben, Zahlen und Unterstriche beinhalten. Ein guter Name sollte
beschreiben, was das Mod macht, und das Verzeichnis, das die Komponenten einer Mod enthält 
muss den gleichen Namen wie den Mod-Name haben.
Um herauszufinden, ob ein Mod-Name verfügbar ist, versuche ihn auf
[content.minetest.net](https://content.minetest.net) zu finden.

    mymod
    ├── init.lua (erforderlich) - Läuft, wenn das Spiel geladen wird.
    ├── mod.conf (empfohlen) - Enthält Beschreibung und Abhängigkeiten.
    ├── textures (optional)
    │ └─── .... alle Texturen oder Bilder.
    ├─── sounds (optional)
    │ └─── .... alle Sounds
    └── .... alle anderen Dateien oder Verzeichnisse

Nur die init.lua-Datei wird in einem Mod benötigt, damit sie beim Laden des Spiels läuft;
jedoch wird mod.conf empfohlen und es können weitere Komponenten erforderlich sein,
abhängig von der Funktionalität des Mods.


## Abhängigkeiten

Eine Abhängigkeit tritt auf, wenn ein Mod erfordert, dass ein anderer Mod vorher geladen werden muss.
Ein Mod kann verlangen, dass der Code, die Gegenstände oder andere Ressourcen eines anderen Mods für die weitere Verwendung verfügbar sind.  

Es gibt zwei Arten von Abhängigkeiten: harte und optionale Abhängigkeiten.
Beide erfordern, dass zuerst dieses andere Mod geladen wird. Wenn der Mod, von dem die abhängige Mod nicht 
verfügbar ist, wird eine harte Abhängigkeit dazu führen, dass der Mod nicht geladen werden kann, während eine optionale
Abhängigkeit dazu führen kann, dass der Funktionsumfang des Mods eingeschränkt bleibt.

Eine optionale Abhängigkeit ist nützlich, wenn du optional einen anderen Mod unterstützen möchtest; sie kann
de

Übersetzt mit www.DeepL.com/Translator
