# 🖧 API Server

!!! info ""
    L’**API Server** è il cuore del backend Open-RMF e rappresenta il punto di contatto tra il mondo web (frontend, dashboard, microservizi) e il core Open-RMF (basato su ROS 2).  
    Svolge il ruolo di gateway, orchestrando la comunicazione, la sicurezza e l’esposizione delle funzionalità di alto livello.

---

## 🎯 Scopo dell’API Server

L’API Server nasce per:

- **Esporre funzionalità RMF tramite API standard** (REST e WebSocket), accessibili da dashboard, servizi esterni e client di vario tipo.
- **Tradurre le richieste web** (HTTP/WebSocket) in comunicazioni verso il core Open-RMF, che utilizza ROS 2 come middleware.
- **Gestire autenticazione, autorizzazione e sicurezza** di tutte le richieste.
- **Fornire aggiornamenti in tempo reale** su eventi e stati di robot, task, risorse e sistema.

---

## 🧩 Responsabilità principali

!!! abstract ""
    - **Gateway tra mondi:** collega il mondo web (HTTP/REST/WebSocket) con il core robotico (ROS 2).
    - **API RESTful e real-time:** offre endpoint sincroni (REST) e canali push (WebSocket) per la massima flessibilità.
    - **Sicurezza centralizzata:** gestisce autenticazione tramite OIDC/JWT, verifica permessi e ruoli, audita le operazioni.
    - **Configurabilità:** può essere personalizzato per diversi deployment, supportando vari database e provider di autenticazione.
    - **Gestione utenti e permessi:** mantiene uno strato di gestione utenti, ruoli e mapping con provider esterni.

---

## 🏆 Vantaggi architetturali

!!! success ""
    - **Disaccoppiamento:** i client non accedono mai direttamente a ROS 2, ma solo tramite API standard e sicure.
    - **Scalabilità:** ogni dashboard, microservizio o sistema terzo può dialogare con Open-RMF senza conoscere i dettagli interni di ROS 2.
    - **Sicurezza:** centralizza la gestione della sicurezza, riducendo la superficie di attacco e facilitando audit e compliance.
    - **Realtime:** supporta notifiche push e aggiornamenti di stato in tempo reale tramite WebSocket.

---

## 🌐 Posizionamento nell’architettura

!!! tip ""
    L’API Server si trova tra il frontend (dashboard, microservizi, integrazioni) e il backend robotico Open-RMF.  
    È il solo punto di ingresso per tutte le richieste web ed è responsabile di tradurle in azioni, task o query verso il core RMF.

---

## 🔒 Sicurezza e gestione accessi

!!! warning ""
    Tutto il traffico verso l’API Server deve essere autenticato e autorizzato.  
    L’API Server:
    - Verifica l’identità degli utenti tramite provider OIDC/JWT.
    - Applica policy di autorizzazione granulari (ruoli, permessi, mapping).
    - Può essere configurato per audit, logging e compliance normativa.

---

## 🛠️ Configurabilità e deployment

L’API Server è pensato per essere:

- **Configurabile** tramite file di configurazione o variabili ambiente.
- **Portabile**: può essere containerizzato e orchestrato su Kubernetes/Docker.
- **Adattabile**: supporta più database (SQLite, PostgreSQL) e vari provider di autenticazione.

---