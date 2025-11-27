# 🔥 Blueprint Home Assistant - Gestione Climatizzatori con Batteria e Surplus Solare

[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Compatible-41BDF5.svg)](https://www.home-assistant.io/)
[![Blueprint](https://img.shields.io/badge/Blueprint-Automation-orange.svg)](https://www.home-assistant.io/docs/automation/using_blueprints/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Automazione avanzata per Home Assistant che gestisce automaticamente i climatizzatori domestici in base allo stato di carica della batteria e al surplus di produzione solare.

## ✨ Caratteristiche

- 🔋 **Gestione intelligente batteria** - 6 livelli operativi basati su SOC
- ☀️ **Massimizza autoconsumo** - Utilizza il surplus solare disponibile
- 🏠 **8 zone climatiche** - Gestione priorità per zone giorno/notte/ospiti
- 📊 **Logica a gradini** - Attivazione progressiva in base a energia disponibile
- 🛡️ **Protezione batteria** - Cambio automatico a rete sotto 50% SOC
- 📱 **Notifiche complete** - Informazioni dettagliate su ogni cambio stato
- ⚙️ **Completamente personalizzabile** - Via interfaccia grafica HA

## 🚀 Quick Start

### 1. Prerequisiti

Crea questi helper in Home Assistant:

```yaml
# Impostazioni → Dispositivi e servizi → Helper → Crea helper → Toggle
input_boolean:
  attivazione_riscaldamento:
    name: "Attivazione Riscaldamento Automatico"
    icon: mdi:fire
  
  presenza_ospiti:
    name: "Presenza Ospiti Camera FG"
    icon: mdi:account-check

# OPZIONALE - Input text per salvare ultima notifica
input_text:
  ultima_notifica_riscaldamento:
    name: "Ultima Notifica Riscaldamento"
    max: 255
    icon: mdi:message-text
```

### 2. Installazione Blueprint

**Metodo 1 - Import automatico:**

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=YOUR_GITHUB_RAW_URL)

**Metodo 2 - Manuale:**
1. Scarica `climate_battery_management_blueprint.yaml`
2. Copia in `/config/blueprints/automation/climate/`
3. Riavvia Home Assistant

### 3. Crea Automazione

1. **Impostazioni** → **Automazioni e Scene** → **Crea automazione**
2. **Inizia con un blueprint** → Seleziona "Gestione Climatizzatori con Batteria e Surplus Solare"
3. Compila i parametri seguendo la configurazione base sotto

## ⚙️ Configurazione Base

### Sensori Energia (obbligatori)
- **Batteria SOC**: `sensor.deye_battery` (0-100%)
- **Produzione Solare**: `sensor.power_production_now` (W)
- **Carico Casa**: `sensor.deye_load_power` (W)

### Sensori Temperatura (obbligatori)
- **Temp. Esterna**: `sensor.temperature_sensor`
- **Temp. Interna Riferimento**: `sensor.a_c_sala_room_temperature`

### Climatizzatori
Associa i tuoi climatizzatori per ogni zona:
- Zone Giorno: Cucina, Sala, Soggiorno, Studio
- Zone Notte: Matrimoniale, Camera Secondaria, Camera Ospiti/FG
- Zone Ospiti: B&B

### Soglie Operative (valori consigliati)

| Parametro | Valore | Descrizione |
|-----------|--------|-------------|
| **Batteria Alta** | 90% | Attiva livello MAX |
| **Batteria Media** | 70% | Attiva livello MEDIO |
| **Batteria Bassa** | 50% | Passa a rete |
| **Surplus Alto** | 4000W | Livello MAX |
| **Surplus Medio** | 2000W | Livello MEDIO |
| **Temp. Esterna Min** | 20°C | Attiva riscaldamento |
| **Temp. Esterna Max** | 22°C | Spegne tutto |

## 🧠 Logica di Funzionamento

```
┌─────────────────────────────────────────────────────────────┐
│ SOC/SURPLUS          │ ZONE ATTIVE         │ POTENZA        │
├─────────────────────────────────────────────────────────────┤
│ >90% / >4000W  🔥    │ Tutte (8)           │ ~12-16 kW      │
│ 70-90% / 2-4kW ⚡    │ Prioritarie + FG(6) │ ~8-10 kW       │
│ 50-70% / <2kW  🟡    │ Solo essenziali (5) │ ~6-8 kW        │
│ <50%  🔄 PROTEZIONE  │ SPEGNIMENTO → RETE  │ 0 kW           │
└─────────────────────────────────────────────────────────────┘
```

### Matrice Zone per Livello

| Zona | MAX 🔥 | MEDIO ⚡ | BASE 🟡 | Temp |
|------|--------|---------|---------|------|
| Cucina | ✅ | ✅ | ✅ | Comfort |
| Sala | ✅ | ✅ | ✅ | Comfort |
| Soggiorno | ✅ | ✅ | ✅ | Comfort |
| Studio | ✅ | ❌ | ❌ | Comfort |
| Matrimoniale | ✅ | ✅ | ✅ | Eco |
| Camera Secondaria | ✅ | ✅ | ✅ | Eco |
| Camera FG | ✅* | ✅* | ❌ | Ospiti |
| B&B | ✅ | ❌ | ❌ | Comfort |

*_Condizionata a presenza ospiti_

## 📖 Documentazione Completa

Per la documentazione dettagliata con esempi, troubleshooting e FAQ, consulta:

➡️ **[ISTRUZIONI_BLUEPRINT_RISCALDAMENTO.md](ISTRUZIONI_BLUEPRINT_RISCALDAMENTO.md)**

Include:
- 📋 Setup passo-passo dettagliato
- 💡 4 esempi di configurazione completi
- 🔍 Guida troubleshooting
- ❓ FAQ estese
- 🎨 Esempi card Lovelace

## 🎯 Casi d'Uso Principali

### 1. Alta Produzione Solare
```yaml
battery_high_threshold: 85%
surplus_high_threshold: 5000W
comfort_temperature: 23°C
```
→ Massimo autoconsumo, comfort elevato

### 2. Batteria Piccola
```yaml
battery_high_threshold: 95%
battery_low_threshold: 55%
comfort_temperature: 21°C
```
→ Protezione batteria, consumo ridotto

### 3. Massimo Risparmio
```yaml
battery_high_threshold: 98%
surplus_high_threshold: 5000W
outdoor_temp_low: 16°C
comfort_temperature: 20°C
```
→ Attivazione minima, costi ridotti al minimo

## 🔧 Requisiti Tecnici

- **Home Assistant**: 2023.8 o superiore
- **Integrazione Batteria**: Deye, Huawei, SolarEdge, Victron, etc.
- **Integrazione Climatizzatori**: SmartIR, Broadlink, Sensibo, controllo nativo
- **Sensori Temperatura**: Qualsiasi sensore compatibile HA

## 📊 Dashboard Esempio

```yaml
type: entities
title: Riscaldamento Solare
entities:
  - entity: input_boolean.attivazione_riscaldamento
  - entity: sensor.deye_battery
  - entity: sensor.power_production_now
  - entity: sensor.deye_load_power
  - entity: input_text.ultima_notifica_riscaldamento
  - entity: sensor.temperature_sensor
  - entity: sensor.a_c_sala_room_temperature
```

## 🐛 Troubleshooting Rapido

**Automazione non si attiva?**
- Verifica `input_boolean.attivazione_riscaldamento` = ON
- Controlla temperatura esterna < 20°C
- Verifica SOC batteria e surplus

**Cicli continui on/off?**
- Aumenta `comfort_hysteresis` a 1.0°C
- Separa le soglie batteria (es. 90/65/45)

**Nessuna notifica?**
- Verifica nome servizio in Developer Tools → Servizi
- Test manuale: `service: notify.mobile_app_nome`

## 🤝 Contributi

Pull request e segnalazioni bug sono benvenute!

1. Fork del repository
2. Crea feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit modifiche (`git commit -m 'Add AmazingFeature'`)
4. Push al branch (`git push origin feature/AmazingFeature`)
5. Apri Pull Request

## 📝 Changelog

### v1.0.0 (2024-11)
- ✨ Release iniziale
- 🔋 6 livelli gestione batteria
- ☀️ Integrazione surplus solare
- 🏠 Supporto 8 zone climatiche
- 📱 Sistema notifiche completo

## 📄 Licenza

Questo progetto è rilasciato sotto licenza MIT - vedi file [LICENSE](LICENSE) per dettagli.

## 🙏 Ringraziamenti

- Community Home Assistant Italia
- Tutti i beta tester
- Contributi open source

## 💬 Supporto

- 📖 [Documentazione Completa](ISTRUZIONI_BLUEPRINT_RISCALDAMENTO.md)
- 💬 [Forum Home Assistant Italiano](https://forum.hassiohelp.eu)
- 💬 [Telegram Home Assistant Italia](https://t.me/HassioHelp)
- 🐛 [Segnala Bug](../../issues)

---

**Made with ❤️ for the Home Assistant Community**

Se questo blueprint ti è utile, considera di lasciare una ⭐ su GitHub!
