# 💡 Esempi di utilizzo

!!! info ""
    Questa sezione descrive **casi d’uso teorici** e flussi di interazione tipici tra client (dashboard, microservizi) e backend Open-RMF tramite l’API Server.  
    Gli esempi forniscono spunti concreti su come sfruttare le API REST e i canali WebSocket per la gestione di flotte, task, risorse e monitoraggio in tempo reale.

---

## 🚀 Esempi di casi d’uso

### 1. **Monitoraggio stato flotte e robot in tempo reale**
Un sistema di supervisione o una dashboard si collega all’API Server, si autentica e si sottoscrive ai canali WebSocket di stato (`robot_states`, `fleet_states`).  
In questo modo, l’operatore visualizza aggiornamenti continui su posizione, carica, stato operativo e anomalie di ogni robot o flotta.

---

### 2. **Creazione e supervisione di un nuovo task**
Un utente autorizzato accede alla dashboard, compila un form per assegnare un nuovo compito (es. trasporto di un oggetto).  
La richiesta viene inoltrata all’API Server tramite un endpoint REST (`/tasks`) e, una volta creato il task, il client si sottoscrive al canale `task_states` per seguirne l’avanzamento in tempo reale.

---

### 3. **Gestione di risorse (porte, ascensori)**
Attraverso endpoint REST dedicati, è possibile interrogare lo stato di una porta o di un ascensore, oppure inviare comandi di apertura/chiusura o chiamata.  
Il client monitora lo stato aggiornato della risorsa tramite i canali `door_states` o `lift_states` via WebSocket.

---

### 4. **Notifiche di eventi e allarmi**
Un microservizio o una dashboard può sottoscriversi al canale di eventi (`event_logs`) per ricevere notifiche istantanee su anomalie, allarmi di sicurezza, completamento task, interventi manuali o problemi di sistema.

---

### 5. **Gestione multi-utente e permessi**
L’API Server consente a più utenti, ognuno con ruoli e permessi diversi, di accedere alle stesse API.  
Ad esempio, un “operatore” può creare e monitorare task, mentre un “amministratore” può anche gestire flotte, risorse e configurazioni avanzate.

---

## 🔄 Flusso generale di interazione

!!! abstract "Esempio di flusso tipico"
    1. **Autenticazione**: Il client ottiene un token da un provider OIDC e lo invia all’API Server.
    2. **Richiesta REST**: Il client effettua una richiesta (ad es. creazione task, interrogazione stato robot).
    3. **Sottoscrizione WebSocket**: Il client si collega e si sottoscrive ai canali di interesse per ricevere aggiornamenti in tempo reale.
    4. **Gestione risposte e aggiornamenti**: Il client aggiorna la UI o i dati interni in base alle risposte e agli eventi ricevuti.

---

## 🏆 Benefici di questi flussi

!!! success ""
    - **Esperienza utente migliorata**: aggiornamenti in tempo reale e interfacce reattive.
    - **Automazione**: possibilità di integrare sistemi esterni e microservizi per gestire flotte e task senza intervento manuale.
    - **Sicurezza**: ogni interazione è soggetta a controlli di autenticazione e autorizzazione.
    - **Scalabilità**: il modello è adatto sia a piccoli impianti che a grandi installazioni multi-flotta.

---

## ❓ Domande frequenti

!!! question "Posso combinare più canali WebSocket?"
    Sì, un client può sottoscriversi a più canali contemporaneamente per ottenere una panoramica completa e aggiornata del sistema.

!!! question "Gli esempi sono validi sia in simulazione che in produzione?"
    Assolutamente sì: l’architettura Open-RMF consente di usare gli stessi flussi sia su robot reali che in ambienti simulati.

!!! question "Questi esempi coprono tutti i casi d’uso?"
    Sono solo spunti: l’API Server è pensato per essere esteso e adattato a molti scenari diversi.

---