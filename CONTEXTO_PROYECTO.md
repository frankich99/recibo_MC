# MEMORIA TÉCNICA Y CONTEXTO DEL PROYECTO (PARA MODELOS Y AGENTES IA)

> **PROPÓSITO DE ESTE ARCHIVO:**
> Este documento sirve como memoria persistente para cualquier LLM / modelo de IA que continúe el desarrollo o mantenimiento de este repositorio (`recibo_mc`).
> Lee este archivo antes de realizar cualquier cambio en el código o responder solicitudes del usuario.

---

## 1. Identidad y Dominio del Proyecto
- **Nombre del Proyecto:** `recibo_mc`
- **Tipo de Aplicación:** SPA (Single Page Application) 100% frontend en vanilla HTML5, CSS3 y JavaScript ES6+.
- **Archivo Principal:** [`index.html`](file:///G:/proyectos/recibo_mc/index.html). No usa frameworks pesados (React, Vue, etc.) ni bundlers (Webpack, Vite). Todo se compila directamente en el navegador.
- **Función:** Generación, personalización, gestión de series/folios y emisión impresa de recibos de agua potable y cobranza para comités / juntas de agua (MC).

---

## 2. REGLAS DE ORO DE CALIBRACIÓN FÍSICA (NO VIOLAR BAJO NINGUNA CIRCUNSTANCIA)

El usuario utiliza papel pre-impreso y hojas cortadas a medidas específicas:

### A. Papel Oficio (Pliego completo)
- **Medida Real Oficial:** `21.5 cm × 33.0 cm` (o `215 mm × 330 mm`).
- **NUNCA alterar a:** `21.6 cm`, `21.2 cm`, `21.05 cm` o `30.3 cm`.
- **Estructura Vertical de la Hoja:**
  - 3 Tiras de recibos horizontales de `10.2 cm` (`102 mm`) cada una = `306 mm`.
  - 1 Franja inferior de corte sobrante de `2.4 cm` (`24 mm`) con clase `.corte-sobrante` y línea discontinua = `24 mm`.
  - Altura total: `306 mm + 24 mm = 330 mm` (`33.0 cm`).

### B. Simetría de la Tira Horizontal (21.5 cm de ancho)
- **Ancho total de tira:** `215 mm`.
- **Dividida exactamente en 2 talones:** `107.5 mm` cada uno (`10.75 cm`).
  - Talón Izquierdo: Cobranza / Archivo oficina.
  - Talón Derecho: Usuario / Cliente.
- **Composición milimétrica por talón:**
  - Margen exterior: `8.75 mm`
  - Caja de contenido (`.talon-box`): `90.00 mm`
  - Margen interior corte central: `8.75 mm`
  - **Fórmula:** `8.75 + 90.00 + 8.75 = 107.50 mm`.
  - Ambos talones deben ser perfectamente simétricos e idénticos en posición horizontal.

### C. Papel "Tira Suelta"
- **Medida:** `21.5 cm × 10.2 cm` (`215 mm × 102 mm`).
- Representa 1 sola tira cortada sin sobrante inferior.

### D. Formato "Ajustado"
- **ELIMINADO Y DEPRECADO:** Existía previamente como `21.2 × 30.3 cm`. Se eliminó por completo del código y de la interfaz por instrucción expresa del usuario. No restaurar bajo ningún concepto.

---

## 3. Arquitectura del Código en `index.html`

### A. Estado y Persistencia (`localStorage`)
- Clave de configuración: `recibo_mc_ajustes`
- Propiedades clave:
  ```javascript
  {
    margenLateral: 8.75,         // Margen exterior en mm (default: 8.75)
    margenCorteCentral: 8.75,    // Margen central en mm (default: 8.75)
    boxWidthMm: 90.0,            // Ancho físico de la caja en mm (default: 90.0)
    borderWidth: 1.0,            // Grosor bordes
    hHeader: 26,                 // Altura encabezado
    // ... otros offsets y fuentes
  }
  ```
- Si `localStorage` no tiene estos campos, la función `cargarAjustes()` debe inicializarlos con sus valores por defecto (8.75, 8.75, 90.0).

### B. Métodos Clave
- `getPageDimensions(tirasEnPag)`: Retorna la geometría de la página según el formato activo (`oficio`, `tira`, `a4`).
- `buildStripHTML(...)`: Ensambla el HTML de la tira aplicando los márgenes calculados `pLeft` y `pRight`.
- `renderSlipContent(...)`: Generador del talón interior de 90 × 92 mm. Fuente única de verdad para el diseño del recibo.
- `updateAjustes()`: Aplica variables CSS en vivo y recalcula vistas.
- `recalcSheetScale()`: Ajusta el escalado visual (`scale(...)`) para que la hoja quepa perfectamente en la vista previa del usuario sin desbordar ni distorsionar la geometría imprimible.

### C. Impresión `@media print`
- Inyecta `@page { size: 215mm 330mm; margin: 0; }` para Oficio.
- Las dimensiones de impresión no deben depender de la resolución de pantalla ni del zoom del navegador.

---

## 4. Gestión de Git
- Repositorio: `https://github.com/frankich99/recibo_MC.git`
- Rama de trabajo activo: `dev`
- Rama principal: `main`
- Flujo: Tras realizar y verificar cambios en `dev`, fusionar o actualizar `main` (`git checkout main; git merge dev; git push origin main; git checkout dev`).

---

## 5. Estilo de Comunicación con el Usuario
- Por directriz global del usuario, todas las respuestas deben darse en **modo `/caveman ultra`**:
  - Máxima compresión: frases directas, fragmentos, tablas y viñetas.
  - Eliminar cortesías, artículos y rellenos.
  - Usar enlaces cliqueables en formato `file://` para archivos y símbolos de código.
