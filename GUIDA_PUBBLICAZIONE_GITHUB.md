# 📘 Guida Pubblicazione su GitHub

## 🎯 Obiettivo
Pubblicare il blueprint "Gestione Climatizzatori con Batteria" sul tuo account GitHub (francosax)

---

## 📋 File da Pubblicare

Hai bisogno di questi 5 file (tutti già creati):

1. ✅ `climate_battery_management_blueprint.yaml` - Il blueprint
2. ✅ `README.md` - Pagina principale del repository
3. ✅ `ISTRUZIONI_BLUEPRINT_RISCALDAMENTO.md` - Documentazione completa
4. ✅ `LICENSE` - Licenza MIT
5. ✅ `.gitignore` - File da ignorare in Git

---

## 🚀 Metodo 1: Interfaccia Web GitHub (CONSIGLIATO - 5 minuti)

### Passo 1: Crea il Repository

1. Vai su: **https://github.com/francosax**
2. Clicca sul pulsante verde **"New"** (o "+ New repository")
3. Compila:
   - **Repository name**: `ha-climate-battery-management`
   - **Description**: `Blueprint Home Assistant per gestione intelligente climatizzatori con batteria e surplus solare`
   - **Visibility**: ✅ Public (per condividere con la community)
   - **Initialize**: 
     - ❌ NO README (ne hai già uno)
     - ✅ Add .gitignore: None (ne hai già uno)
     - ✅ Choose a license: MIT
4. Clicca **"Create repository"**

### Passo 2: Carica i File

