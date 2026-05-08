# 🔧 Troubleshooting - Sherman Dashboard

## ⚠️ Problema: El dashboard muestra datos de ejemplo

### Síntomas:
- Solo aparecen 3 transacciones fijas
- Datos de "Diana Muñoz", "Angie Estupiñan", "Luis García"
- Los ingresos no se actualizan desde Google Sheets

### ✅ Solución Paso a Paso:

## 1. Verificar que Google Sheets API esté habilitada

1. Ve a [Google Cloud Console](https://console.cloud.google.com/)
2. Selecciona tu proyecto
3. Ve a **"APIs & Services"** → **"Library"**
4. Busca **"Google Sheets API"**
5. Verifica que esté **HABILITADA** (debe decir "API enabled")
6. Si no está habilitada, haz click en **"Enable"**

## 2. Verificar permisos de la hoja de Google Sheets

1. Abre tu Google Sheet: `1I5HSsNyutjuaLGR1ruyh5B_rKCuOU77GdduAWpzJFf8`
2. Click en **"Compartir"** (botón superior derecho)
3. En "Acceso general", selecciona:
   - **"Cualquiera con el enlace"**
   - Permiso: **"Lector"** o **"Viewer"**
4. Click en **"Listo"**

**URL directa para verificar:**
```
https://docs.google.com/spreadsheets/d/1I5HSsNyutjuaLGR1ruyh5B_rKCuOU77GdduAWpzJFf8/edit
```

## 3. Verificar nombre de la pestaña

1. Abre tu Google Sheet
2. Verifica que la pestaña se llame **exactamente** "Finance" (con F mayúscula)
3. Si tiene otro nombre, hay dos opciones:
   - **Opción A**: Renombra la pestaña a "Finance"
   - **Opción B**: Cambia la variable en EasyPanel a tu nombre actual

## 4. Verificar estructura de datos

Tu hoja debe tener estas columnas en este orden:

| A | B | C | D | E | F | G | H | I | J | K |
|---|---|---|---|---|---|---|---|---|---|---|
| ID | Fecha | Hora | Tipo Movimiento | Colaborador | Concepto | Tipo Servicio | Subtipo | Cantidad Perros | Monto | Observaciones |

**Formato importante:**
- **Fecha**: YYYY-MM-DD (ej: 2026-01-22)
- **Tipo Movimiento**: "Ingreso" o "Gasto" (exactamente así)
- **Monto**: Número sin símbolos o con $ (ej: 50000 o $50.000)

## 5. Probar API Key manualmente

Abre esta URL en tu navegador (reemplaza con tu API Key):

```
https://sheets.googleapis.com/v4/spreadsheets/1I5HSsNyutjuaLGR1ruyh5B_rKCuOU77GdduAWpzJFf8/values/Finance?key=<VITE_GOOGLE_API_KEY>
```

**Resultados esperados:**

✅ **Si funciona**: Verás un JSON con tus datos
```json
{
  "range": "Finance!A1:Z1000",
  "majorDimension": "ROWS",
  "values": [...]
}
```

❌ **Si falla**: Verás un error. Posibles errores:

### Error: "API key not valid"
```json
{
  "error": {
    "code": 400,
    "message": "API key not valid. Please pass a valid API key."
  }
}
```
**Solución**: La API Key es incorrecta. Verifica en Google Cloud Console.

### Error: "API has not been used"
```json
{
  "error": {
    "code": 403,
    "message": "Google Sheets API has not been used in project..."
  }
}
```
**Solución**: Habilita Google Sheets API en tu proyecto (ver paso 1).

### Error: "The caller does not have permission"
```json
{
  "error": {
    "code": 403,
    "message": "The caller does not have permission"
  }
}
```
**Solución**: Comparte la hoja como "Cualquiera con el enlace" (ver paso 2).

### Error: "Unable to parse range"
```json
{
  "error": {
    "code": 400,
    "message": "Unable to parse range: Finance"
  }
}
```
**Solución**: La pestaña "Finance" no existe. Verifica el nombre (ver paso 3).

## 6. Redesplegar en EasyPanel

Una vez que confirmes que la URL del paso 5 funciona:

1. Ve a EasyPanel → Tu servicio
2. Ve a **"Settings"** o **"Environment"**
3. Verifica que las 4 variables estén correctas:
   ```
   VITE_GOOGLE_API_KEY=<VITE_GOOGLE_API_KEY>
   VITE_SPREADSHEET_ID=1I5HSsNyutjuaLGR1ruyh5B_rKCuOU77GdduAWpzJFf8
   VITE_SHEET_NAME=Finance
   VITE_SHEET_RANGE=A:Z
   ```
4. Click en **"Redeploy"** o **"Rebuild"**
5. Espera 3-5 minutos

## 7. Verificar logs en EasyPanel

1. Ve a tu servicio en EasyPanel
2. Click en **"Logs"** o **"Console"**
3. Busca mensajes como:
   - ✅ `"Datos actualizados: X transacciones"`
   - ❌ `"No se configuró VITE_GOOGLE_API_KEY"`
   - ❌ `"Error al obtener datos de Google Sheets"`

## 8. Verificar en el navegador

1. Abre tu dashboard: https://ollama-sherman.5n921h.easypanel.host/
2. Presiona **F12** para abrir DevTools
3. Ve a la pestaña **"Console"**
4. Refresca la página (F5)
5. Busca mensajes:
   - ✅ `"Datos actualizados: X transacciones"`
   - ❌ `"No se configuró VITE_GOOGLE_API_KEY"`
   - ❌ `"Error al obtener datos de Google Sheets"`

---

## 🎨 Problema: El diseño se ve diferente

### Posibles causas:

### 1. Logo no aparece
**Causa**: El archivo `public/logo.jpeg` no se copió al build
**Solución**:
1. Verifica que `public/logo.jpeg` exista en el repo
2. Redespliega en EasyPanel

### 2. Colores oscuros en lugar de blancos
**Causa**: Los estilos de Tailwind no se compilaron correctamente
**Solución**:
1. Verifica que `tailwind.config.js` esté en el repo
2. Verifica que `postcss.config.js` esté en el repo
3. Redespliega en EasyPanel

### 3. Gráficos no se ven bien
**Causa**: Recharts no se instaló o hay problemas con dependencias
**Solución**:
1. Revisa los logs de build en EasyPanel
2. Busca errores de npm install
3. Redespliega en EasyPanel

---

## 🔄 Checklist Completo

Marca cada item cuando lo verifiques:

- [ ] Google Sheets API está habilitada en Google Cloud
- [ ] La hoja está compartida como "Cualquiera con el enlace"
- [ ] La pestaña se llama "Finance" (o variable VITE_SHEET_NAME está correcta)
- [ ] La estructura de columnas es correcta
- [ ] La URL de prueba funciona en el navegador
- [ ] Las 4 variables de entorno están en EasyPanel
- [ ] Redespliegue completado
- [ ] Los logs de EasyPanel no muestran errores
- [ ] La consola del navegador no muestra errores
- [ ] El archivo `public/logo.jpeg` existe en el repo

---

## 📞 ¿Aún no funciona?

Si después de seguir todos estos pasos aún no funciona:

1. **Copia el error exacto** de la consola del navegador
2. **Copia los logs** de EasyPanel
3. **Verifica** que la URL de prueba del paso 5 funcione

La mayoría de problemas se resuelven con:
1. Habilitar Google Sheets API
2. Compartir la hoja correctamente
3. Redesplegar en EasyPanel

---

## 🎯 Verificación Final

Para confirmar que todo funciona:

1. ✅ El número de transacciones cambia (no siempre 3)
2. ✅ Los nombres de colaboradores son los de tu hoja, no los de ejemplo
3. ✅ Los datos se actualizan cuando modificas la hoja de Google Sheets
4. ✅ El botón "Actualizar" trae datos nuevos
5. ✅ El logo de Sherman aparece en el header
6. ✅ El fondo es blanco (no oscuro)

Si todos estos puntos se cumplen, ¡tu dashboard está funcionando correctamente! 🎉
