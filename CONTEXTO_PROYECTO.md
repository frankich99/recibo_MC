# MEMORIA TÉCNICA Y CONTEXTO DEL PROYECTO (PARA MODELOS Y AGENTES IA)

> **PROPÓSITO DE ESTE ARCHIVO:**
> Este documento sirve como memoria persistente para cualquier LLM / modelo de IA que continúe el desarrollo o mantenimiento de este repositorio (`recibo_mc`).
> Lee este archivo antes de realizar cualquier cambio en el código o responder solicitudes del usuario.

---

## 1. Identidad y Dominio del Proyecto
- **Nombre del Proyecto:** `recibo_mc`
- **Tipo de Aplicación:** SPA (Single Page Application) 100% frontend en vanilla HTML5, CSS3 y JavaScript ES6+.
- **Archivo Principal:** [`index.html`](file:///G:/proyectos/recibo_mc/index.html). No usa frameworks pesados (React, Vue, etc.) ni bundlers (Webpack, Vite). Todo se ejecuta directamente en el navegador.
- **Función:** Generación, personalización, gestión de series/folios y emisión impresa de recibos de agua potable y cobranza para comités / juntas de agua (MC).

---

## 2. REGLAS DE ORO DE CALIBRACIÓN FÍSICA (NO VIOLAR BAJO NINGUNA CIRCUNSTANCIA)

El usuario utiliza papel pre-impreso y hojas cortadas a medidas específicas:

### A. Papel Oficio (Pliego completo)
- **Medida Real Oficial:** `21.6 cm × 33.0 cm` (o `216 mm × 330 mm`).
- **Estructura Vertical de la Hoja:**
  - 3 Tiras de recibos horizontales de `10.2 cm` (`102 mm`) cada una = `306 mm`.
  - 1 Franja inferior de corte sobrante de `2.4 cm` (`24 mm`) con clase `.corte-sobrante` y línea discontinua = `24 mm`.
  - Altura total: `306 mm + 24 mm = 330 mm` (`33.0 cm`).

### B. Simetría de la Tira Horizontal (21.6 cm de ancho)
- **Ancho total de tira:** `216 mm`.
- **Dividida exactamente en 2 talones:** `108.0 mm` cada uno (`10.8 cm`).
  - Talón Izquierdo: Cobranza / Archivo oficina.
  - Talón Derecho: Usuario / Cliente.
- **Composición milimétrica por talón:**
  - Margen exterior: `9.00 mm`
  - Caja de contenido (`.talon-box`): `90.00 mm`
  - Margen interior corte central: `9.00 mm`
  - **Fórmula:** `9.00 + 90.00 + 9.00 = 108.00 mm`.
  - Ambos talones deben ser perfectamente simétricos e idénticos en posición horizontal.

### C. Papel "Tira Suelta"
- **Medida:** `21.6 cm × 10.2 cm` (`216 mm × 102 mm`).
- Representa 1 sola tira cortada sin sobrante inferior.

### D. Formato "Ajustado"
- **ELIMINADO Y DEPRECADO:** Se eliminó por completo del código y de la interfaz por instrucción expresa del usuario. No restaurar bajo ningún concepto.

---

## 3. Optimizaciones Móviles (Modo Celular)
- **Header:** Diseñado para teléfonos (< 640px) ocupando apenas ~45px de altura sin truncar el título ni generar desbordes.
- **Tabla de Conceptos (Paso 5):** Estructura responsiva (`w-12` partida, `min-w-0` descripción, `w-18` importe) para visualizar todos los datos sin barra de desplazamiento horizontal forzada en pantallas de 320px a 480px.
- **Modal de Ajustes:** Vista dual (Controles / Vista Recibo 1:1) con función `autoAjustarZoomSlip()` que calcula en vivo el factor `scale()` sobre el contenedor para que el recibo quepa completo en pantalla de celular.

---

## 4. Arquitectura del Código en `index.html`

### A. Estado y Persistencia (`localStorage`)
- Clave de configuración: `recibo_mc_ajustes`
- Propiedades clave:
  ```javascript
  {
    margenLateral: 9.0,          // Margen exterior en mm (default: 9.0)
    margenCorteCentral: 9.0,     // Margen central en mm (default: 9.0)
    boxWidthMm: 90.0,            // Ancho físico de la caja en mm (default: 90.0)
    borderWidth: 1.0,            // Grosor bordes
    hHeader: 16.0,               // Altura encabezado
    // ... otros offsets y fuentes
  }
  ```
- Si `localStorage` tiene los valores antiguos (8.75), la función `cargarAjustes()` los migra automáticamente a `9.0`.

### B. Métodos Clave
- `getPageDimensions(tirasEnPag)`: Retorna la geometría de la página según el formato activo (`oficio` = 216×330mm, `tira` = 216×102mm, `a4` = 210×297mm).
- `buildStripHTML(...)`: Ensambla el HTML de la tira aplicando los márgenes calculados `pLeft` y `pRight` (`mExt: 9.0mm`, `mCut: 9.0mm`).
- `renderSlipContent(...)`: Generador del talón interior de 90 × 92 mm. Fuente única de verdad para el diseño del recibo.
- `updateAjustes()`: Aplica variables CSS en vivo y recalcula vistas.
- `recalcSheetScale()` / `autoAjustarZoom()`: Ajusta el escalado visual (`scale(...)`) para que la hoja quepa perfectamente en la vista previa del usuario sin desbordar ni distorsionar la geometría imprimible.

---

## 5. Gestión de Git
- Repositorio: `https://github.com/frankich99/recibo_MC.git`
- Rama de trabajo activo: `dev`
- Rama principal: `main`
- Flujo: Tras realizar y verificar cambios en `dev`, fusionar o actualizar `main` (`git checkout main; git merge dev; git push origin main; git checkout dev`).

---

## 6. Estilo de Comunicación con el Usuario
- Por directriz global del usuario, todas las respuestas deben darse en **modo `/caveman ultra`**:
  - Máxima compresión: frases directas, fragmentos, tablas y viñetas.
  - Eliminar cortesías, artículos y rellenos.
  - Usar enlaces cliqueables en formato `file://` para archivos y símbolos de código.
