# Extractor Bancario — Laboratorio de la Colonia SAS

Convierte resúmenes bancarios PDF a Excel automáticamente usando IA.

## Estructura del proyecto

```
extractor-bancario/
├── index.html          ← Frontend (toda la UI + generación del Excel)
├── api/
│   └── process.js      ← Serverless function (proxy seguro a Anthropic)
├── vercel.json         ← Configuración de Vercel
└── README.md
```

---

## Cómo desplegarlo en Vercel (paso a paso)

### 1. Crear cuenta en GitHub (si no tenés)
Entrá a https://github.com y creá una cuenta gratuita.

### 2. Crear un repositorio nuevo
- En GitHub, hacé clic en **New repository**
- Nombre: `extractor-bancario` (o el que quieras)
- Dejalo en **Public** o **Private** (privado es más seguro)
- Hacé clic en **Create repository**

### 3. Subir los archivos
Tenés dos opciones:

**Opción A — Desde la web de GitHub (más fácil):**
- En tu repo recién creado, hacé clic en **Add file → Upload files**
- Arrastrá los archivos `index.html`, `vercel.json` y la carpeta `api/` (con `process.js` adentro)
- Hacé clic en **Commit changes**

**Opción B — Con Git (si tenés Git instalado):**
```bash
git init
git add .
git commit -m "primer commit"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/extractor-bancario.git
git push -u origin main
```

### 4. Crear cuenta en Vercel
- Entrá a https://vercel.com
- Hacé clic en **Sign Up** → elegí **Continue with GitHub**
- Autorizá la conexión

### 5. Importar el proyecto
- En Vercel, hacé clic en **Add New → Project**
- Buscá tu repositorio `extractor-bancario` y hacé clic en **Import**
- No cambies nada en la configuración
- Hacé clic en **Deploy** y esperá ~1 minuto

### 6. Agregar la API Key de Anthropic
Este es el paso más importante. La API key NO va en el código — va en las variables de entorno de Vercel.

- En tu proyecto de Vercel, andá a **Settings → Environment Variables**
- Hacé clic en **Add New**
- Nombre: `ANTHROPIC_API_KEY`
- Valor: tu API key (la conseguís en https://console.anthropic.com)
- Entornos: marcá **Production**, **Preview** y **Development**
- Hacé clic en **Save**

### 7. Redesplegar para que tome la variable
- Andá a la pestaña **Deployments**
- Hacé clic en los tres puntos del último deployment → **Redeploy**

### 8. Listo ✓
Vercel te da una URL del tipo:
```
https://extractor-bancario-tuusuario.vercel.app
```
Guardala en favoritos. Funciona desde cualquier dispositivo (PC, celular, tablet).

---

## Cómo obtener la API Key de Anthropic

1. Entrá a https://console.anthropic.com
2. Si no tenés cuenta, creá una (necesitás tarjeta de crédito para cargar crédito)
3. En el menú lateral, andá a **API Keys**
4. Hacé clic en **Create Key**
5. Copiate la key (empieza con `sk-ant-...`) — no la vas a poder ver de nuevo
6. Pegala en Vercel como se explicó arriba

**Costo estimado:** Procesar un resumen de ~300 movimientos cuesta aproximadamente U$D 0.05–0.10 con Claude Sonnet.

---

## Agregar nuevos bancos

Para incorporar un nuevo banco (Galicia, Nación, etc.), necesitás:

1. Conseguir un PDF de ejemplo del banco
2. En `index.html`, dentro del objeto `PROMPTS`, agregar una nueva clave con las instrucciones específicas para ese banco
3. En el array `BANKS`, cambiar `ready:false` a `ready:true` para ese banco
4. Subir los cambios a GitHub → Vercel redespliega automáticamente

---

## Funcionalidades actuales

- **Banco Patagonia / Francés** ✓
  - Hoja `CUENTA CORRIENTE`: saldo anterior, todos los movimientos, saldo calculado, saldo actual
  - Hoja `CTA CTE ESP P/PJ`: mismos datos
  - Columnas: FECHA, CONCEPTO, REFER., DÉBITOS, CRÉDITOS, SALDO
  - Números en formato argentino (punto miles, coma decimal)
  - Filas en amarillo donde el saldo calculado difiere del banco
  - Verificación del saldo final

- **Otros bancos** — próximamente

---

## Seguridad

- La API key de Anthropic **nunca llega al navegador** — está guardada en las variables de entorno de Vercel
- Los PDFs se envían directamente a la API de Anthropic y **no se guardan en ningún servidor propio**
- El Excel se genera completamente en el navegador (lado del cliente)
