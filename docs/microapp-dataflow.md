## 🧩 Flusso dati delle Micro-App (rmf-dashboard-framework)

Tutte le micro-app **ricevono e interpretano** i dati tramite l’hook **useRmfApi()**, che restituisce un’istanza di `RmfApi`.  
Di seguito il dettaglio **per micro-app** (file, chiamata, parsing).

---

### 🤖 Robots

**File:** robots-table.tsx  
**Flusso:**
```ts
const rmfApi = useRmfApi();
const fleets = await rmfApi.fleetsApi.getFleetsFleetsGet();
const tasks   = await rmfApi.tasksApi.queryTaskStatesTasksGet();
```

Usa l’hook `useRmfApi` per ottenere l’istanza di API.  
Recupera le flotte e i robot tramite `rmfApi.fleetsApi.getFleetsFleetsGet()`.

- **Parsing:** i payload vengono mappati nel tipo `RobotTableData`.  
- **Sorgente:** REST `/fleets` + `/tasks`.

---

### 🗺️ Map

**File:** map.tsx  
**Flusso:**
```ts
const buildingMap$ = rmfApi.buildingMapObs;
const fleets$      = rmfApi.fleetsObs;
```

Sottoscrive a `rmfApi.buildingMapObs` per la mappa dell’edificio.  
Sottoscrive a `rmfApi.fleetsObs` per flotte e robot.

- **Parsing:** oggetti `RobotData`, `Place` estratti dagli observable RxJS.  
- **Sorgente:** WebSocket (building map + posizione robot).

---

### 🚪 Doors

**File:** doors-table.tsx  
**Flusso:**
```ts
const doors$ = rmfApi.doorsObs;
```

Usa `rmfApi.doorsObs` per ottenere la lista delle porte.  
I dati sono già in formato array di Door e vengono visualizzati direttamente.

- **Parsing:** array `Door[]` già tipizzato.  
- **Sorgente:** WebSocket `/doors`.

---

### 🛗 Lifts

**File:** lifts-table.tsx  
**Flusso:**
```ts
const lifts$ = rmfApi.liftsObs;
```

Usa `rmfApi.liftsObs` per ottenere la lista degli ascensori.  
I dati sono già in formato array di Lift.

- **Parsing:** array `Lift[]` pronto per la tabella.  
- **Sorgente:** WebSocket `/lifts`.

---

### 📝 Task

**File:** tasks-window.tsx  
**Flusso:**
```ts
const tasks = await rmfApi.tasksApi.queryTaskStatesTasksGet();
```

Recupera i task tramite `rmfApi.tasksApi.queryTaskStatesTasksGet()`.
I dati vengono mappati in oggetti `TaskState` e visualizzati.

- **Parsing:** oggetti `TaskState`; per l’export CSV viene usato `utils.ts`.  
- **Sorgente:** REST `/tasks`.

---

### 🔒 Mutex Groups

**File:** robot-mutex-group-table.tsx  
**Flusso:**
```ts
const fleets = await rmfApi.fleetsApi.getFleetsFleetsGet();
const mutexGroups = extractMutexGroups(fleets);
```

Recupera le flotte tramite `rmfApi.fleetsApi.getFleetsFleetsGet()`.
Estrae i gruppi mutex dai dati dei robot tramite mapping.

- **Parsing:** estrazione manuale dai dati flotta.  
- **Sorgente:** REST `/fleets`.

---

### 🧰 Parsing generale
- **Tipizzazione TypeScript:** ogni dato è tipizzato (es. `RobotState`, `TaskState`, `Door`, ecc.).
- **Funzioni di utilità**: per estrarre o trasformare dati (es. `getTaskBookingLabelFromTaskState`).
- **Observable**: i dati sono spesso forniti come observable e aggiornati in tempo reale.

---
