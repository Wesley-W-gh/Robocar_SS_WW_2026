# LAFVIN Smart Robocar - Linienverfolgung

Dieses Repository enthält die Software zur autonomen Linienverfolgung eines LAFVIN Smart Robocars auf Basis eines Raspberry Pi 4. Die Architektur ist modular aufgebaut und ermöglicht die Anpassung des Fahrverhaltens über eine zentrale Konfigurationsdatei, ohne dass der Quellcode modifiziert werden muss.


Projektstruktur

* Main.py: Der Einstiegspunkt des Programms. Initialisiert das System, startet die Hauptschleife und fängt Signale wie Strg+C ab, um die Motoren beim Beenden sicher zu stoppen.
* Control.py: Enthält die Logik des Zustandsautomaten. Wertet die Sensordaten aus und steuert das Fahrverhalten inklusive des Richtungsspeichers bei temporärem Linienverlust.
* Motor.py: Die Hardware-Abstraktionsschicht für den PCA9685 PWM-Treiber zur Ansteuerung der vier Gleichstrommotoren.
* Sensor.py: Schnittstelle für die drei Infrarot-Liniensensoren. Enthält zudem ein Skript zum Testen der Sensorwerte im Terminal.
* config.json: Zentrale Konfigurationsdatei für Geschwindigkeiten, Zeitverzögerungen und Lenkfaktoren.

Installation und Ausführung

1. Repository auf den Raspberry Pi klonen und in das Verzeichnis wechseln:
   git clone https://github.com/Wesley-W-gh/Robocar.git
   cd Robocar

2. Das Hauptprogramm starten:
   python3 Main.py
   Das Programm kann jederzeit über die Tastenkombination Strg + C abgebrochen werden. Die Motoren stoppen dabei automatisch.

3. Optionaler Sensortest:
   python3 Sensor.py

Kalibrierung und Tuning (config.json)

Das Fahrverhalten wird ausschließlich über die Parameter in der config.json optimiert.

* BASE_SPEED: Grundgeschwindigkeit auf geraden Abschnitten (0-100). Standardwert: 40
* STEER_SPEED: Basisgeschwindigkeit waehrend der Kurvenfahrt. Standardwert: 35
* DELAY_CORRECTION: Zykluszeit der Hauptschleife in Sekunden. Niedrigere Werte erhöhen die Reaktionsgeschwindigkeit und mindern das Pendeln. Standardwert: 0.002
* FACTOR_STEER_INNER: Multiplikator für die kurveninneren Räder bei leichten Abweichungen. Höhere Werte stabilisieren den Geradeauslauf. Standardwert: 0.65
* FACTOR_STEER_OUTER: Multiplikator für die kurvenäußeren Räder bei leichten Abweichungen. Standardwert: 1.15
* FACTOR_SPIN: Multiplikator für die Rotation auf der Stelle bei vollständigem Linienverlust. Standardwert: 0.85

Fehlerbehebung beim Fahrverhalten:

* Starkes Pendeln auf der Geraden: Den Wert von FACTOR_STEER_INNER schrittweise erhöhen, um die Lenkkorrektur zu dämpfen, oder die BASE_SPEED verringern.
* Fahrzeug bricht in Kurven aus: Den Wert von FACTOR_STEER_INNER verringern, damit das kurveninnere Rad stärker verzögert und ein engerer Radius gefahren wird.
