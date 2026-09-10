# Aseo del hogar

Tracker compartido de tareas del hogar para Seb y Soffi. Reemplaza la tabla
de Notion "🧹 Aseo del hogar". Página estática (HTML/CSS/JS) con Firebase
Firestore como backend, pensada para GitHub Pages — mismo patrón que
`compras-seffi`.

## Cómo funciona

- Cada tarea es un documento en la colección `tareas` de Firestore.
- La primera vez que se abre la página con la base de datos vacía, se
  siembran automáticamente las 28 tareas migradas desde Notion.
- Click en una tarjeta avanza su estado: `Por hacer/Crítico/Pendiente` →
  `Haciendo` → `Listo`. Una tarea lista tiene un botón "Reabrir".
- Filtros por responsable (Seb / Soffi / Ambos) y por lugar.
- No requiere login: cualquiera con el link (tú o Soffi) puede ver y marcar
  tareas.

## Puesta en marcha (una sola vez)

1. **Crear proyecto Firebase**: en https://console.firebase.google.com,
   "Agregar proyecto" (puede ser el mismo proyecto que usa
   `compras-seffi` si prefieres reutilizarlo, o uno nuevo llamado
   p. ej. `aseo-hogar`).
2. **Activar Firestore**: en el proyecto, "Firestore Database" → "Crear
   base de datos" → modo producción, región `southamerica-east1` (o la que
   uses en tus otros proyectos).
3. **Reglas de Firestore** (acceso sin cuenta, solo para este uso
   personal/privado — el link no debe compartirse públicamente):
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /tareas/{tareaId} {
         allow read, write: if true;
       }
     }
   }
   ```
4. **Registrar una app web**: en Configuración del proyecto → "Tus apps" →
   ícono `</>` → registrar app → copia el objeto `firebaseConfig` que te
   entrega.
5. **Pegar la config**: reemplaza el bloque `firebaseConfig` al inicio del
   `<script>` en `index.html` con los valores reales (`apiKey`,
   `authDomain`, `projectId`, etc.).
6. **Publicar en GitHub Pages**: push de este repo a GitHub → Settings →
   Pages → Deploy from branch → rama `main`, carpeta `/root` (o `/docs` si
   prefieres esa convención). El sitio queda en
   `https://<tu-usuario>.github.io/<repo>/`.

## Datos migrados

Los 28 registros de la tabla de Notion original quedaron incluidos como
semilla en `index.html` (arreglo `SEED_TAREAS`), con nombres normalizados
(Seb / Soffi) y notas de la planilla original preservadas en el campo
`notas`. Dos filas vacías de Notion (sin nombre de tarea) no se migraron.
