# Guía de Deployment a Render

## 📋 Requisitos Previos

1. Cuenta GitHub con el repositorio subido
2. Cuenta en [Render.com](https://render.com) (gratis)
3. Cuenta en [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) (gratis)

---

## Paso 1: Preparar MongoDB Atlas (Base de Datos en la Nube)

### 1.1 Crear Cluster en MongoDB Atlas

1. Ve a [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Registrate o inicia sesión
3. Click en "Create Deployment"
4. Selecciona **Free Tier** (M0)
5. Elige tu región
6. Click en "Create Deployment"

### 1.2 Crear Usuario de Base de Datos

1. En el Dashboard, ve a **Database Access**
2. Click en **Add New Database User**
3. Username: `admin`
4. Password: Genera una contraseña fuerte
5. Click en **Add User**

### 1.3 Obtener String de Conexión

1. Ve a **Clusters**
2. Click en **Connect**
3. Selecciona **Drivers**
4. Copia la string de conexión (algo como):
```
mongodb+srv://admin:<PASSWORD>@cluster0.xxxxx.mongodb.net/exam_iva?retryWrites=true&w=majority
```
5. Reemplaza `<PASSWORD>` con tu contraseña
6. **Guarda esta string** - la necesitarás después

### 1.4 Whitelist IP

1. En MongoDB Atlas, ve a **Network Access**
2. Click en **Add IP Address**
3. Selecciona **Allow Access from Anywhere** (0.0.0.0/0)
4. Click en **Confirm**

---

## Paso 2: Subir a GitHub

```bash
cd c:\Users\andre\OneDrive\Desktop\exam_U3\exam_U3

git init
git add .
git commit -m "Initial commit: IVA Calculator app"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/exam_U3.git
git push -u origin main
```

---

## Paso 3: Desplegar Backend en Render

### 3.1 Crear Servicio Web para Backend

1. Ve a [Render.com](https://render.com)
2. Inicia sesión
3. Click en **New +** → **Web Service**
4. Conecta tu repositorio GitHub
5. Completa el formulario:

| Campo | Valor |
|-------|-------|
| **Name** | `exam-iva-backend` |
| **Environment** | `Node` |
| **Build Command** | `cd backend && npm install` |
| **Start Command** | `cd backend && npm start` |
| **Region** | Tu región más cercana |
| **Plan** | `Free` (o Starter) |

### 3.2 Agregar Variables de Entorno

1. Desplázate hasta la sección **Environment Variables**
2. Agrega estas variables:

```
PORT = 3000
MONGODB_URI = mongodb+srv://admin:TU_CONTRASEÑA@cluster0.xxxxx.mongodb.net/exam_iva?retryWrites=true&w=majority
FRONTEND_URL = https://exam-iva-frontend.onrender.com
```

3. Click en **Create Web Service**

### 3.3 Esperar Deployment

- Render desplegará automáticamente tu backend
- Ve a **Logs** para ver el progreso
- Cuando esté listo, verás: `Server is running...`
- Copia la URL del servicio (ejemplo: `https://exam-iva-backend.onrender.com`)

---

## Paso 4: Desplegar Frontend en Render

### 4.1 Crear Servicio Static Site para Frontend

1. En Render, click en **New +** → **Static Site**
2. Conecta tu repositorio GitHub
3. Completa el formulario:

| Campo | Valor |
|-------|-------|
| **Name** | `exam-iva-frontend` |
| **Build Command** | `cd frontend && npm install` |
| **Publish Directory** | `frontend` |
| **Plan** | `Free` |

### 4.2 Configurar Script de Deploy

Antes de desplegar, actualiza `frontend/script.js`:

Reemplaza:
```javascript
const BACKEND_URL = window.location.hostname === 'localhost' 
  ? 'http://localhost:3000' 
  : 'https://your-backend-render-url.com';
```

Con tu URL real de Render:
```javascript
const BACKEND_URL = 'https://exam-iva-backend.onrender.com';
```

### 4.3 Deploy

1. Click en **Create Static Site**
2. Espera a que termine el deployment
3. Copia la URL del sitio (ejemplo: `https://exam-iva-frontend.onrender.com`)

---

## Paso 5: Actualizar CORS en Backend

Vuelve al dashboard de Render y actualiza la variable `FRONTEND_URL`:

```
FRONTEND_URL = https://exam-iva-frontend.onrender.com
```

Render redesplegará automáticamente.

---

## ✅ Pruebas

### Frontend
1. Ve a `https://exam-iva-frontend.onrender.com`
2. Ingresa 5 productos
3. Click en "Calculate IVA"
4. Debes ver el cálculo guardado en MongoDB

### Backend (Postman)
1. URL: `https://exam-iva-backend.onrender.com/api/health`
2. Debes recibir: `{"status":"Backend is running"}`

---

## 🐛 Troubleshooting

### El frontend no conecta al backend
- Verifica que `FRONTEND_URL` en backend sea correcta
- Revisa that CORS está habilitado en `server.js`
- Checa que MongoDB está correctamente conectada

### MongoDB no conecta
- Verifica la contraseña en `MONGODB_URI`
- Asegúrate de que la IP está whitelistada (0.0.0.0/0)
- Comprueba que el nombre de la database es `exam_iva`

### El servidor de Render se apaga
- Los servicios Free pueden hibernar. Haz ping para despertar
- Considera actualizar a Starter ($4/mes)

---

## 📊 Estructura Final en Render

```
Render Services:
│
├── Web Service (Backend)
│   ├── Name: exam-iva-backend
│   ├── URL: https://exam-iva-backend.onrender.com
│   └── Environment: Node.js
│
└── Static Site (Frontend)
    ├── Name: exam-iva-frontend
    ├── URL: https://exam-iva-frontend.onrender.com
    └── Conecta a Backend vía API
```

---

## 📝 Para GitHub

Crea un archivo `.gitignore`:

```
node_modules/
.env
.DS_Store
*.log
```

---

Listo! Tu aplicación está en la nube ☁️
