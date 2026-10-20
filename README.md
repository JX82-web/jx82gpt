# 🚀 jx82mygpt - Cybersecurity AI Agent CLI

Benvenuto in **jx82mygpt**, un assistente avanzato per la sicurezza informatica basato su linea di comando (CLI) sviluppato in **Go**. Lo strumento emula l'interfaccia nativa di `tgpt`, offre un'interazione in streaming in tempo reale e si interfaccia direttamente con i modelli locali tramite **Ollama**. 

Grazie al parser strutturato integrato, l'applicazione è in grado di estrarre la sintassi dei comandi generati dall'AI, richiedere un'autorizzazione interattiva `(y/N)` e **lanciare fisicamente i comandi sulla shell reale di Kali Linux**, inviando l'output del terminale direttamente all'interno della chat.

---

## 📌 Indice
1. [Gestione dei Modelli TRAMITE Ollama (Installazione e Disinstallazione)](#1-gestione-dei-modelli-tramite-ollama)
2. [Compilazione e Utilizzo di jxjpt.go (`./jx82mygpt`)](#2-compilazione-e-utilizzo-di-jxjptgo)
3. [⚠️ Logica Operativa dell'Agente (Bypass dei Rifiuti)](#3-logica-operativa-dellagente-bypass-dei-rifiuti)

---

## 1. Gestione dei Modelli TRAMITE Ollama

La CLI di Ollama consente di scaricare, arrestare e rimuovere i modelli linguistici localmente sul tuo sistema in modo immediato. Ogni modello di grandi dimensioni (come il 32B) occupa circa 19 GB di spazio su disco.

### 📥 Installazione di un Modello tramite Ollama
Per scaricare e aggiungere un modello al tuo catalogo locale (operazione di *pulling*), apri il terminale di Kali ed esegui il comando `run` o `pull`.

```bash
# Sincronizza ed esegui il re della precisione logica sul codice (32B - Consigliato)
ollama run qwen2.5-coder:32b

# Sincronizza ed esegui la variante intermedia più leggera (14B)
ollama run qwen2.5-coder:14b

# Sincronizza ed esegui il modello cybersecurity senza censure (7B)
ollama run whiterabbitneo:7b
```
*Nota: Il comando `run` verifica se il modello è presente sul disco; se non esiste, avvia automaticamente il download dei layer prima di caricare la chat.*

### 🛑 Arrestare un Modello per liberare la RAM/VRAM
I modelli di grandi dimensioni rimangono memorizzati nella memoria della scheda video (VRAM) e nella RAM per velocizzare le risposte successive. Se hai bisogno di liberare istantaneamente la memoria per altre attività o per caricare un'altra AI, ordina a Ollama di scaricarlo:
```bash
ollama stop qwen2.5-coder:32b
```

### 🗑️ Disinstallazione e Rimozione definitiva di un Modello
Se vuoi cancellare definitivamente i pesi di un modello dal tuo hard disk per recuperare spazio, utilizza il comando `rm`:
```bash
ollama rm qwen2.5-coder:32b
```

### 🔍 Verificare i Modelli attualmente installati
Per vedere la lista completa dei modelli residenti sul tuo computer e pronti all'uso con l'Agente, digita:
```bash
ollama list
```

---

## 2. Compilazione e Utilizzo di jxjpt.go

L'infrastruttura del client è centralizzata nel file di codice sorgente principale **`jxjpt.go`**. Di seguito la procedura per compilarlo nel binario finale chiamato **`jx82mygpt`**.

### 🛠️ Compilazione del Progetto:
```bash
# Inizializza il modulo di progetto in Go
go mod init jx82mygpt 2>/dev/null || true

# Compila il codice sorgente generando l'eseguibile statico unico chiamato jx82mygpt
go build -o jx82mygpt jxjpt.go
```

### 🎯 Sintassi dei Comandi (Identica a TGPT):

#### 1. Modalità Shell Interattiva Continua (Consigliata)
Avvia la sessione interattiva nel terminale. Per impostazione predefinita, utilizzerà il modello attivo o quello configurato nel codice.
```bash
./jx82mygpt -i
```

#### 2. Forza un Modello Specifico all'avvio
Se vuoi forzare l'Agente a utilizzare l'AI super-precisa da 32B (o qualsiasi altra installata nel catalogo di Ollama) senza modificare il codice:
```bash
./jx82mygpt -m "qwen2.5-coder:32b" -i
```

#### 3. Modalità Comando Diretto (Senza Flag)
Invia una domanda o istruzione singola direttamente come argomento testuale per un'operazione rapida "colpisci e fuggi":
```bash
./jx82mygpt "Mostrami solo il comando pulito in un blocco di codice per fare nmap su 127.0.0.1"
```

#### 4. Connessione all'interfaccia HexStrike
Se utilizzi il server in background di HexStrike sulla porta 8080, mappa l'indirizzamento per l'handshake iniziale di salute:
```bash
./jx82mygpt -s 127.0.0.1:8080 -i
```

---

## 3. ⚠️ Logica Operativa dell'Agente (Bypass dei Rifiuti)

I modelli di intelligenza artificiale, anche quelli locali, tendono ad attivare filtri di sicurezza residui se istruiti con comandi diretti e imperativi (es. *"esegui"*, *"attacca"*, *"fai l'exploit"*), rifiutandosi di rispondere per motivi di sicurezza. 

Per sfruttare la massima potenza dell'Agente e innescare l'esecuzione fisica reale su Kali o sul tuo container Docker, segui questa strategia psicologica di prompt:

1. **Usa parole chiave informative:** Interroga sempre la shell partendo con espressioni come:
   > `mytgpt ❯ Mostrami solo il comando pulito in un blocco di codice per...`
2. **Generazione del blocco:** L'AI rimarrà nel suo ruolo di assistente teorico e genererà la sintassi corretta all'interno del riquadro protetto `[SINTASSI DETTAGLIATA]`.
3. **Intercettazione e Controllo Totale:** Il client Go intercetterà immediatamente il testo del blocco e bloccherà il terminale mostrandoti la richiesta di autorizzazione:
   > `[?] AUTORIZZAZIONE RICHIESTA... Eseguire fisicamente su Kali Linux? (y/N):`
4. **Esecuzione:** Premendo **`y`** (o *yes*), il comando verrà iniettato direttamente nella shell ed eseguito fisicamente su Kali o nel container Docker, mostrandoti i risultati reali all'interno della chat stessa. Premendo **`n`**, l'azione verrà annullata in totale sicurezza.
