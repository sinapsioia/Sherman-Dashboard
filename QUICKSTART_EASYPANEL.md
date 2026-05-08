# 🚀 Quick Start - Despliegue en EasyPanel

## ✅ Resumen de lo que ya tienes listo:

1. ✅ **Código subido a GitHub**: https://github.com/jpchacon09/Sherman-Dashboard
2. ✅ **Dockerfile configurado** con multi-stage build
3. ✅ **Nginx optimizado** para servir la aplicación
4. ✅ **Build script** para inyectar variables de entorno
5. ✅ **Documentación completa** en DEPLOY_EASYPANEL.md

## 📝 Checklist Pre-Despliegue

Antes de empezar, asegúrate de tener:

- [ ] Cuenta activa en [EasyPanel](https://easypanel.io/)
- [ ] Google Cloud API Key (con Google Sheets API habilitada)
- [ ] ID de tu Google Spreadsheet
- [ ] Nombre de la pestaña donde están tus datos (ej: "Finance")

## 🎯 Pasos Rápidos (5 minutos)

### 1. Accede a EasyPanel
```
https://easypanel.io/
```

### 2. Crea un Nuevo Servicio
- Click en **"New Service"** o **"+ Create"**
- Selecciona **"GitHub App"** o **"GitHub Repository"**

### 3. Conecta el Repositorio
- Autoriza EasyPanel en GitHub si es necesario
- Busca y selecciona: **`Sherman-Dashboard`**
- Rama: **`main`**

### 4. Configura Build Method
- **Build Method**: `Docker`
- **Dockerfile Path**: `./Dockerfile` (detectado automáticamente)
- **Context**: `.` (raíz del proyecto)

### 5. Agrega Variables de Entorno

**IMPORTANTE**: Configura estas 4 variables:

```env
VITE_GOOGLE_API_KEY=<VITE_GOOGLE_API_KEY>
VITE_SPREADSHEET_ID=1I5HSsNyutjuaLGR1ruyh5B_rKCuOU77GdduAWpzJFf8
VITE_SHEET_NAME=Finance
VITE_SHEET_RANGE=A:Z
```

#### ¿Dónde obtener cada una?

**VITE_GOOGLE_API_KEY**:
1. Google Cloud Console → Tu proyecto
2. APIs & Services → Credentials
3. Copia tu API Key existente o crea una nueva

**VITE_SPREADSHEET_ID**:
- De la URL de tu Google Sheet:
  ```
  https://docs.google.com/spreadsheets/d/[ESTE_ES_EL_ID]/edit
  ```

**VITE_SHEET_NAME**:
- Nombre de la pestaña (tab) en tu Google Sheet
- Ejemplo: `Finance`, `DATA`, `Transacciones`, etc.

**VITE_SHEET_RANGE**:
- Rango de columnas a leer
- Valor recomendado: `A:Z` (lee todas las columnas)

### 6. Configura Puerto y Red
- **Port**: `80` (ya está en el Dockerfile)
- **Protocol**: `HTTP`

### 7. Deploy!
- Click en **"Deploy"** o **"Create & Deploy"**
- Espera 3-5 minutos...
- ☕ Toma un café mientras se construye

## 🎉 Post-Despliegue

### Verificación
1. EasyPanel te dará una URL (ej: `sherman-dashboard.easypanel.host`)
2. Abre la URL en tu navegador
3. Deberías ver el logo de Sherman y el dashboard cargando
4. Los datos se sincronizarán desde Google Sheets automáticamente

### Primera Vez
Si ves el mensaje "Cargando datos de Sherman...":
- Es normal la primera vez
- Espera 5-10 segundos
- Si persiste, revisa las variables de entorno

### Verificar Logs
En EasyPanel:
1. Ve a tu servicio
2. Click en **"Logs"** o **"Console"**
3. Busca errores de API Key o Spreadsheet ID

## 🔧 Troubleshooting Común

### ❌ "Build Failed"
**Solución**:
- Verifica que las 4 variables de entorno estén configuradas
- Revisa los logs de build en EasyPanel
- Asegúrate de que el repo esté actualizado

### ❌ "Cannot read Google Sheets"
**Solución**:
- Verifica que Google Sheets API esté habilitada en Google Cloud
- Confirma que la API Key sea correcta
- Asegúrate de que tu Google Sheet esté compartido como "Cualquiera con el enlace"

### ❌ Dashboard carga pero sin datos
**Solución**:
1. Abre la consola del navegador (F12)
2. Ve a la pestaña "Console"
3. Busca errores relacionados con Google Sheets
4. Verifica el nombre de la hoja (VITE_SHEET_NAME)

### ❌ Error 404 en rutas
**Solución**:
- Esto no debería pasar, el nginx.conf ya está configurado
- Si ocurre, verifica que nginx.conf esté en la raíz del proyecto

## 🔄 Actualizaciones Futuras

### Despliegue Automático
Para activar redespliegues automáticos:

1. En EasyPanel → Tu servicio → Settings
2. Busca "Git" o "GitHub"
3. Activa **"Auto Deploy"** o **"Deploy on Push"**
4. Ahora cada push a `main` desplegará automáticamente

### Despliegue Manual
```bash
# En tu máquina local
cd "/Users/jpchacon/Sherman Dashboard"
git add .
git commit -m "Tu mensaje de commit"
git push origin main

# Luego en EasyPanel
# Click en "Redeploy" o espera el auto-deploy
```

## 🌐 Dominio Personalizado (Opcional)

Para usar tu propio dominio:

1. En EasyPanel → Tu servicio → **Domains**
2. Click en **"Add Domain"**
3. Ingresa tu dominio: `dashboard.sherman.com`
4. Copia los valores DNS que te da EasyPanel
5. Configura los DNS en tu proveedor de dominio
6. Espera 10-30 minutos para propagación
7. EasyPanel generará SSL automáticamente

## 📊 Lo que verás una vez desplegado

✅ Dashboard completo funcionando 24/7
✅ Sincronización automática cada 30 segundos
✅ Logo de Sherman en el header
✅ Fondo blanco profesional
✅ Gráficos de velocímetro para metas de colaboradores
✅ Simuladores "What If" interactivos
✅ Filtros avanzados por colaborador, fecha, servicio
✅ Análisis diario de ingresos y gastos
✅ Todos los gráficos y métricas
✅ HTTPS automático
✅ Responsive (funciona en móvil)

## 🔗 Enlaces Útiles

- **Repositorio**: https://github.com/jpchacon09/Sherman-Dashboard
- **Guía Completa**: [DEPLOY_EASYPANEL.md](./DEPLOY_EASYPANEL.md)
- **EasyPanel**: https://easypanel.io/
- **Google Cloud Console**: https://console.cloud.google.com/

## 💡 Tips Pro

1. **Copia de seguridad**: Mantén una copia de tus variables de entorno en un lugar seguro
2. **Monitoreo**: EasyPanel te envía notificaciones si hay problemas
3. **Performance**: El dashboard está optimizado con cache y compresión gzip
4. **Seguridad**: Nunca compartas tu API Key públicamente
5. **Testing**: Puedes probar localmente con `docker build` antes de desplegar

## 📞 Siguiente Paso

**¡Ve ahora a EasyPanel y despliega!** 🚀

Todo está listo para que simplemente sigas estos pasos y en 5 minutos tengas tu dashboard en vivo.

---

**¿Necesitas la guía detallada?** → Lee [DEPLOY_EASYPANEL.md](./DEPLOY_EASYPANEL.md)

**¡Buena suerte con tu despliegue!** 🎉
