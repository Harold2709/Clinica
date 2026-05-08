# DentalCare — Sistema de Gestión Clínica

Sistema de gestión para clínica dental pequeña. Diseñado para 3 roles:
Secretaria, Residente y Doctor Principal.

---

## Estructura del proyecto

```
dentalcare/
│
├── index.html              ← Punto de entrada (abrir en navegador)
│
├── css/
│   ├── variables.css       ← Tokens de diseño: colores, tipografía, sombras
│   ├── base.css            ← Reset, animaciones globales, utilidades
│   ├── components.css      ← Botones, badges, cards, forms, chips, toasts
│   ├── layout.css          ← Sidebar, topbar, modal, estructura app
│   └── screens.css         ← Estilos específicos de cada pantalla
│
└── js/
    ├── app.js              ← Punto de entrada JS (inicializa todo)
    ├── data.js             ← Store de datos (mock → reemplazar por API)
    ├── ui.js               ← Utilidades: toast, modal, navegación, validación
    ├── roles.js            ← Control de acceso por rol
    └── modules/
        ├── agenda.js       ← Lógica de la pantalla Agenda
        ├── pacientes.js    ← Lógica de la pantalla Pacientes
        └── flujo.js        ← Lógica del acordeón Flujo de trabajo
```

---

## Cómo abrir el proyecto

### Opción 1 — Servidor local (recomendado)
Necesario porque el HTML usa `<script type="module">`.

```bash
# Si tienes Python instalado:
cd dentalcare
python -m http.server 8080
# Abre: http://localhost:8080

# Si tienes Node.js:
npx serve .
# Abre la URL que muestre la terminal
```

### Opción 2 — VS Code con Live Server
1. Instala la extensión **Live Server** en VS Code
2. Abre la carpeta `dentalcare/`
3. Clic derecho en `index.html` → "Open with Live Server"

> ⚠️ Abrir `index.html` directamente con doble clic en el explorador
> de archivos **no funcionará** por las restricciones de módulos ES6.
> Siempre usa un servidor local.

---

## Pantallas disponibles

| Pantalla        | Acceso                        |
|-----------------|-------------------------------|
| Agenda          | Todos los roles               |
| Pacientes       | Todos los roles               |
| Ortodoncia      | Todos los roles               |
| Endodoncia      | Residente + Doctor Principal  |
| Finanzas        | Solo Doctor Principal         |
| Flujo de trabajo| Todos los roles               |

---

## Próximos pasos para producción

### 1. Conectar a base de datos
Reemplaza los datos en `js/data.js` por llamadas `fetch()` a una API REST:

```js
// Ejemplo: reemplazar datos hardcodeados
const pacientes = await fetch('/api/pacientes').then(r => r.json());
```

**Opciones de backend recomendadas:**
- **SQLite + Node.js (Express)** — ideal para uso local en la clínica
- **SQLite + Python (Flask/FastAPI)** — si el equipo prefiere Python
- **Supabase** — si quieren acceso desde múltiples dispositivos

### 2. Autenticación real
Actualmente los roles se cambian con botones. En producción:
- Añadir una pantalla de login (`pages/login.html`)
- Usar JWT o sesiones para autenticar
- El backend devuelve el rol del usuario al iniciar sesión

### 3. Persistencia de formularios
En `js/modules/agenda.js` y `js/modules/pacientes.js`, los comentarios
`// TODO: persistir en DB` indican los puntos donde conectar la API.

### 4. Recordatorios por WhatsApp
Integrar con **Twilio** o **WhatsApp Business API**:
```js
// Desde el backend al guardar una cita:
await fetch('/api/recordatorio', {
  method: 'POST',
  body: JSON.stringify({ telefono: paciente.telefono, fecha: cita.fecha })
});
```

### 5. Subida de radiografías y fotos
- En el módulo de Ortodoncia y Endodoncia, el área de "Subir foto" 
  necesita un `<input type="file">` conectado a un endpoint de upload
- Usar **Cloudinary**, **S3** o almacenamiento local del servidor

---

## Stack tecnológico

- **HTML5 semántico** — sin frameworks, sin dependencias
- **CSS3 con variables custom** — diseño system escalable
- **JavaScript ES6 Modules** — imports nativos del navegador
- **Google Fonts** — DM Sans + DM Mono (carga desde CDN)

Sin `npm install`. Sin build step. Sin Webpack. Listo para abrir.
