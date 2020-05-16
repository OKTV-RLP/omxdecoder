# Raspi-Decoder

Konfiguration für Raspis zum Decodieren von eingehenden Live-Signalen

## Anpassung für die Ausgabe

Achtung: Wenn Dateien an den entsprechenden Orten nicht vorhanden sind, müssen sie angelegt werden.

/etc/omxplayer.conf

```
OMXPLAYER_OPT="-o hdmi --hw --nativedeinterlace --hdmiclocksync --refresh --live --blank=0xff3f3f3f --no-keys --no-osd --timeout 5"
# url or filename to be played by omxplayer:
OMXPLAYER_URL=[Streaming URL]
```

[Streaming URL] durch die jeweilig verwendete URL austauschen. z.B: rtmp://stream-cdn.ok54.de/tv/ok54_live

/etc/rsyslog.d/omxplayer.conf

```
:msg,contains,"omxplayer" /tmp/omxplayer.log
& ~
```

/etc/systemd/system/omxplayer.service

```
[Unit]
Description=omxplayer Daemon
After=network.target
StartLimitIntervalSec=0

[Service]
EnvironmentFile=/etc/omxplayer.conf
ExecStart=/usr/bin/omxplayer $OMXPLAYER_OPT "$OMXPLAYER_URL"
Type=simple
Restart=always
RestartSec=10
StartLimitBurst=0

[Install]
WantedBy=multi-user.target
```

/boot/cmdline.txt (HINTEN ANFÜGEN!):

```
splash quiet plymouth.ignore-serial-consoles logo.nologo vt.global_cursor_default=0 fbcon=map:2
```

/boot/config.txt durch die gleichnamige Datei in diesem Verzeichnis ersetzen.

## Nutzung

-   Automatischer Start des Diensts mit `sudo systemctl enable omxplayer.service`
-   Dienst starten mit `sudo systemctl start omxplayer.service`
-   Dienst neustarten mit `sudo systemctl restart omxplayer.service`

Wenn kein Signal anliegt startet der Prozess andauernd neu. Ebenso falls der Prozess wegen irgendeinem Fehler während des Streams mal terminieren sollte. Nach wenigen Sekunden sollte es dann weiter gehen.
Das der leere Hintergrund (blank) so leicht grau ist ist absicht damit man sieht dass das System ein Signal ausgibt bzw. ob der omxplayer selbst läuft.
