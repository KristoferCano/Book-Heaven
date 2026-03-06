# 🔧 Error: "cd backend: No such file or directory" - SOLUCIÓN

## ❌ El Problema

```
/bin/bash: line 1: cd backend && composer install...
/bin/bash: No such file or directory
```

Railway está intentando ejecutar el comando desde la **raíz del repositorio**, pero el repositorio tiene estructura:
```
BookHeaven/
├── backend/  ← Aquí está el código PHP
├── frontend/
└── nixpacks.toml
```

## ✅ La Solución

### OPCIÓN 1: Configurar Root Directory en Railway UI (RECOMENDADO)

1. Ve a **[Railway.app](https://railway.app)** → Tu Proyecto
2. Click en tu Servicio PHP
3. **Settings** → **Root Directory**
4. Ingresa: `backend`
5. Click **Save**

Railway ahora sabrá que todo empieza desde `/backend`

### OPCIÓN 2: Alternativa - Usar railway.json

Ya actualicé tu `backend/railway.json` para que funcione correctamente.

---

## 📋 Checklist para Que Funcione

- [ ] Ve a tu Servicio PHP en Railway
- [ ] Abre **Settings**
- [ ] Busca **"Root Directory"** o **"Service Root"**
- [ ] Escribe: `backend`
- [ ] Click **Save/Guardar**
- [ ] Railway se reinicia automáticamente

---

## 🚀 Después de Configurar Root Directory

Railway hará:

1. **Build Phase**
   ```bash
   # Se ejecuta desde /backend/
   composer install --optimize-autoloader --no-dev
   ```

2. **Release Phase**
   ```bash
   php artisan migrate --force
   php artisan cache:clear
   php artisan config:cache
   php artisan route:cache
   ```

3. **Start Phase**
   ```bash
   vendor/bin/heroku-php-apache2 public/ -F fpm_config.ini
   ```

✅ Todo debería funcionar

---

## 📸 Cómo Se Ve en Railway

En Railway Dashboard:
1. Proyecto → PHP Service (el que dice "PHP")
2. Arriba derecha: **"Settings"** ⚙️
3. Baja hasta **"Root Directory"**
4. Escribe `backend`
5. Save

---

## ❌ Si Sigue Fallando

### Error: "still can't find backend"
**Solución:**
- Verifica que el nombre de carpeta sea exactamente `backend` (minúsculas)
- Si está mal escrito en Railway, corrígelo

### Error: "No composer.json"
**Solución:**
- Verifica que `backend/composer.json` exista
- Revisa que el Root Directory sea `backend`

### Error: "Docker build failed" nuevamente
**Solución:**
1. Ve a **Deployment** → última build
2. Haz click en **View Logs**
3. Busca la línea de error exacta
4. Cópiala y mándame para ayudarte

---

## ✅ Archivos que ya hice commit

- ✅ `nixpacks.toml` - Configuración corregida
- ✅ `backend/railway.json` - Simplificado
- ✅ `backend/Procfile` - Procesos

**Los cambios ya están en GitHub**, pero falta que **configures el Root Directory manualmente en Railway**. No se puede hacer automáticamente porque es una configuración de la UI.

---

## 🔄 Resumen

1. **GitHub**: ✅ Cambios ya subidos
2. **Railway Settings**: ⏳ TÚ HACES ESTO (Root Directory = `backend`)
3. **Verificar**: Ve a Deployments → Redeploy
4. **Listo**: Debería compilar sin errores 🎉

---

**¿Ya habías entrado al Root Directory en Railroad antes?** Si no sabes dónde es, dimelo y te muestro los pasos exactos con captura.
