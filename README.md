# SchreinSerialParser

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Platform](https://img.shields.io/badge/platform-Arduino-00979D.svg)

Une bibliothèque Arduino ultra-optimisée pour la communication série industrielle, conçue pour les systèmes embarqués critiques avec gestion robuste des trames et validation d'intégrité.

## 🚀 Caractéristiques

- **🎯 Haute Performance** : Parsing de trames sans allocations mémoire dynamiques
- **🛡️ Validation Intégrale** : Checksum XOR et gestion de timeout configurables
- **📡 Protocole Robust** : Format de trame `<[control|key|value]&checksum>`
- **⚡ Mémoire Optimisée** : Utilisation exclusive de C-strings (pas de String Arduino)
- **🔧 Architecture Événementielle** : Système de callbacks asynchrone
- **🏭 Industrie 4.0** : Conforme aux exigences des applications industrielles critiques

## 📦 Installation

### Méthode 1 : Gestionnaire de Bibliothèque Arduino
1. Ouvrez l'IDE Arduino
2. Allez dans `Outils` → `Gérer les bibliothèques...`
3. Recherchez "SchreinSerialParser"
4. Cliquez sur "Installer"

### Méthode 2 : Installation Manuelle
1. Téléchargez la dernière version [ici](https://github.com/yourusername/SchreinSerialParser/releases)
2. Extrayez le ZIP dans votre dossier `Arduino/libraries/`
3. Redémarrez l'IDE Arduino

## 🎯 Utilisation Rapide

```cpp
#include <SchreinSerialParser.h>

SchreinSerialParser parser(Serial);

void setup() {
  Serial.begin(9600);
  parser.setTimeout(1000);
  parser.enableChecksum(true);
  
  parser.onFrameParsed([](const char* control, const char* key, const char* value) {
    Serial.print("Reçu: ");
    Serial.print(control);
    Serial.print(".");
    Serial.print(key);
    Serial.print(" = ");
    Serial.println(value);
  });
}

void loop() {
  parser.loop();
  
  // Envoyer une commande
  String cmd = SchreinSerialParser::command("sensor", "read_temp", "");
  parser.sendFrame(cmd.c_str());
}