1. Nella pagina del repository vuoto, clicca **"uploading an existing file"**
2. **Scarica i 5 file** da questi link:
   - [climate_battery_management_blueprint.yaml](computer:///mnt/user-data/outputs/climate_battery_management_blueprint.yaml)
   - [README.md](computer:///mnt/user-data/outputs/README.md)
   - [ISTRUZIONI_BLUEPRINT_RISCALDAMENTO.md](computer:///mnt/user-data/outputs/ISTRUZIONI_BLUEPRINT_RISCALDAMENTO.md)
   - [LICENSE](computer:///home/claude/LICENSE)
   - [.gitignore](computer:///home/claude/.gitignore)

3. **Trascina tutti i 5 file** nella finestra di upload
4. Nella casella di commit scrivi:
   ```
   Initial commit - Blueprint gestione climatizzatori
   
   - Blueprint completo con 6 livelli operativi
   - Gestione 8 zone climatiche  
   - Documentazione completa in italiano
   - Esempi configurazione e troubleshooting
   ```
5. Clicca **"Commit changes"**

### Passo 3: Configura il Repository

1. Vai su **Settings** (ingranaggio in alto)
2. Nella sezione **About** (a destra):
   - Clicca sull'ingranaggio ⚙️
   - Aggiungi descrizione: `Blueprint Home Assistant per gestione intelligente climatizzatori con batteria e surplus solare`
   - Website: Lascia vuoto o metti il tuo sito
   - Topics: `home-assistant`, `blueprint`, `automation`, `climate`, `solar`, `battery`, `hvac`
   - ✅ Salva

3. Opzionale - Abilita Issues:
   - Settings → Features → ✅ Issues

### ✅ FATTO! 

Il tuo repository è online: **https://github.com/francosax/ha-climate-battery-management**

---

## 💻 Metodo 2: Riga di Comando Git (Per Utenti Avanzati)

### Prerequisiti
- Git installato sul tuo computer
- Account GitHub configurato

### Passo 1: Crea il Repository su GitHub
Come nel Metodo 1, ma NON inizializzare con README/License

### Passo 2: Prepara i File
```bash
# Crea una cartella per il progetto
mkdir ha-climate-battery-management
cd ha-climate-battery-management

# Scarica i file nella cartella oppure copialli manualmente
```

### Passo 3: Esegui lo Script di Pubblicazione

Ho preparato uno script automatico per te! Scaricalo qui:
- [publish_to_github.sh](computer:///home/claude/publish_to_github.sh)

```bash
# Rendi lo script eseguibile
chmod +x publish_to_github.sh

# Esegui lo script
./publish_to_github.sh
```

Lo script farà automaticamente:
1. ✅ Verifica presenza file
2. ✅ Inizializza repository Git
3. ✅ Crea commit con messaggio descrittivo
4. ✅ Configura remote GitHub
5. ✅ Push su GitHub

Ti chiederà le credenziali GitHub quando fai il push.

**⚠️ IMPORTANTE**: Se usi autenticazione a 2 fattori su GitHub:
- NON usare la password normale
- Crea un **Personal Access Token**: 
  - GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
  - Generate new token → Seleziona `repo` scope
  - Copia il token e usalo come password

---

## 🎨 Passo 4: Personalizza (Opzionale)

### Aggiungi Screenshots
1. Crea una cartella `images/` nel repository
2. Aggiungi screenshot del blueprint in azione
3. Nel README.md, aggiungi:
   ```markdown
   ## 📸 Screenshots
   ![Dashboard](images/dashboard.png)
   ```

### Aggiungi Badge
Nel README.md, sopra il titolo, aggiungi badge personalizzati:
```markdown
[![GitHub release](https://img.shields.io/github/v/release/francosax/ha-climate-battery-management)](https://github.com/francosax/ha-climate-battery-management/releases)
[![GitHub stars](https://img.shields.io/github/stars/francosax/ha-climate-battery-management)](https://github.com/francosax/ha-climate-battery-management/stargazers)
```

### Crea un Release
1. Vai su **Releases** → **Create a new release**
2. Tag version: `v1.0.0`
3. Release title: `v1.0.0 - Initial Release`
4. Descrizione:
   ```markdown
   🎉 Prima release del blueprint!
   
   ## Funzionalità
   - ✅ 6 livelli gestione automatica
   - ✅ 8 zone climatiche
   - ✅ Integrazione batteria e surplus solare
   - ✅ Documentazione completa in italiano
   ```
5. Allega i file principali
6. Clicca **"Publish release"**

---

## 🔗 Link Utili Dopo la Pubblicazione

### URL Import Diretto per Home Assistant
```
https://github.com/francosax/ha-climate-battery-management/blob/main/climate_battery_management_blueprint.yaml
```

Gli utenti potranno importare direttamente con questo badge nel README:
```markdown
[![Importa Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/francosax/ha-climate-battery-management/blob/main/climate_battery_management_blueprint.yaml)
```

### Condividi con la Community

**Forum Home Assistant Italia**:
```
Ciao a tutti! 👋

Ho creato un blueprint per gestire i climatizzatori in modo intelligente 
usando batteria e surplus solare.

🔗 Repository: https://github.com/francosax/ha-climate-battery-management

Funzionalità:
- 6 livelli operativi automatici
- Gestione 8 zone climatiche
- Protezione batteria
- Documentazione completa in italiano

Feedback e suggerimenti sono benvenuti! 🙏
```

**Telegram Home Assistant Italia**:
```
🔥 Nuovo Blueprint: Gestione Climatizzatori con Batteria

Gestisce automaticamente i climatizzatori in base a SOC batteria 
e surplus solare, con 6 livelli operativi e 8 zone.

https://github.com/francosax/ha-climate-battery-management

Documentazione completa + esempi configurazione inclusi!
```

**Reddit r/homeassistant**:
```
Title: [Blueprint] Smart Climate Control with Battery & Solar Integration

I've created a blueprint for intelligent climate control based on 
battery SOC and solar surplus...

[Link al repository]
```

---

## ✅ Checklist Finale

Prima di condividere, verifica:

- [ ] Repository pubblico su GitHub
- [ ] README.md visualizzato correttamente
- [ ] Tutti i 5 file presenti
- [ ] Topics aggiunti (home-assistant, blueprint, etc.)
- [ ] Descrizione repository impostata
- [ ] License MIT visualizzata
- [ ] Issues abilitati (opzionale)
- [ ] URL import testato

---

## 🆘 Problemi Comuni

### "Repository not found"
- Verifica di aver creato il repository su GitHub
- Controlla che il nome sia esattamente `ha-climate-battery-management`
- Assicurati che sia pubblico

### "Authentication failed"
- Se usi 2FA: crea un Personal Access Token
- Verifica username: `francosax`
- Il token va usato al posto della password

### "File too large"
- I file del blueprint sono piccoli, non dovresti avere questo problema
- Se succede: verifica di non aver incluso file extra (.log, .db, etc.)

### "Push rejected"
- Il repository non è vuoto? Usa `git pull` prima di `git push`
- Oppure usa `git push -f` per forzare (ATTENZIONE: sovrascrive tutto)

---

## 📞 Supporto

Se hai problemi con la pubblicazione:
1. Controlla questa guida
2. Vedi la documentazione GitHub: https://docs.github.com
3. Chiedi aiuto alla community Home Assistant Italia

---

**Buona pubblicazione! 🚀**
