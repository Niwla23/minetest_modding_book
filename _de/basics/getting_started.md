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

Das Verständnis der Grundstruktur des Ordners eines Mods ist eine wesentliche Fähigkeit, wenn es darum geht, die folgenden Punkte zu berücksichtigen
Erstellen von Mods.

* [Was sind Spiele und Mods?](#what-are-games-and-mods)
* [Wo werden Mods gespeichert?](#where-are-mods-stored)
* [Mod Directory](#mod-directory)
* [Abhängigkeiten](#dependencies)
* [Mod-Pakete](#mod-packs)
* [Beispiel](#example)


## Was sind Spiele und Mods?

Die Stärke von Minetest ist die Fähigkeit, Spiele einfach und ohne die Notwendigkeit,
 Ihre eigenen Voxel-Grafiken, Voxel-Algorithmen oder ausgefallenen Netzwerkcode zu erstellen.

In Minetest ist ein Spiel eine Sammlung von Mods, die zusammenwirken, um die folgenden Funktionen bereitzustellen
Inhalt und Verhalten eines Spiels.
Ein Modul, allgemein bekannt als Mod, ist eine Sammlung von Skripten und Ressourcen.
Es ist möglich, ein Spiel mit nur einem Mod zu machen, aber das wird selten gemacht, weil es so ist.
reduziert den Aufwand, mit dem Teile des Spiels angepasst und ausgetauscht werden können.
unabhängig von anderen.

Es ist auch möglich, Mods außerhalb eines Spiels zu vertreiben, in diesem Fall sind sie auch außerhalb eines Spiels erhältlich.
sind auch *Mods* im traditionellen Sinne - Modifikationen. Diese Mods passen sich an
oder die Funktionen eines Spiels erweitern.

Sowohl die in einem Spiel enthaltenen Mods als auch die Mods von Drittanbietern verwenden die gleiche API.

Dieses Buch behandelt die wichtigsten Teile der Minetest API,
und ist sowohl für Spieleentwickler als auch für Modder geeignet.


## Wo werden Mods gespeichert?

<a name="mod-locations"></a>

Jeder Mod hat sein eigenes Verzeichnis, in dem sich sein Lua-Code, seine Texturen, Modelle und seine
Sounds werden platziert. Minetest prüft an verschiedenen Stellen auf folgende Punkte
mods. Diese Orte werden allgemein als *modale Lastpfade* bezeichnet.

Für ein bestimmtes Welt-/Speicherspiel werden drei Mod-Standorte überprüft.
Sie sind, in Ordnung:

1. Spielmodifikationen. Dies sind die Mods, die das Spiel bilden, das die Welt spielt.
   Z.B.: `minetest/games/minetest_game/mods/`, `/usr/share/minetest/games/minetest/`
2. Globale Mods, der Ort, an dem Mods fast immer installiert sind.
   Im Zweifelsfall platzieren Sie sie hier.
   Z.B.: `minetest/mods/`minetest/mods/`.
3. World Mods, der Ort, an dem Mods gespeichert werden können, die spezifisch für einen
   besondere Welt.
   Z.B.: "Minetest/Welten/Welt/Weltmodalitäten/Weltmodalitäten/".

Minetest überprüft die Standorte in der oben angegebenen Reihenfolge. Wenn es auf einen Mod trifft.
mit dem gleichen Namen wie zuvor gefunden, wird der spätere Mod an Ort und Stelle geladen.
des früheren Mod.
Das bedeutet, dass du Spielmods überschreiben kannst, indem du einen Mod mit dem gleichen Namen platzierst.
in der globalen Mod-Position.

Der tatsächliche Standort der einzelnen Mod-Load-Pfade hängt davon ab, welches Betriebssystem Sie sind.
mit und wie Sie Minetest installiert haben.

* * **Windows:**Windows:**
    * Für portable Builds, d.h.: von einer.zip-Datei, gehen Sie einfach in das Verzeichnis, in dem sich die Datei
      Du hast die Zip-Datei extrahiert und nach den `Games`, `mods` und `worlds` gesucht.
      Verzeichnisse.
    * Für installierte Builds, d.h. von einer setup.exe,
      Schau in C:\\\\\\\Minetest oder C:\\\\\\Games\\\Minetest.
* **GNU/Linux:***
    * Für systemweite Installationen siehe `~/.minetest`.
      Beachten Sie, dass `~` das Heimatverzeichnis des Benutzers bedeutet und dass Dateien und Verzeichnisse
      die mit einem Punkt (`.`) beginnen, sind ausgeblendet.
    * Für portable Installationen suchen Sie im Build-Verzeichnis.
    * Für Flatpak-Installationen siehe `~/.var/app/net.minetest.Minetest/.minetest/mods/`.
* * **MacOS****
    * Schauen Sie unter `~/Library/Application Support/minetest/`.
      Beachten Sie, dass `~` den Benutzer Home bedeutet, d.h.: `Users/USERNAME/`.

## Mod-Verzeichnis

![Finde das Verzeichnis des Mods]({{ page.root }}}/static/folder_modfolder.jpg)

Ein *Mod-Name* wird verwendet, um auf einen Mod zu verweisen. Jeder Mod sollte einen eindeutigen Namen haben.
Mod-Namen können Buchstaben, Zahlen und Unterstriche beinhalten. Ein guter Name sollte
beschreibe, was die Mod macht, und das Verzeichnis, das die Komponenten einer Mod enthält.
muss den gleichen Namen wie der Mod-Name haben.
Um herauszufinden, ob ein Mod-Name verfügbar ist, versuche ihn zu finden unter
[content.minetest.net](https://content.minetest.net).

    mymod
    ├── init.lua (erforderlich) - Läuft, wenn das Spiel geladen wird.
    ├── mod.conf (empfohlen) - Enthält Beschreibung und Abhängigkeiten.
    ├── textures (optional)
    │ └─── .... alle Texturen oder Bilder.
    ├─── sounds (optional)
    │ └─── .... alle Sounds
    └── .... alle anderen Dateien oder Verzeichnisse

Nur die init.lua-Datei wird in einem Mod benötigt, damit sie beim Laden des Spiels läuft;
jedoch wird mod.conf empfohlen und es können weitere Komponenten erforderlich sein.
abhängig von der Funktionalität des Mods.


## Abhängigkeiten

Eine Abhängigkeit tritt auf, wenn ein Mod erfordert, dass ein anderer Mod vor sich selbst geladen wird.
Ein Mod kann verlangen, dass der Code, die Gegenstände oder andere Ressourcen eines anderen Mods verfügbar sind.
für die Verwendung.

Es gibt zwei Arten von Abhängigkeiten: harte und optionale Abhängigkeiten.
Beide erfordern, dass zuerst der Mod geladen wird. Wenn der Mod, von dem die abhängige Mod nicht ist.
verfügbar, wird eine harte Abhängigkeit dazu führen, dass der Mod nicht geladen werden kann, während eine optionale
Abhängigkeit kann dazu führen, dass weniger Funktionen aktiviert werden.

Eine optionale Abhängigkeit ist nützlich, wenn du optional einen anderen Mod unterstützen möchtest; sie kann
de

Übersetzt mit www.DeepL.com/Translator