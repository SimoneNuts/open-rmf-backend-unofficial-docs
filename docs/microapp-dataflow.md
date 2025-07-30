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
- **Parsing:** i payload vengono mappati nel tipo `RobotTableData`.  
- **Sorgente:** REST `/fleets` + `/tasks`.

---

### 🗺️ Mappa  

**File:** map.tsx  
**Flusso:**
```ts
const buildingMap$ = rmfApi.buildingMapObs;
const fleets$      = rmfApi.fleetsObs;
```
- **Parsing:** oggetti `RobotData`, `Place` estratti dagli observable RxJS.  
- **Sorgente:** WebSocket (building map + posizione robot).

---

### 🚪 Porte  

**File:** doors-table.tsx  
**Flusso:**
```ts
const doors$ = rmfApi.doorsObs;
```
- **Parsing:** array `Door[]` già tipizzato.  
- **Sorgente:** WebSocket `/doors`.

---

### 🛗 Ascensori  

**File:** lifts-table.tsx  
**Flusso:**
```ts
const lifts$ = rmfApi.liftsObs;
```
- **Parsing:** array `Lift[]` pronto per la tabella.  
- **Sorgente:** WebSocket `/lifts`.

---

### 📝 Task  

**File:** tasks-window.tsx  
**Flusso:**
```ts
const tasks = await rmfApi.tasksApi.queryTaskStatesTasksGet();
```
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
- **Parsing:** estrazione manuale dai dati flotta.  
- **Sorgente:** REST `/fleets`.

---

### 🧰 Parsing generale
- **TypeScript:** tipi generati da OpenAPI (`TaskState`, `RobotTableData`, ecc.).  
- **Utility:** funzioni di mappatura (`getPlaces`, `getTaskBookingLabelFromTaskState`).  
- **Validazione:** classi auto-generate con metodo `.validate()`.

---

Se vuoi approfondire una micro-app specifica, apri il relativo componente e cerca `useRmfApi()` e le funzioni di mapping.