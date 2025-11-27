# 📦 PACCHETTO COMPLETO PER PUBBLICAZIONE GITHUB

## 🎯 Repository Target
**GitHub Username**: francosax  
**Repository Name**: ha-climate-battery-management  
**URL Finale**: https://github.com/francosax/ha-climate-battery-management

---

## 📁 File da Scaricare e Pubblicare

### 1. File Principali (OBBLIGATORI)
- [climate_battery_management_blueprint.yaml](computer:///mnt/user-data/outputs/climate_battery_management_blueprint.yaml) - **30KB** - Il blueprint
- [README.md](computer:///mnt/user-data/outputs/README.md) - **8KB** - Pagina principale repository
- [ISTRUZIONI_BLUEPRINT_RISCALDAMENTO.md](computer:///mnt/user-data/outputs/ISTRUZIONI_BLUEPRINT_RISCALDAMENTO.md) - **29KB** - Documentazione completa

### 2. File di Configurazione (OBBLIGATORI)
- [LICENSE](computer:///mnt/user-data/outputs/LICENSE) - **1KB** - Licenza MIT
- [gitignore](computer:///mnt/user-data/outputs/gitignore) - **267B** - Da rinominare in `.gitignore`

### 3. File di Supporto (OPZIONALI)
- [GUIDA_PUBBLICAZIONE_GITHUB.md](computer:///mnt/user-data/outputs/GUIDA_PUBBLICAZIONE_GITHUB.md) - **8KB** - Guida passo-passo
- [publish_to_github.sh](computer:///mnt/user-data/outputs/publish_to_github.sh) - **3KB** - Script automatico

---

## 🚀 METODO RAPIDO: Interfaccia Web (5 minuti)

### Step 1: Crea Repository
1. Vai su https://github.com/francosax
2. Clicca **"New"** (repository)
3. Nome: `ha-climate-battery-management`
4. Descrizione: `Blueprint Home Assistant per gestione intelligente climatizzatori con batteria e surplus solare`
5. ✅ Public
6. ✅ Add MIT License
7. Clicca **"Create repository"**

### Step 2: Carica File
1. Clicca **"uploading an existing file"**
2. **Scarica tutti e 5 i file** dai link sopra
3. **Rinomina** `gitignore` in `.gitignore` (con il punto all'inizio)
4. **Trascina tutti i file** nella finestra GitHub
5. Commit message:
   ```
   Initial commit - Blueprint gestione climatizzatori
   
   - Blueprint completo con 6 livelli operativi
   - Gestione 8 zone climatiche
   - Documentazione completa in italiano
   - Esempi e troubleshooting
   ```
6. Clicca **"Commit changes"**

### Step 3: Configura (2 minuti)
1. **Settings** → Nella sidebar a destra, clicca ⚙️ su "About"
2. Topics: Aggiungi `home-assistant`, `blueprint`, `automation`, `climate`, `solar`, `battery`
3. **Salva**

### ✅ COMPLETATO!
Repository online: **https://github.com/francosax/ha-climate-battery-management**

---

## 💻 METODO ALTERNATIVO: Git da Terminale

### Prerequisiti
- Git installato
- Credenziali GitHub (con Personal Access Token se usi 2FA)

### Comandi Rapidi
```bash
# 1. Crea cartella e scarica i file
mkdir ha-climate-battery-management
cd ha-climate-battery-management

# 2. Scarica i file dai link sopra e mettili in questa cartella
# Rinomina gitignore in .gitignore

# 3. Esegui lo script automatico (scarica publish_to_github.sh dai link)
chmod +x publish_to_github.sh
./publish_to_github.sh

# Oppure manualmente:
git init
git add .
git commit -m "Initial commit - Blueprint gestione climatizzatori"
git branch -M main
git remote add origin https://github.com/francosax/ha-climate-battery-management.git
git push -u origin main
```

---

## 🔗 Link Utili Dopo la Pubblicazione

### Import Diretto in Home Assistant
```
https://github.com/francosax/ha-climate-battery-management/blob/main/climate_battery_management_blueprint.yaml
```

### Badge Import (da aggiungere al README se vuoi)
```markdown
[![Importa Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/francosax/ha-climate-battery-management/blob/main/climate_battery_management_blueprint.yaml)
```

---

## 📢 Condividi con la Community

### Forum Home Assistant Italia
```
Titolo: [Blueprint] Gestione Intelligente Climatizzatori con Batteria e Surplus Solare

Ciao a tutti! 👋

Ho sviluppato un blueprint per gestire automaticamente i climatizzatori 
in base allo stato della batteria e al surplus di produzione solare.

🔗 GitHub: https://github.com/francosax/ha-climate-battery-management

✨ Caratteristiche:
- 6 livelli operativi automatici (MAX/MEDIO/BASE/PROTEZIONE)
- Gestione 8 zone climatiche con priorità
- Protezione batteria integrata
- Notifiche dettagliate
- Completamente configurabile via UI
- Documentazione completa in italiano + esempi

Il blueprint include 4 esempi di configurazione per diversi scenari 
(alta produzione, batteria piccola, massimo risparmio, ecc.)

Feedback e suggerimenti sono benvenuti! 🙏
```

### Telegram HA Italia
```
🔥 Nuovo Blueprint: Gestione Climatizzatori con Batteria

Controlla automaticamente 8 zone climatiche in base a:
- SOC batteria (6 livelli: 90%/70%/50%)
- Surplus solare (4000W/2000W)
- Temperature interne/esterne

✅ Documentazione completa
✅ 4 esempi configurazione
✅ Troubleshooting + FAQ

https://github.com/francosax/ha-climate-battery-management
```

### Reddit r/homeassistant
```
Title: [Blueprint] Smart Climate Control with Battery & Solar Surplus Integration (Italian Docs)

I've created a comprehensive blueprint for managing multiple climate zones 
based on battery state of charge and solar production surplus.

Features:
- 6 automatic operational levels
- 8 independent climate zones with priorities
- Battery protection (auto-switch to grid at configurable threshold)
- Fully configurable via UI
- Complete documentation in Italian + troubleshooting

GitHub: https://github.com/francosax/ha-climate-battery-management

The blueprint manages heating/cooling for 8 zones (living areas, bedrooms, 
guest rooms) with automatic priority management based on available energy.

Feedback welcome!
```

---

## ✅ Checklist Pre-Pubblicazione

Prima di condividere, verifica:

- [ ] Repository creato su GitHub (https://github.com/francosax/ha-climate-battery-management)
- [ ] Tutti i 5 file caricati
- [ ] `.gitignore` con il punto (non `gitignore`)
- [ ] README.md visualizzato correttamente nella home
- [ ] LICENSE MIT presente
- [ ] Topics aggiunti (home-assistant, blueprint, automation, climate, solar, battery)
- [ ] Descrizione repository impostata
- [ ] Repository è pubblico
- [ ] URL blueprint testato: https://github.com/francosax/ha-climate-battery-management/blob/main/climate_battery_management_blueprint.yaml

---

## 📊 Statistiche File

| File | Dimensione | Tipo | Note |
|------|------------|------|------|
| climate_battery_management_blueprint.yaml | 30 KB | Blueprint | File principale |
| README.md | 8 KB | Markdown | Home repository |
| ISTRUZIONI_BLUEPRINT_RISCALDAMENTO.md | 29 KB | Markdown | Docs completa |
| LICENSE | 1 KB | Text | MIT License |
| .gitignore | 267 B | Text | Git ignore |
| **TOTALE** | **~68 KB** | | Repository completo |

---

## 🎓 Per Approfondimenti

Consulta la guida completa:
- [GUIDA_PUBBLICAZIONE_GITHUB.md](computer:///mnt/user-data/outputs/GUIDA_PUBBLICAZIONE_GITHUB.md)

Include:
- Istruzioni dettagliate passo-passo
- Troubleshooting problemi comuni
- Come creare releases
- Come aggiungere screenshot
- Badge personalizzati

---

## 🆘 Serve Aiuto?

1. Leggi la GUIDA_PUBBLICAZIONE_GITHUB.md
2. Controlla GitHub Docs: https://docs.github.com
3. Chiedi alla community Home Assistant Italia

---

**Buona pubblicazione! 🚀**

_Tutti i file sono pronti e testati. Segui il METODO RAPIDO per pubblicare in 5 minuti!_
