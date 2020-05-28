# OMXDecoder (Raspberry Pi Hardware H.264/AAC Live Decoder)

Konfiguration für Raspis zum Decodieren von eingehenden Live-Signalen

## Empfohlene Hardware

-   Raspberry Pi 3B
-   BMD UpDownCross HD (für Reclocking / Signalwandlung)

## Systemvorbereitung

Notwendige Pakete installieren:
`sudo apt update; sudo apt upgrade; sudo apt install omxplayer ntp`


/boot/cmdline.txt (HINTEN ANFÜGEN!):

```
splash quiet plymouth.ignore-serial-consoles logo.nologo vt.global_cursor_default=0 fbcon=map:2
```

## Nutzung

-   Automatischer Start des Diensts aktivieren mit `sudo systemctl enable omxplayer.service`
-   Dienst starten mit `sudo systemctl start omxplayer.service`
-   Dienst neustarten mit `sudo systemctl restart omxplayer.service`

Wenn kein Signal anliegt startet der Prozess andauernd neu. Ebenso falls der Prozess wegen irgendeinem Fehler während des Streams mal terminieren sollte. Nach wenigen Sekunden sollte es dann weiter gehen.
Der leere Hintergrund (blank) ist absichtlich leicht grau, damit man sieht, dass das System ein Signal ausgibt bzw. ob der omxplayer selbst läuft.
