# 🧩 Reference completo degli Endpoint
Qui sotto trovi **tutti gli endpoint dettagliati** con metodi, path, parametri ed esempi

!!! tip "Quick start"
    - **Base URL**: `http://localhost:8000`  
    - **Live Swagger UI**: [http://localhost:8000/docs](http://localhost:8000/docs)  
    - **OpenAPI JSON**: [http://localhost:8000/openapi.json](http://localhost:8000/openapi.json)

---

## 📊 Schema ad Alto Livello

```mermaid
flowchart LR
    subgraph Client
        A[Dashboard / Client]
    end

    subgraph API
        AL[Alert] 
        DO[Porte] 
        LI[Ascensori] 
        TA[Task] 
        SC[Pianificati] 
        FL[Flotte] 
        AD[Admin] 
    end

    A --> AL
    A --> DO
    A --> LI
    A --> TA
    A --> SC
    A --> FL
    A --> AD

    AL -- "POST /alerts/request" --> AR[Crea Alert]
    DO -- "POST /doors/{name}/request" --> DR[Richiedi porta]
    LI -- "POST /lifts/{name}/request" --> LR[Richiedi ascensore]
    TA -- "POST /tasks/dispatch_task" --> TD[Invia task]
    SC -- "POST /scheduled_tasks" --> SCr[Crea pianificato]
    FL -- "GET /fleets" --> FLs[Elenco flotte]
    AD -- "POST /admin/users" --> AUs[Crea utente]
```

---

## 🔐 Autenticazione

Tutti gli endpoint (eccetto `/socket.io`) richiedono un token Bearer ottenuto dal tuo IdP OpenID Connect.

```http
Authorization: Bearer <jwt>
```

---

## 📁 Endpoint catalogue

### 🚨 Alert (`/alerts`)
!!! abstract "Concept"
    Gli **alert** sono notifiche interattive mostrate agli operatori (allarme antincendio, batteria scarica, ecc.).

| Metodo | Percorso | Descrizione | Corpo richiesta |
|--------|----------|-------------|-----------------|
| `POST` | `/alerts/request` | Crea nuovo alert | `AlertRequest` |
| `GET`  | `/alerts/request/{alert_id}` | Leggi singolo alert | — |
| `POST` | `/alerts/request/{alert_id}/respond` | Rispondi a un alert | `{"response": "ack"}` |
| `GET`  | `/alerts/requests/task/{task_id}` | Alert relativi a un task | — |
| `GET`  | `/alerts/unresponded_requests` | Alert ancora da rispondere | — |

??? example "Example – Crea un alert"
    ```bash
    curl -X POST http://localhost:8000/alerts/request \
      -H "Authorization: Bearer $TOKEN" \
      -H "Content-Type: application/json" \
      -d '{
        "title": "Fuoco al piano 1",
        "message": "Evacuazione immediata",
        "tier": "critical",
        "display": true
      }'
    ```

---

### 🚪 Porte – Doors (`/doors`)

| Metodo | Percorso | Descrizione | Corpo |
|--------|----------|-------------|-------|
| `GET`  | `/doors` | Elenca tutte le porte | — |
| `GET`  | `/doors/{name}/state` | Stato attuale di una porta | — |
| `POST` | `/doors/{name}/request` | Apri / Chiudi | `{"mode": 2}` |

??? info "Modalità porta"
    | Valore | Significato |
    |--------|-------------|
    | `0`    | Chiusa      |
    | `1`    | In movimento |
    | `2`    | Aperta      |

---

### 🛗 Ascensori – Lifts (`/lifts`)

| Metodo | Percorso | Descrizione | Corpo |
|--------|----------|-------------|-------|
| `GET`  | `/lifts` | Elenca ascensori | — |
| `GET`  | `/lifts/{name}/state` | Stato + piano attuale | — |
| `POST` | `/lifts/{name}/request` | Chiama / sposta ascensore | `LiftRequest` |

---

### 📝 Task (`/tasks`)

| Metodo | Percorso | Descrizione | Corpo |
|--------|----------|-------------|-------|
| `POST` | `/tasks/dispatch_task` | Invia nuovo task | `DispatchTaskRequest` |
| `POST` | `/tasks/cancel_task` | Annulla task in corso | `{"task_id": "uuid"}` |
| `GET`  | `/tasks/requests` | Lista delle richieste task | — |
| `GET`  | `/tasks/{task_id}/state` | Stato di un singolo task | — |
| `GET`  | `/tasks/{task_id}/log` | Log eventi del task | — |

---

### 📅 Task pianificati – Scheduled Tasks (`/scheduled_tasks`)

| Metodo | Percorso | Descrizione | Corpo |
|--------|----------|-------------|-------|
| `POST` | `/scheduled_tasks` | Crea task ricorrente | `ScheduledTaskRequest` |
| `GET`  | `/scheduled_tasks` | Lista task pianificati | — |
| `GET`  | `/scheduled_tasks/{id}` | Dettaglio singola pianificazione | — |
| `DELETE` | `/scheduled_tasks/{id}` | Elimina pianificazione | — |

---

### 🚛 Flotte e robot – Fleets (`/fleets`)

| Metodo | Percorso | Descrizione |
|--------|----------|-------------|
| `GET`  | `/fleets` | Tutte le flotte e i robot |
| `GET`  | `/fleets/{name}/state` | Stato flotta (robot, batteria, problemi…) |
| `POST` | `/fleets/{name}/decommission` | Metti robot fuori servizio |
| `POST` | `/fleets/{name}/recommission` | Riporta robot in servizio |

---

### ⚙️ Amministrazione – Admin (`/admin`)

!!! danger "Richiede ruolo admin"

| Metodo | Percorso | Descrizione |
|--------|----------|-------------|
| `GET`  | `/admin/users` | Elenca utenti |
| `POST` | `/admin/users` | Crea utente |
| `PUT`  | `/admin/users/{u}/roles` | Sostituisci ruoli |
| `POST` | `/admin/roles` | Crea ruolo |
| `POST` | `/admin/roles/{r}/permissions` | Aggiungi permesso |

---

## 📦 Schemi comuni

### `AlertRequest`
```json
{
  "title": "string",
  "message": "string",
  "tier": "info | warning | critical",
  "display": true,
  "responses_available": ["ack", "ignore"]
}
```

### `DispatchTaskRequest`
```json
{
  "category": "patrol",
  "description": {"zone": "L1_corridor"},
  "fleet_name": "tinyRobot",
  "priority": {"type": "binary", "value": 1},
  "requester": "dashboard"
}
```

### `LiftRequest`
```json
{
  "request_type": 1,
  "destination": "L2",
  "door_mode": 0
}
```

### `ScheduledTaskRequest`
```json
{
  "task_request": { /* stesso di DispatchTaskRequest */ },
  "schedules": [
    { "every": 1, "period": "day", "at": "09:00" }
  ],
  "start_from": "2025-07-30T09:00:00Z",
  "until": "2025-12-31"
}
```

---

## 🧪 Script di prova rapida

Salva come `test_api.sh`, inserisci il tuo `$TOKEN`.

```bash
#!/usr/bin/env bash
HOST="http://localhost:8000"
TOKEN="IL_TUO_JWT"

# Lista flotte
curl -sS -H "Authorization: Bearer $TOKEN" $HOST/fleets | jq .

# Invia task di prova
curl -sS -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"category":"patrol","description":{"zone":"corridoio_L1"},"fleet_name":"tinyRobot"}' \
     $HOST/tasks/dispatch_task | jq .
```