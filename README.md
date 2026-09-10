# Sistema de Emisión e Impresión de Recibos (recibo_mc)

Sistema web SPA (Single Page Application) en **vanilla HTML5 / CSS3 / JavaScript (ES6+)** para diseño, personalización, previsualización e impresión de alta precisión de recibos de servicios / cobranza de agua potable.

---

## ⚠️ REGLAS CRÍTICAS DE CALIBRACIÓN FÍSICA (LEER OBLIGATORIAMENTE)

Cualquier modelo de IA o desarrollador que modifique las medidas o el CSS de impresión DEBE respetar estrictamente estas especificaciones milimétricas:

### 1. Dimensiones de Papel
| Formato | Medidas Físicas | Distribución Vertical | Uso |
|---|---|---|---|
| **Oficio (Principal)** | **21.5 cm × 33.0 cm** (215 × 330 mm) | 3 tiras de 10.2 cm (306 mm) + 2.4 cm (24 mm) sobrante inferior | Impresión estándar de 3 recibos por pliego |
| **Tira Suelta** | **21.5 cm × 10.2 cm** (215 × 102 mm) | 1 tira individual exacta al ras (sin sobrante) | Impresoras continuas / alimentación de tira |
| **A4 Estándar** | **21.0 cm × 29.7 cm** (210 × 297 mm) | 2 tiras de 10.2 cm (204 mm) + 9.3 cm corte sobrante | Pliegos A4 convencionales |

> **PROHIBICIÓN ESTRICTA:**
> - **NUNCA** usar 21.6 cm, 21.2 cm, 21.05 cm ni 30.3 cm para Oficio. La medida física real es **21.5 cm × 33.0 cm**.
> - El formato "Ajustado" fue **completamente eliminado**. No reintroducir botones ni referencias a "ajustado".

### 2. Simetría Horizontal Exacta de Cada Tira (21.5 cm / 215 mm)
Cada tira horizontal se divide en **2 talones gemelos** (Izquierdo: Cobranza / Talón Oficina; Derecho: Usuario / Cliente).
- **Ancho por talón (Slip Width):** `107.5 mm` (`10.75 cm`, mitad exacta de 215 mm).
- **Caja de contenido (`.talon-box`):** `90.00 mm` de ancho × `92.00 mm` de alto.
- **Márgenes simétricos por talón:**
  - Margen exterior lateral: `8.75 mm`
  - Caja de recibo: `90.00 mm`
  - Margen interior hacia línea de corte central: `8.75 mm`
  - **Suma exacta:** `8.75 mm + 90.00 mm + 8.75 mm = 107.50 mm`.
  - **Total de la tira (2 talones):** `107.50 mm × 2 = 215.00 mm` (**21.5 cm**).

### 3. Distribución Vertical en Hoja Oficio (33.0 cm / 330 mm)
- Tira 1: `102.00 mm`
- Tira 2: `102.00 mm`
- Tira 3: `102.00 mm`
- **Subtotal recibos:** `306.00 mm` (30.6 cm)
- **Sobrante inferior con línea punteada de corte:** `24.00 mm` (2.4 cm)
- **Total:** `330.00 mm` (**33.0 cm**).

---

## 🛠️ Arquitectura y Estructura del Código

Todo el aplicativo reside en un único archivo autónomo optimizado:

```text
recibo_mc/
├── index.html            # Core completo: Markup, Estilos (@media print) y Lógica JS
├── README.md             # Este archivo de especificaciones y guía técnica
├── CONTEXTO_PROYECTO.md  # Ficha técnica y directrices directas para agentes/LLMs
├── datos/                # Archivos de datos / plantillas JSON o CSV
├── logo/                 # Activos gráficos y logotipos institucionales
└── .agents/              # Habilidades y agentes de soporte (caveman, ui-ux-pro-max, etc.)
```

### Funciones Clave en [`index.html`](file:///G:/proyectos/recibo_mc/index.html)
1. `getPageDimensions(tirasEnPag)`: Retorna las medidas milimétricas (`widthMm`, `heightMm`, `slipWidthMm`, etc.) según el papel seleccionado (`oficio`, `tira`, `a4`).
2. `buildStripHTML(serieNum, forceBlank, ...)`: Genera el marcado HTML de cada tira horizontal aplicando los paddings simétricos calculados (`mExt`, `mCut`).
3. `renderSlipContent(params)`: Fuente única de verdad para el contenido interior del talón de 90 × 92 mm (encabezado, número, fecha, datos del usuario, tabla de conceptos, firmas).
4. `updateAjustes()` / `cargarAjustes()`: Persistencia en `localStorage` con fallback seguro de valores por defecto:
   - `margenLateral`: default `8.75` mm.
   - `margenCorteCentral`: default `8.75` mm.
   - `boxWidthMm`: default `90.0` mm.
   - `borderWidth`, `fontSize`, `hHeader`, etc.
5. `recalcSheetScale()`: Ajuste dinámico del preview en pantalla mediante transform CSS `scale()` para visualización fiel sin deformar el DOM imprimible.
6. `@media print`:
   - `@page { size: 215mm 330mm; margin: 0; }` para Oficio.
   - `@page { size: 215mm 102mm; margin: 0; }` para Tira.
   - Remoción de encabezados/pies de página del navegador, colores forzados (`-webkit-print-color-adjust: exact`).

---

## 🎛️ Panel de Ajustes (Medidas Calibrables en UI)
En el modal de **Ajustes** -> pestaña **Medidas**:
- **Margen Exterior (Lateral):** Control numérico + steppers en mm y cm (default: 8.75 mm).
- **Margen Corte Central:** Control numérico + steppers en mm y cm (default: 8.75 mm).
- **Ancho Caja Recibo:** Control numérico + steppers en mm y cm (default: 90.00 mm).
- Modificar estos valores recalcula inmediatamente la tira y el preview manteniendo la simetría.

---

## 🌿 Flujo Git
- **Ramas:** `dev` (desarrollo activo) y `main` (producción).
- **Regla:** Mantener ambas ramas sincronizadas tras validaciones exitosas.
