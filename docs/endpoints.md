# 🔗 Endpoints

!!! info ""
    Gli **endpoint** rappresentano i punti di accesso offerti dall’API Server per interagire con il backend Open-RMF.  
    Sono progettati per esporre funzionalità in modo standardizzato, sicuro e scalabile sia tramite chiamate sincrone (REST) che aggiornamenti in tempo reale (WebSocket).

---

## 🧭 Tipologie di endpoint

- **REST API**
    - Permettono operazioni sincrone: invio di richieste e ricezione di una risposta immediata.
    - Ideali per operazioni di lettura, scrittura o modifica di risorse (es. flotte, task, robot, porte).

- **WebSocket / Event Streams**
    - Permettono una comunicazione asincrona e bidirezionale.
    - Utilizzate per ricevere aggiornamenti in tempo reale su stati di robot, task, eventi di sistema, ecc.

---

## 🗂️ Struttura degli endpoint REST

!!! abstract "Principali risorse REST"
    - **/fleets**: gestione e interrogazione delle flotte robotiche disponibili.
    - **/robots**: informazioni e stato dei singoli robot.
    - **/tasks**: creazione, aggiornamento e monitoraggio dei task.
    - **/doors**: stato e controllo delle porte automatizzate.
    - **/lifts**: gestione e monitoraggio degli ascensori.
    - **/users**: (se implementato) gestione utenti, ruoli, permessi.

---

## 🌐 Canali WebSocket/Event Streams

!!! abstract "Principali canali WebSocket"
    - **robot_states**: aggiornamenti in tempo reale sullo stato dei robot.
    - **task_states**: evoluzione e stato corrente dei task.
    - **door_states**: stato delle porte in tempo reale.
    - **lift_states**: stato degli ascensori.
    - **event_logs**: eventi e notifiche di sistema.

---

## 🏆 Ruolo degli endpoint nell’architettura

!!! tip ""
    Gli endpoint rappresentano il “contratto” tra il mondo esterno e il backend Open-RMF.  
    Permettono a dashboard, microservizi e sistemi esterni di eseguire operazioni, ricevere informazioni aggiornate e integrarsi in modo sicuro e standardizzato.

---

## 📋 Esempi di operazioni

### 🧭 Panoramica teorica
- Ottenere la lista delle flotte disponibili  
- Creare un nuovo task  
- Recuperare lo stato corrente di un robot  
- Modificare lo stato di una risorsa (es. apertura porta)
- 
--8<-- "docs/endpoints_detail.md"

---

## 🔒 Sicurezza degli endpoint

!!! warning ""
    Tutti gli endpoint sono protetti tramite autenticazione (es. JWT/OIDC) e autorizzazione basata su ruoli e permessi.  
    L’accesso a specifici endpoint o canali può essere limitato in base al profilo utente.

---

## ❓ Domande frequenti

!!! question "Posso estendere o personalizzare gli endpoint?"
    Sì, l’API Server può essere configurato o esteso per offrire endpoint aggiuntivi o personalizzati.

!!! question "Come vengono versionati gli endpoint?"
    È buona pratica includere la versione nell’URL (es. `/v1/fleets`), facilitando l’evoluzione senza rompere la retrocompatibilità.

!!! question "Come funzionano le sottoscrizioni agli eventi?"
    Di norma, il client si connette via WebSocket e si sottoscrive ai canali di interesse, ricevendo aggiornamenti dal server in tempo reale.

---