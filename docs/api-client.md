# 🧑‍💻 API Client

!!! info ""
    L’**API Client** è una componente concettuale fondamentale per l’integrazione tra applicazioni esterne (come dashboard, microservizi, sistemi di orchestrazione) e l’API Server del backend Open-RMF.  
    Il suo scopo è semplificare e standardizzare le interazioni, garantendo coerenza, sicurezza e facilità di evoluzione del sistema.

---

## 🎯 Scopo dell’API Client

L’API Client nasce per:

- **Fornire un’interfaccia chiara** e documentata che rappresenti tutte le funzionalità esposte dall’API Server.
- **Centralizzare la logica di comunicazione** verso il backend, evitando duplicazioni e facilitando la manutenzione.
- **Favorire l’integrabilità** di dashboard, microservizi e strumenti di terze parti con Open-RMF in modo coerente e scalabile.
- **Standardizzare la gestione di autenticazione, autorizzazione, errori e aggiornamenti in tempo reale**.

---

## 🧩 Caratteristiche e responsabilità

!!! abstract "Punti chiave"
    - **Astrazione delle API:** l’API Client espone metodi (o endpoint) che rappresentano le operazioni di business disponibili sul sistema (es. gestione flotte, task, risorse).
    - **Gestione delle chiamate asincrone e real-time:** supporta sia operazioni sincrone (es. REST) che aggiornamenti/eventi asincroni (es. WebSocket).
    - **Sicurezza:** centralizza la gestione di autenticazione (es. JWT/OIDC), autorizzazione e rinnovo token.
    - **Tipizzazione e validazione:** definisce i contratti dati tra frontend/microservizi e backend, riducendo il rischio di errori.
    - **Unificazione degli errori:** fornisce una gestione centralizzata degli errori e delle eccezioni di comunicazione.

---

## 🏆 Vantaggi dell’API Client

!!! success ""
    - **Manutenibilità:** aggiornare la logica di interazione con il backend richiede modifiche solo nell’API Client, non su tutti i consumatori.
    - **Sicurezza:** riduce il rischio di errori di autenticazione/autorizzazione grazie a una gestione centralizzata.
    - **Scalabilità:** semplifica l’integrazione di nuovi frontend, microservizi e strumenti grazie a un’interfaccia unica e documentata.
    - **Flessibilità:** l’API Client può essere adattato o esteso per supportare nuovi casi d’uso o evoluzioni delle API backend.

---

## 🌐 Ruolo nell'architettura

!!! tip ""
    L’API Client si colloca come ponte tra i consumatori (frontend, microservizi, sistemi esterni) e l’API Server.  
    Rappresenta il punto di ingresso unico e controllato verso tutte le funzionalità di Open-RMF esposte tramite API.

---

## 🔒 Sicurezza e conformità

L’API Client, essendo responsabile della comunicazione con il backend, è il luogo ideale dove implementare:

- **Gestione delle credenziali e dei token di accesso**
- **Refresh automatico dei token scaduti**
- **Verifiche sui permessi degli utenti**
- **Gestione centralizzata delle risposte di errore e delle eccezioni di sicurezza**

---