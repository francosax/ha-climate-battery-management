# Blueprint: Gestione Climatizzatori con Batteria e Surplus Solare

## 📋 Indice
1. [Introduzione](#introduzione)
2. [Prerequisiti](#prerequisiti)
3. [Installazione](#installazione)
4. [Configurazione](#configurazione)
5. [Logica di Funzionamento](#logica-di-funzionamento)
6. [Parametri Dettagliati](#parametri-dettagliati)
7. [Esempi di Configurazione](#esempi-di-configurazione)
8. [Troubleshooting](#troubleshooting)
9. [FAQ](#faq)

---

## 🎯 Introduzione

Questo blueprint implementa un sistema avanzato di gestione automatica dei climatizzatori basato su:
- **Stato di carica (SOC)** della batteria domestica
- **Surplus di produzione solare** disponibile
- **Temperature interne ed esterne**
- **Presenza ospiti** nelle zone secondarie

### Vantaggi principali
✅ **Massimizza l'autoconsumo** dell'energia solare  
✅ **Protegge la batteria** evitando scariche eccessive  
✅ **Gestione intelligente a priorità** delle zone climatiche  
✅ **Notifiche dettagliate** su ogni cambio di stato  
✅ **Completamente personalizzabile** tramite interfaccia grafica  

---

## 🔧 Prerequisiti

### Hardware richiesto
- Home Assistant installato e funzionante
- Sistema fotovoltaico con batteria di accumulo
- Climatizzatori compatibili con Home Assistant (tramite integrazione IR, WiFi, MQTT, ecc.)
- Sensori di temperatura interni ed esterni

### Integrazioni necessarie
- Integrazione del sistema di batteria/inverter (es. Deye, Huawei, Solaredge, ecc.)
- Integrazione climatizzatori (es. SmartIR, Broadlink, Sensibo, controllo nativo del produttore)
- Sensori temperatura (es. Zigbee, Z-Wave, ESPHome)

### Entità da creare preventivamente in Home Assistant

#### 1. Input Boolean (Helpers → Toggle)
```yaml
# In configuration.yaml o tramite UI (Impostazioni → Dispositivi e servizi → Helper)
input_boolean:
  attivazione_riscaldamento:
    name: "Attivazione Riscaldamento Automatico"
    icon: mdi:fire
  
  presenza_piero:
    name: "Presenza Ospiti Camera FG"
    icon: mdi:account-check
```

#### 2. Input Number (Helpers → Number) - OPZIONALI
Se vuoi usare input_number invece dei valori di default del blueprint:
```yaml
input_number:
  temperatura_comfort_2:
    name: "Temperatura Comfort Zone Giorno"
    min: 18
    max: 26
    step: 0.5
    unit_of_measurement: "°C"
    icon: mdi:thermometer
  
  temperatura_minima_eco:
    name: "Temperatura Eco Zone Notte"
    min: 16
    max: 22
    step: 0.5
    unit_of_measurement: "°C"
    icon: mdi:thermometer-low
  
  t_ospiti_studio:
    name: "Temperatura Camera Ospiti/Studio"
    min: 16
    max: 24
    step: 0.5
    unit_of_measurement: "°C"
    icon: mdi:thermometer
```

#### 3. Input Text (Helpers → Text) - OPZIONALE
Per salvare l'ultima notifica inviata:
```yaml
input_text:
  ultima_notifica_riscaldamento:
    name: "Ultima Notifica Riscaldamento"
    max: 255
    icon: mdi:message-text
```

---

## 📥 Installazione

### Metodo 1: Import diretto (consigliato)
1. Apri Home Assistant
2. Vai su **Impostazioni** → **Automazioni e Scene** → **Blueprint**
3. Clicca sul pulsante **Importa blueprint** (in basso a destra)
4. Inserisci l'URL del blueprint (se ospitato su GitHub) oppure:

### Metodo 2: Import manuale
1. Scarica il file `climate_battery_management_blueprint.yaml`
2. Copia il file nella cartella `/config/blueprints/automation/`
3. Crea la sottocartella se non esiste: `/config/blueprints/automation/climate/`
4. Riavvia Home Assistant oppure vai su **Impostazioni** → **Sistema** → **YAML** → **Ricarica configurazione automazioni**

### Verifica installazione
1. Vai su **Impostazioni** → **Automazioni e Scene**
2. Clicca su **Crea automazione** → **Inizia con un blueprint**
3. Cerca "Gestione Climatizzatori con Batteria e Surplus Solare"

---

## ⚙️ Configurazione

### Passo 1: Crea una nuova automazione dal blueprint
1. **Impostazioni** → **Automazioni e Scene** → **Crea automazione**
2. Seleziona **Inizia con un blueprint**
3. Trova "Gestione Climatizzatori con Batteria e Surplus Solare"
4. Dai un nome all'automazione (es. "Gestione Riscaldamento Solare")

### Passo 2: Configura i sensori energia
| Parametro | Descrizione | Esempio |
|-----------|-------------|---------|
| **Sensore SOC Batteria** | Percentuale carica batteria (0-100%) | `sensor.deye_battery` |
| **Sensore Produzione Solare** | Potenza solare istantanea (W) | `sensor.power_production_now` |
| **Sensore Carico Attuale** | Consumo totale casa (W) | `sensor.deye_load_power` |

### Passo 3: Configura i sensori temperatura
| Parametro | Descrizione | Esempio |
|-----------|-------------|---------|
| **Temperatura Esterna** | Sensore temperatura outdoor | `sensor.temperature_sensor` |
| **Temperatura Riferimento** | Temperatura zona principale (sala) | `sensor.a_c_sala_room_temperature` |

### Passo 4: Associa i climatizzatori
Seleziona i tuoi climatizzatori per ogni zona:

#### Zone Giorno (priorità ALTA)
- **Cucina**: `climate.a_c_cucina`
- **Sala**: `climate.a_c_sala`
- **Soggiorno**: `climate.a_c_soggiorno`
- **Studio**: `climate.a_c_studio` (attivo solo livello MAX)

#### Zone Notte (priorità MEDIA)
- **Camera Matrimoniale**: `climate.a_c_camera_matrimoniale`
- **Camera Demetrio**: `climate.a_c_demetrio`
- **Camera Ospiti/FG**: `climate.a_c_camera_figli_grandi` (condizionato a presenza)

#### Zone Ospiti (priorità BASSA)
- **B&B**: `climate.a_c_b_b` (attivo solo livello MAX)

### Passo 5: Configura i controlli
| Parametro | Descrizione | Esempio |
|-----------|-------------|---------|
| **Interruttore Abilitazione** | On/Off automazione | `input_boolean.attivazione_riscaldamento` |
| **Presenza Ospiti Camera FG** | Attiva/disattiva camera ospiti | `input_boolean.presenza_piero` |

### Passo 6: Imposta le temperature
Puoi usare i valori di default del blueprint oppure collegare input_number:

| Parametro | Default | Range | Descrizione |
|-----------|---------|-------|-------------|
| **Comfort Zone Giorno** | 22°C | 18-26°C | Cucina, sala, soggiorno |
| **Eco Zone Notte** | 19°C | 16-22°C | Camere matrimoniale e Demetrio |
| **Camera Ospiti/Studio** | 19°C | 16-24°C | Camera FG quando presente |

### Passo 7: Configura le soglie operative

#### Soglie Batteria
| Parametro | Default | Descrizione |
|-----------|---------|-------------|
| **Batteria Alta** | 90% | Attiva livello MAX |
| **Batteria Media** | 70% | Attiva livello MEDIO |
| **Batteria Bassa** | 50% | Passa ad automazione rete |

#### Soglie Surplus
| Parametro | Default | Descrizione |
|-----------|---------|-------------|
| **Surplus Alto** | 4000W | Attiva livello MAX |
| **Surplus Medio** | 2000W | Attiva livello MEDIO |

#### Soglie Temperatura
| Parametro | Default | Descrizione |
|-----------|---------|-------------|
| **Temp. Esterna Minima** | 20°C | Sotto questa si attiva |
| **Temp. Esterna Massima** | 22°C | Sopra questa si spegne |
| **Isteresi Comfort** | 0.5°C | Gradi sopra target per spegnere |

### Passo 8: Notifiche (opzionale)
- **Servizio Notifica**: Inserisci il nome del servizio (es. `notify.mobile_app_il_tuo_smartphone`)
- **Entity Ultima Notifica**: Seleziona `input_text.ultima_notifica_riscaldamento`

---

## 🧠 Logica di Funzionamento

### Matrice decisionale

```
┌─────────────────────────────────────────────────────────────────┐
│                    CONDIZIONI PRIORITARIE                        │
│                    (Check in ordine)                             │
└─────────────────────────────────────────────────────────────────┘
         │
         ├─► 1. COMFORT RAGGIUNTO? (Sala > Target + 0.5°C)
         │   └─► SÌ → SPEGNI TUTTO ❄️
         │
         ├─► 2. TEMPERATURA ESTERNA ALTA? (> 22°C)
         │   └─► SÌ → SPEGNI TUTTO 🌡️
         │
         ├─► 3. LIVELLO MAX? (SOC > 90% O Surplus > 4000W) E Temp < 20°C
         │   └─► SÌ → ATTIVA TUTTE LE ZONE 🔥
         │       ├─ Giorno (22°C): Cucina, Sala, Soggiorno, Studio
         │       ├─ Notte (19°C): Matrimoniale, Demetrio
         │       ├─ Ospiti (19°C): Camera FG (se presente)
         │       └─ B&B (22°C)
         │
         ├─► 4. LIVELLO MEDIO? (70% < SOC < 90% O 2000W < Surplus < 4000W)
         │   └─► SÌ → ATTIVA ZONE PRIORITARIE + CAMERA FG ⚡
         │       ├─ Giorno (22°C): Cucina, Sala, Soggiorno
         │       ├─ Notte (19°C): Matrimoniale, Demetrio
         │       ├─ Ospiti (19°C): Camera FG (se presente)
         │       └─ SPENTI: Studio, B&B
         │
         ├─► 5. LIVELLO BASE? (50% < SOC < 70% E Surplus < 2000W)
         │   └─► SÌ → SOLO ZONE ESSENZIALI 🟡
         │       ├─ Giorno (22°C): Cucina, Sala, Soggiorno
         │       ├─ Notte (19°C): Matrimoniale, Demetrio
         │       └─ SPENTI: Studio, Camera FG, B&B
         │
         ├─► 6. BATTERIA CRITICA? (SOC < 50%)
         │   └─► SÌ → SPEGNI TUTTO + PASSA A RETE 🔄
         │
         └─► 7. NESSUNA CONDIZIONE?
             └─► SPEGNI TUTTO (default) 🔴
```

### Priorità zone climatiche

| Livello | SOC | Surplus | Zone Attive | Potenza | Note |
|---------|-----|---------|-------------|---------|------|
| **MAX** 🔥 | >90% | >4000W | Tutte (8 zone) | ~12-16 kW | Massimo comfort |
| **MEDIO** ⚡ | 70-90% | 2000-4000W | Prioritarie + FG (6 zone) | ~8-10 kW | Comfort ottimizzato |
| **BASE** 🟡 | 50-70% | <2000W | Solo essenziali (5 zone) | ~6-8 kW | Risparmio energetico |
| **PROTEZIONE** 🔄 | <50% | N/A | Spegnimento totale | 0 kW | **Passa a rete/generatore** |

---

## 📚 Parametri Dettagliati

### Sensori Energia

#### `battery_sensor` - Sensore SOC Batteria
- **Tipo**: `sensor` con `device_class: battery`
- **Unità**: Percentuale (0-100%)
- **Requisiti**: Deve essere un sensore numerico che rappresenta lo stato di carica
- **Esempio**: 
  ```yaml
  sensor.deye_battery
  sensor.huawei_battery_state_of_capacity
  sensor.victron_battery_soc
  ```

#### `solar_production_sensor` - Produzione Solare
- **Tipo**: `sensor` con `device_class: power`
- **Unità**: Watt (W)
- **Requisiti**: Potenza istantanea di produzione fotovoltaica
- **Esempio**:
  ```yaml
  sensor.power_production_now
  sensor.solaredge_current_power
  sensor.deye_pv_power
  ```

#### `load_power_sensor` - Carico Casa
- **Tipo**: `sensor` con `device_class: power`
- **Unità**: Watt (W)
- **Requisiti**: Consumo totale istantaneo della casa
- **Esempio**:
  ```yaml
  sensor.deye_load_power
  sensor.house_consumption
  sensor.grid_power
  ```

> **💡 Nota**: Il surplus viene calcolato automaticamente come:  
> `surplus = produzione_solare - carico_casa`

### Sensori Temperatura

#### `outdoor_temp_sensor` - Temperatura Esterna
- **Tipo**: `sensor` con `device_class: temperature`
- **Unità**: Gradi Celsius (°C)
- **Requisiti**: Deve rilevare temperatura esterna per trigger attivazione
- **Esempio**:
  ```yaml
  sensor.temperature_sensor
  sensor.outdoor_temperature
  weather.home  # Usa l'attributo temperature
  ```

#### `reference_indoor_temp_sensor` - Temperatura Interna Riferimento
- **Tipo**: `sensor` con `device_class: temperature`
- **Unità**: Gradi Celsius (°C)
- **Requisiti**: Temperatura della zona principale (es. sala) per controllo comfort
- **Esempio**:
  ```yaml
  sensor.a_c_sala_room_temperature
  sensor.living_room_temperature
  climate.sala  # Usa l'attributo current_temperature
  ```

### Climatizzatori

Tutti i climatizzatori devono essere entità di tipo `climate.*` che supportano:
- `hvac_mode: heat` - Modalità riscaldamento
- `temperature` - Setpoint temperatura
- `turn_off` - Spegnimento

**Esempio di entità compatibili**:
```yaml
climate.a_c_cucina
climate.broadlink_sala
climate.sensibo_soggiorno
climate.mitsubishi_studio
```

### Controlli Input

#### `enable_heating_switch` - Interruttore Abilitazione
- **Tipo**: `input_boolean`
- **Funzione**: Master on/off per l'intera automazione
- **Comportamento**: 
  - `ON`: Automazione attiva, applica logica a gradini
  - `OFF`: Automazione disabilitata, nessuna azione

#### `guest_presence_switch` - Presenza Ospiti
- **Tipo**: `input_boolean`
- **Funzione**: Controlla se attivare la Camera FG ai livelli MEDIO e MAX
- **Comportamento**:
  - `ON`: Camera FG inclusa nella gestione
  - `OFF`: Camera FG sempre spenta

### Soglie Operative

#### Batteria
- **`battery_high_threshold`** (default: 90%)
  - Sopra questa soglia → Livello MAX
  - Range consigliato: 80-95%
  
- **`battery_medium_threshold`** (default: 70%)
  - Sopra questa soglia → Livello MEDIO
  - Range consigliato: 60-80%
  
- **`battery_low_threshold`** (default: 50%)
  - **SOGLIA CRITICA**: Sotto questa soglia → Spegnimento totale e passaggio al controllo rete/generatore
  - Questa è la soglia di protezione della batteria
  - Range consigliato: 40-60%
  - **Importante**: Imposta questa soglia coordinandola con l'avvio del tuo generatore o con l'automazione che gestisce il prelievo da rete

#### Surplus Solare
- **`surplus_high_threshold`** (default: 4000W)
  - Sopra questo surplus → Livello MAX
  - Calcolo: Stima 1500-2000W per climatizzatore attivo
  
- **`surplus_medium_threshold`** (default: 2000W)
  - Sopra questo surplus → Livello MEDIO
  - Deve essere < surplus_high_threshold

#### Temperatura
- **`outdoor_temp_low`** (default: 20°C)
  - Sotto questa temperatura → Attiva riscaldamento
  - Range consigliato: 15-22°C
  
- **`outdoor_temp_high`** (default: 22°C)
  - Sopra questa temperatura → Spegni tutto
  - Deve essere > outdoor_temp_low
  
- **`comfort_hysteresis`** (default: 0.5°C)
  - Gradi sopra target per trigger spegnimento
  - Previene oscillazioni continue on/off
  - Range consigliato: 0.3-1.5°C

### Notifiche

#### `notification_service` (Opzionale)
- **Formato**: Nome del servizio notify
- **Esempio**: 
  ```
  notify.mobile_app_iphone_di_franco
  notify.telegram
  notify.all_devices
  ```
- **Lascia vuoto** per disabilitare le notifiche

#### `notification_text_entity` (Opzionale)
- **Tipo**: `input_text`
- **Funzione**: Salva l'ultima notifica per visualizzazione in UI
- **Lascia vuoto** se non necessario

---

## 💡 Esempi di Configurazione

### Configurazione 1: Casa con Alta Produzione Solare

**Scenario**: Impianto 10kW con batteria 15kWh, priorità all'autoconsumo massimo

```yaml
# Soglie batteria aggressive
battery_high_threshold: 85%   # Attiva prima il livello MAX
battery_medium_threshold: 65% # Livello MEDIO più ampio
battery_low_threshold: 45%    # Protegge batteria da scarica profonda

# Soglie surplus ottimizzate
surplus_high_threshold: 5000W  # Impianto grande, può gestire più zone
surplus_medium_threshold: 2500W

# Temperature comfort elevato
comfort_temperature: 23°C      # Comfort maggiore
eco_temperature: 20°C          # Anche notte più calda
guest_temperature: 21°C
```

**Risultato**: Massimo sfruttamento dell'energia solare, comfort elevato, batteria sempre carica >45%.

---

### Configurazione 2: Casa con Batteria Piccola

**Scenario**: Impianto 6kW con batteria 5kWh, necessità di proteggere la batteria

```yaml
# Soglie batteria conservative
battery_high_threshold: 95%   # Solo con batteria quasi piena
battery_medium_threshold: 75% # Fascia media ristretta
battery_low_threshold: 55%    # Passa a rete prima

# Soglie surplus moderate
surplus_high_threshold: 3500W
surplus_medium_threshold: 2000W

# Temperature moderate
comfort_temperature: 21°C
eco_temperature: 18°C
guest_temperature: 18°C

# Temperature esterne più restrittive
outdoor_temp_low: 18°C        # Attiva solo con freddo vero
outdoor_temp_high: 20°C       # Spegne prima quando scalda
```

**Risultato**: Batteria protetta, riscaldamento solo quando strettamente necessario, costi ridotti.

---

### Configurazione 3: Uso Misto Giorno/Notte

**Scenario**: Casa occupata soprattutto la sera, ottimizzazione per rientro serale

```yaml
# Soglie bilanciate
battery_high_threshold: 90%
battery_medium_threshold: 70%
battery_low_threshold: 50%

# Surplus standard
surplus_high_threshold: 4000W
surplus_medium_threshold: 2000W

# Temperature differenziate per comfort serale
comfort_temperature: 22.5°C   # Giorno standard
eco_temperature: 20°C         # Notte più calda per comfort notturno
guest_temperature: 19.5°C

# Isteresi più ampia per ridurre cicli on/off
comfort_hysteresis: 1.0°C
```

**Risultato**: Accumulo solare diurno, rilascio serale con comfort ottimale.

---

### Configurazione 4: Massimo Risparmio

**Scenario**: Priorità assoluta al risparmio, comfort minimo accettabile

```yaml
# Soglie molto conservative
battery_high_threshold: 98%   # Livello MAX solo eccezionale
battery_medium_threshold: 80%
battery_low_threshold: 60%

# Surplus elevato richiesto
surplus_high_threshold: 5000W
surplus_medium_threshold: 3000W

# Temperature minime
comfort_temperature: 20°C
eco_temperature: 17°C
guest_temperature: 17°C

# Temperatura esterna molto bassa per attivazione
outdoor_temp_low: 16°C        # Solo con freddo intenso
outdoor_temp_high: 19°C       # Spegne appena possibile

# Isteresi ampia per ridurre cicli
comfort_hysteresis: 1.5°C
```

**Risultato**: Costi minimi, attivazione solo in caso di reale necessità, batteria sempre carica.

---

## 🔍 Troubleshooting

### Problema: L'automazione non si attiva mai

**Possibili cause**:

1. **Interruttore abilitazione spento**
   - Verifica che `input_boolean.attivazione_riscaldamento` sia `ON`
   - Controlla in: Impostazioni → Dispositivi e servizi → Helper

2. **Temperatura esterna troppo alta**
   - Controlla il valore di `outdoor_temp_sensor`
   - Deve essere < `outdoor_temp_low` (default 20°C)
   - Soluzione: Abbassa `outdoor_temp_low` se necessario

3. **Batteria e surplus sotto soglia**
   - Verifica SOC batteria e surplus calcolato
   - Usa Developer Tools → Stati per controllare i valori
   - Abbassa le soglie se necessario

**Debug**:
```yaml
# Aggiungi un'automazione di debug temporanea
automation:
  - alias: "Debug Riscaldamento"
    trigger:
      - platform: state
        entity_id: sensor.deye_battery
    action:
      - service: notify.persistent_notification.create
        data:
          message: >
            SOC: {{ states('sensor.deye_battery') }}%
            Surplus: {{ (states('sensor.power_production_now')|float - states('sensor.deye_load_power')|float)|round(0) }}W
            Temp Esterna: {{ states('sensor.temperature_sensor') }}°C
            Abilitazione: {{ states('input_boolean.attivazione_riscaldamento') }}
```

---

### Problema: Climatizzatori si accendono e spengono continuamente

**Causa**: Isteresi insufficiente o soglie troppo vicine

**Soluzioni**:

1. **Aumenta l'isteresi di comfort**:
   ```yaml
   comfort_hysteresis: 1.0°C  # Invece di 0.5°C
   ```

2. **Aumenta il tempo di attesa nei trigger**:
   - I trigger temperatura hanno già 10-15 minuti di `for:`
   - Puoi aumentarli modificando l'automazione creata

3. **Separa maggiormente le soglie batteria**:
   ```yaml
   battery_high_threshold: 90%
   battery_medium_threshold: 65%  # Invece di 70%
   battery_low_threshold: 45%     # Invece di 50%
   ```

---

### Problema: Non ricevo notifiche

**Verifica**:

1. **Servizio notifica corretto**:
   ```yaml
   # Trova il nome esatto in Developer Tools → Servizi
   # Cerca "notify" e vedi quali servizi sono disponibili
   ```

2. **Test manuale**:
   ```yaml
   # Developer Tools → Servizi
   service: notify.mobile_app_tuo_telefono
   data:
     message: "Test notifica"
   ```

3. **Controlla i log**:
   - Impostazioni → Sistema → Log
   - Cerca errori relativi a "notify" o al blueprint

---

### Problema: Camera FG non si attiva mai

**Verifica**:

1. **Input boolean presenza attivo**:
   ```yaml
   input_boolean.presenza_piero: "on"
   ```

2. **Livello operativo raggiunto**:
   - Camera FG si attiva solo ai livelli MEDIO e MAX
   - Verifica che SOC > 70% O surplus > 2000W

3. **Climatizzatore funzionante**:
   - Prova ad accenderlo manualmente
   - Verifica lo stato in Developer Tools

---

### Problema: Il surplus è sempre negativo

**Causa**: Sensore load_power non configurato correttamente

**Verifica formula**:
```yaml
surplus = produzione_solare - carico_casa
```

Se il carico include già la produzione (alcuni inverter lo fanno), usa:
```yaml
# Modifica l'automazione generata e cambia:
surplus_power: "{{ states(!input solar_production_sensor) | float(0) }}"
```

---

### Problema: Spegnimento improvviso con batteria alta

**Possibili cause**:

1. **Comfort raggiunto**:
   - La temperatura interna ha superato `target + isteresi`
   - È un comportamento corretto
   - Controlla `sensor.a_c_sala_room_temperature`

2. **Temperatura esterna salita**:
   - Superato `outdoor_temp_high` per più di 10 minuti
   - Comportamento corretto
   - Controlla `sensor.temperature_sensor`

---

## ❓ FAQ

### D: Posso usare meno di 8 climatizzatori?

**R**: Sì! Se non hai tutte le zone, puoi:
- Selezionare lo stesso climatizzatore per più zone
- Creare entità `climate.dummy_*` che non fanno nulla
- Meglio ancora: crea una tua versione del blueprint eliminando le zone non necessarie

---

### D: Posso aggiungere altre zone climatiche?

**R**: Sì, devi modificare il blueprint aggiungendo:
```yaml
# Nella sezione input:
climate_zona_nuova:
  name: Climatizzatore Zona Nuova
  selector:
    entity:
      domain: climate

# Nelle action, aggiungi climate.a_c_zona_nuova alle liste entity_id
```

---

### D: Come integro questo con un generatore o automazione di rete?

**R**: Il blueprint è progettato per coordinarsi con altre automazioni. Ecco come:

**Scenario 1: Hai un generatore che parte automaticamente**
```yaml
# Imposta battery_low_threshold uguale alla soglia di avvio del generatore
battery_low_threshold: 50%  # Se il generatore parte al 50%
```

Quando il SOC scende sotto questa soglia:
1. Questo blueprint spegne tutti i climatizzatori
2. Invia notifica "Controllo trasferito ad automazione RETE"
3. La tua automazione generatore/rete può prendere il controllo

**Scenario 2: Automazione separata per gestione da rete**
```yaml
# Automazione 1: Questo blueprint (SOLARE)
# Condizione: SOC > 50%
condition:
  - condition: numeric_state
    entity_id: sensor.deye_battery
    above: 50

# Automazione 2: Gestione RETE/GENERATORE
trigger:
  - entity_id: sensor.deye_battery
    below: 50
    platform: numeric_state
action:
  # Gestisci climatizzatori con logica diversa
  # Es: solo zone essenziali, temperatura ridotta, ecc.
```

**Scenario 3: Coordinamento tramite input_select**
```yaml
# Helper per stato sistema
input_select:
  fonte_energia_riscaldamento:
    options:
      - "Solare"
      - "Rete"
      - "Generatore"

# Condizione nel blueprint
condition:
  - condition: state
    entity_id: input_select.fonte_energia_riscaldamento
    state: "Solare"

# Automazione che cambia stato
automation:
  - alias: "Cambio Fonte Energia"
    trigger:
      - entity_id: sensor.deye_battery
        below: 50
        platform: numeric_state
    action:
      - service: input_select.select_option
        target:
          entity_id: input_select.fonte_energia_riscaldamento
        data:
          option: "Rete"  # O "Generatore"
```

**Raccomandazione**: Usa `battery_low_threshold` come punto di coordinamento tra le tue automazioni solare e rete/generatore.

---

### D: Come cambio le temperature durante il giorno?

**R**: Due modi:

1. **Usa input_number** (consigliato):
   - Crea `input_number.temperatura_comfort_2`
   - Cambialo manualmente o con un'altra automazione

2. **Crea automazioni separate**:
   ```yaml
   automation:
     - alias: "Temperatura Giorno"
       trigger:
         - platform: time
           at: "08:00:00"
       action:
         - service: input_number.set_value
           target:
             entity_id: input_number.temperatura_comfort_2
           data:
             value: 22
     
     - alias: "Temperatura Notte"
       trigger:
         - platform: time
           at: "22:00:00"
       action:
         - service: input_number.set_value
           target:
             entity_id: input_number.temperatura_comfort_2
           data:
             value: 20
   ```

---

### D: Posso disabilitare alcune zone manualmente?

**R**: Sì, ma non tramite il blueprint. Crea un'automazione separata:
```yaml
automation:
  - alias: "Disabilita Studio Manualmente"
    trigger:
      - platform: state
        entity_id: input_boolean.disabilita_studio
        to: "on"
    action:
      - service: climate.turn_off
        target:
          entity_id: climate.a_c_studio
```

---

### D: Come integro questo con Alexa/Google Home?

**R**: L'integrazione vocale controlla solo `input_boolean.attivazione_riscaldamento`:

```yaml
# Routine Alexa: "Alexa, attiva riscaldamento automatico"
# Chiamata servizio:
service: input_boolean.turn_on
entity_id: input_boolean.attivazione_riscaldamento

# Routine Google: "Ok Google, riscaldamento automatico"
# Comando: "Attiva Attivazione Riscaldamento Automatico"
```

---

### D: Posso avere notifiche diverse per Telegram e App?

**R**: Il blueprint supporta un solo servizio. Per notifiche multiple:

1. Crea un gruppo di notifiche:
   ```yaml
   # configuration.yaml
   notify:
     - name: riscaldamento_tutti
       platform: group
       services:
         - service: mobile_app_iphone
         - service: telegram
   ```

2. Usa `notify.riscaldamento_tutti` nel blueprint

---

### D: Come visualizzo lo stato dell'automazione in Lovelace?

**R**: Esempio di card:

```yaml
type: entities
title: Stato Riscaldamento Solare
entities:
  - entity: input_boolean.attivazione_riscaldamento
    name: Automazione Attiva
  - entity: sensor.deye_battery
    name: Batteria
  - entity: sensor.power_production_now
    name: Produzione Solare
  - entity: sensor.deye_load_power
    name: Carico Casa
  - type: attribute
    entity: sensor.deye_battery
    attribute: last_changed
    name: Ultimo Aggiornamento
  - entity: input_text.ultima_notifica_riscaldamento
    name: Ultimo Evento
  - entity: sensor.temperature_sensor
    name: Temperatura Esterna
  - entity: sensor.a_c_sala_room_temperature
    name: Temperatura Sala
  - entity: input_number.temperatura_comfort_2
    name: Target Comfort
```

---

### D: Come faccio il backup della configurazione?

**R**: Le automazioni create dai blueprint sono salvate in:
```
/config/.storage/automations
```

Per backup completo:
1. Impostazioni → Sistema → Backup
2. Crea backup completo
3. Scarica il file .tar

Oppure usa un'integrazione Git per backup automatico del file `automations.yaml`.

---

### D: L'automazione va in conflitto con altre automazioni?

**R**: Possibili conflitti:

1. **Altre automazioni che controllano gli stessi climatizzatori**:
   - Disabilita o rimuovi le vecchie automazioni
   - Oppure aggiungi condizioni per evitare sovrapposizioni

2. **Mode single impedisce esecuzioni multiple**:
   - È intenzionale per evitare race conditions
   - Se serve, cambia a `mode: restart` nell'automazione generata

---

### D: Quanto consuma in termini di risorse HA?

**R**: Molto poco:
- Trigger basati su eventi e soglie (non polling)
- Una sola automazione attiva
- Calcoli molto semplici
- Nessun impatto percettibile anche su Raspberry Pi 3

---

### D: Posso usarlo anche per il raffrescamento estivo?

**R**: Sì, ma richiede modifiche al blueprint:
- Cambia `hvac_mode: heat` in `hvac_mode: cool`
- Inverti le logiche di temperatura
- Rinomina in "Gestione Climatizzatori Estate"

Oppure crea una seconda automazione dal blueprint modificato per l'estate.

---

## 📞 Supporto e Contributi

### Segnalazione Bug
Se trovi un bug, segnalalo su GitHub con:
- Versione Home Assistant
- Log dell'errore
- Configurazione (senza dati sensibili)

### Miglioramenti
Pull request benvenute per:
- Ottimizzazioni logiche
- Nuove funzionalità
- Documentazione

### Community
Discuti su:
- Forum Home Assistant italiano
- Telegram Home Assistant Italia
- Discord Home Assistant

---

## 📄 Licenza

MIT License - Uso libero con attribuzione

---

## 🙏 Ringraziamenti

Grazie alla community Home Assistant per il supporto e le idee che hanno permesso lo sviluppo di questo blueprint.

---

**Versione**: 1.0.0  
**Data**: Novembre 2024  
**Autore**: Franco (Head of Testing, Solar Automation Enthusiast)
