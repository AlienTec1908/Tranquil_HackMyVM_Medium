# Tranquil - HackMyVM (Medium)
 
![Tranquil.png](Tranquil.png)

## Übersicht

*   **VM:** Tranquil
*   **Plattform:** HackMyVM (https://hackmyvm.eu/machines/machine.php?vm=Tranquil)
*   **Schwierigkeit:** Medium
*   **Autor der VM:** DarkSpirit
*   **Datum des Writeups:** 19. November 2022
*   **Original-Writeup:** https://alientec1908.github.io/Tranquil_HackMyVM_Medium/
*   **Autor:** Ben C.

## Kurzbeschreibung

Das Ziel der "Tranquil"-Challenge war die Erlangung von User- und Root-Rechten. Der Weg begann mit der Entdeckung eines SSH-Dienstes, der auf dem unüblichen Port 21 lief. Eine `curl`-Anfrage an diesen Port enthüllte Hinweise auf den Benutzernamen `guru` und eine Bilddatei `tranquil.jpg`. Die Bilddatei (deren Beschaffungsmethode im Log nicht gezeigt wird) enthielt eine mit dem Hexahue-Chiffre kodierte Nachricht (`12,5,5,17,4,2,13,14`). Die Entschlüsselung dieser Nachricht (z.B. mit `dcode.fr`) ergab das Passwort `KEEPCALM`. Dies ermöglichte den SSH-Login als Benutzer `guru`. Die User-Flag wurde in dessen Home-Verzeichnis gefunden. Der genaue Weg zur Root-Privilegieneskalation und zum Auffinden der Root-Flag ist im bereitgestellten Log nicht dokumentiert.

## Disclaimer / Wichtiger Hinweis

Die in diesem Writeup beschriebenen Techniken und Werkzeuge dienen ausschließlich zu Bildungszwecken im Rahmen von legalen Capture-The-Flag (CTF)-Wettbewerben und Penetrationstests auf Systemen, für die eine ausdrückliche Genehmigung vorliegt. Die Anwendung dieser Methoden auf Systeme ohne Erlaubnis ist illegal. Der Autor übernimmt keine Verantwortung für missbräuchliche Verwendung der hier geteilten Informationen. Handeln Sie stets ethisch und verantwortungsbewusst.

## Verwendete Tools

*   `arp-scan`
*   `nmap`
*   `curl`
*   `dcode.fr` (oder ähnliches Online-Tool für Hexahue, impliziert)
*   `ssh`
*   `cat`
*   Standard Linux-Befehle

## Lösungsweg (Zusammenfassung)

Der Angriff auf die Maschine "Tranquil" gliederte sich in folgende Phasen:

1.  **Reconnaissance:**
    *   IP-Findung mit `arp-scan` (`192.168.2.126`).
    *   `nmap`-Scan identifizierte einen SSH-Dienst (OpenSSH 8.4p1) auf dem nicht standardmäßigen Port 21. Kein anderer Port war offen.

2.  **Initial Access (Hinweis-Extraktion & Steganographie):**
    *   Eine `curl`-Anfrage an `http://192.168.2.126:21` (obwohl es ein SSH-Port ist) enthüllte im initialen Server-Output einen Hinweis auf den Benutzernamen `guru` und die Datei `tranquil.jpg`.
    *   *Die Methode zur Beschaffung der `tranquil.jpg` ist im Log nicht dokumentiert.*
    *   Die Analyse der `tranquil.jpg` offenbarte eine mit dem Hexahue-Chiffre kodierte Zahlenfolge: `12,5,5,17,4,2,13,14`.
    *   Entschlüsselung des Hexahue-Ciphers (z.B. mit `dcode.fr`) ergab das Passwort `KEEPCALM`.
    *   Erfolgreicher SSH-Login als `guru` mit dem Passwort `KEEPCALM` auf Port 21 (`ssh -p 21 guru@tranquil`).
    *   User-Flag `HMVbecauseweare` in `/home/guru/user.txt` gelesen.

3.  **Privilege Escalation (zu `root`):**
    *   *Der detaillierte Weg zur Root-Privilegieneskalation und zum Auffinden der Root-Flag ist im bereitgestellten Log nicht dokumentiert.*
    *   Die Root-Flag `HMVyourfriends` wurde gefunden (Pfad `/root/root.txt`).

## Wichtige Schwachstellen und Konzepte

*   **SSH auf nicht standardmäßigem Port:** Erschwerte die Entdeckung geringfügig.
*   **Informationsleck im SSH-Banner/Initial-Output:** Enthielt Hinweise auf Benutzernamen und eine relevante Datei.
*   **Steganographie (Hexahue-Chiffre in Bild):** Ein Passwort wurde mittels einer Farbchiffre in einem Bild versteckt.
*   **Schwaches Passwort (durch Steganographie versteckt):** Das Passwort `KEEPCALM` war, obwohl versteckt, nicht stark.

## Flags

*   **User Flag (`/home/guru/user.txt`):** `HMVbecauseweare`
*   **Root Flag (`/root/root.txt`):** `HMVyourfriends` (Weg zur Erlangung nicht im Log dokumentiert)

## Tags

`HackMyVM`, `Tranquil`, `Medium`, `SSH`, `Steganography`, `Hexahue Cipher`, `Password Discovery`, `Linux`, `Privilege Escalation` (teilweise)
