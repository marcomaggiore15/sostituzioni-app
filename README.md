# sostituzio# 🏫 Gestore Sostituzioni Docenti - Smart Allocator

Una moderna applicazione web basata su React progettata per semplificare, ottimizzare e tracciare le sostituzioni dei docenti assenti all'interno di un istituto scolastico.

L'algoritmo intelligente assegna le supplenze basandosi su priorità specifiche (Ore a Disposizione, Ore di Potenziamento, Docenti di Sostegno già presenti in classe) e bilancia il carico di lavoro per garantire equità.

## ✨ Funzionalità Principali

* **🧠 Algoritmo Intelligente di Assegnazione:**

  * Cerca il sostituto ideale con questa priorità:

    1. Docenti con ora a **Disposizione (D)**.

    2. Docenti con ora di **Potenziamento (P)**.

    3. Docenti di **Sostegno** assegnati a quella specifica classe.

  * A parità di priorità, l'algoritmo sceglie il docente che ha effettuato *meno sostituzioni* in passato (bilanciamento del carico).

* **🛠️ Assegnazione Straordinaria (Manuale):**

  * Possibilità di bypassare l'algoritmo in caso di emergenza, scegliendo qualsiasi docente in servizio (mostrando dove si trova attualmente) e inserendo una motivazione.

* **📅 Gestione Orario a Matrice Completa:**

  * **Docenti Curricolari:** Inserimento e modifica di classi (es. "3A"), ore a Disposizione ("D") e Potenziamento ("P").

  * **Docenti di Sostegno:** Inserimento delle classi in cui il docente presta servizio.

* **👥 Anagrafica Dinamica:**

  * Aggiunta, rimozione e rinomina dei docenti. Le modifiche al nome si riflettono automaticamente su orari e storico salvato.

* **📁 Importazione ed Esportazione Avanzata:**

  * **Importa Excel/CSV:** Carica interi orari da file Excel (tramite libreria `SheetJS`).

  * **Backup e Trasferimento (JSON):** Esporta l'intero database in un file per trasferire il lavoro da un PC all'altro in assenza di cloud.

* **🖨️ Registro e Reportistica:**

  * Storico completo delle sostituzioni.

  * Esportazione nativa in **PDF** (tramite `jsPDF` e `html2canvas`) e in **CSV/Excel**.

  * Stampa diretta formattata in bianco e nero.

* **📊 Statistiche (Equità):**

  * Monitoraggio in tempo reale del numero di sostituzioni effettuate da ciascun docente.

* **☁️ Salvataggio Ibrido (Cloud / Local):**

  * Funziona in locale (`localStorage`) se esportato staticamente su Vercel.

  * Supporta la **Sincronizzazione Globale Pubblica** tramite Firebase Firestore, protetta da **Password di Salvataggio** ("admin" di default).

## 💻 Tecnologie Utilizzate

* **Frontend Framework:** React.js (con Hooks: `useState`, `useEffect`, `useMemo`)

* **Styling:** Tailwind CSS (utility-first CSS per un design moderno e responsive)

* **Iconografia:** Lucide React

* **Database e Sync:** Firebase (Authentication Anonymous, Firestore Database)

* **Parsing Excel:** SheetJS (`xlsx`) caricato via CDN

* **Generazione PDF:** `jsPDF` e `html2canvas` caricati via CDN

## 📂 Struttura del Progetto

Essendo il progetto ottimizzato per essere un'applicazione web leggera e facilmente esportabile, il core dell'applicativo è racchiuso all'interno di un componente principale.

Ecco la suddivisione logica e la struttura dei file:

```
/
├── public/                 # Asset statici (favicon, index.html)
├── src/
│   ├── App.jsx             # ✨ CORE APP: Contiene logica, stato, database e UI
│   ├── index.js            # Entry point di React
│   └── index.css           # Importazione Tailwind CSS
├── package.json            # Dipendenze del progetto
└── README.md               # Documentazione

```

### 🧠 Anatomia del file `App.jsx`

Il file `App.jsx` è modulato in sezioni logiche per una facile manutenzione:

1. **Imports & Helpers:** Caricamento librerie esterne in modo dinamico per alleggerire il bundle iniziale.

2. **Dataset Iniziali:** Costanti per i dati di fallback (`INITIAL_MASTER_SCHEDULE`, ecc.) usate durante il primo avvio.

3. **Configurazione Firebase:** Setup delle chiavi Cloud e inizializzazione di Firestore.

4. **State Management (Hooks):** Definizione delle variabili di stato (`activeTab`, `masterSchedule`, `replacementsHistory`, ecc.).

5. **Logica di Salvataggio e Sincronizzazione:** Gli `useEffect` che gestiscono la lettura e scrittura su `localStorage` o su `Firestore`.

6. **Funzioni Core:** `handleCalculateReplacement` (Algoritmo), `handleFileUpload` (Parser Excel), `handleDownloadPDF`, ecc.

7. **Render (UI):** Navigazione a tab e rendering condizionale delle diverse sezioni:

   * *Tab 1 (assegna)*

   * *Tab 2 (modificaDP)*

   * *Tab 3 (sostegno)*

   * *Tab 4 (anagrafica)*

   * *Tab 5 (importa)*

   * *Tab 6 (registro)*

   * *Tab 7 (equita)*

8. **Modali:** Componenti sovrapposti per Salvataggio con Password, Reset, e Rinomina.

## 🚀 Guida al Deploy e Configurazione Cloud (Vercel + Firebase)

Se pubblichi il sito su Vercel senza configurare Firebase, l'app salverà i dati solo sul tuo browser (LocalStorage).
Per avere una **sincronizzazione in tempo reale su tutti i dispositivi**, segui questi passi:

### 1. Configura Firebase

1. Vai su [Firebase Console](https://console.firebase.google.com/) e crea un progetto.

2. Vai su **Authentication** -> Metodi di accesso -> Abilita **Anonimo** (Guest).

3. Vai su **Firestore Database** -> Crea Database (inizia in *Modalità di test*).

4. Vai nelle impostazioni di progetto (icona ingranaggio) e registra un'app Web `</>`.

5. Copia le chiavi fornite all'interno della variabile `firebaseConfig`.

### 2. Inserisci le chiavi nel codice

Apri il file `App.jsx` e cerca la costante `firebaseConfig` (attorno alla riga 80). Sostituisci i valori:

```
const firebaseConfig = {
  apiKey: "LA_TUA_API_KEY",
  authDomain: "IL_TUO_PROJECT_ID.firebaseapp.com",
  projectId: "IL_TUO_PROJECT_ID",
  storageBucket: "IL_TUO_PROJECT_ID.appspot.com",
  messagingSenderId: "123456789",
  appId: "IL_TUO_APP_ID"
};

```

### 3. Deploy su Vercel

1. Carica il codice su GitHub.

2. Collega la repository al tuo account Vercel.

3. Clicca su **Deploy**.

4. Fatto! L'app sarà live. Le modifiche fatte da PC saranno visibili immediatamente su smartphone e viceversa.

## 🔒 Sicurezza e Salvataggio

L'applicazione è dotata di un sistema di salvataggio manuale e protetto.

* Quando apporti una modifica, il pulsante in alto a destra si illuminerà (Salva Modifiche \*\*\*).

* Cliccandolo, verrà richiesta una password.

* La password predefinita alla prima installazione è **`admin`**.

* Puoi modificare la password in qualsiasi momento cliccando su "Vuoi cambiare la password?" all'interno del modale di salvataggio.

*Sviluppato per istituti d'istruzione secondaria e segreterie scolastiche.*ni-app
