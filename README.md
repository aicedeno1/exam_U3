# IVA Calculator - Opción 2

Aplicación web completa que calcula el IVA (21%) de 5 productos. Incluye Backend (Node.js), Frontend (HTML/CSS/JS) y Base de Datos (MongoDB).

## 📁 Estructura del Proyecto

```
exam_U3/
├── backend/                      # Backend Express + MongoDB
│   ├── server.js                # Servidor principal
│   ├── package.json             # Dependencias backend
│   └── .env.example             # Variables de entorno (template)
│
├── frontend/                     # Frontend estático
│   ├── index.html               # Página principal
│   ├── style.css                # Estilos
│   ├── script.js                # Lógica del cliente
│   └── package.json             # Info del frontend
│
├── DEPLOYMENT_GUIDE.md          # Guía paso a paso para Render
├── .gitignore                   # Archivos a ignorar en Git
└── README.md                    # Este archivo
```

## 🚀 Inicio Rápido (Local)

### 1. Instalar Dependencias

```bash
# Backend
cd backend
npm install

# Frontend (no necesita npm, es estático)
```

### 2. Configurar MongoDB Local

Asegúrate de que MongoDB esté corriendo en `localhost:27017`

```bash
# En Windows (si instalaste MongoDB)
mongod
```

### 3. Iniciar Backend

```bash
cd backend
npm start
```

Backend correrá en `http://localhost:3000`

### 4. Iniciar Frontend

```bash
cd frontend
# Opción 1: Usar Python
python -m http.server 3001

# Opción 2: Usar Node (si tienes)
npx http-server -p 3001
```

Frontend estará en `http://localhost:3001`

## 🌐 Desplegar a Render (Nube)

Para una guía **paso a paso completa**, ver [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md)

Resumen rápido:
1. Crear base de datos en MongoDB Atlas
2. Subir código a GitHub
3. Desplegar backend en Render (Web Service)
4. Desplegar frontend en Render (Static Site)
5. Conectar con variables de entorno

## 📊 Características

### Backend
- ✅ API REST con Express.js
- ✅ Cálculo de IVA (21%)
- ✅ Almacenamiento en MongoDB
- ✅ CORS habilitado
- ✅ Endpoint de health check

### Frontend
- ✅ Interfaz intuitiva y responsiva
- ✅ Formulario para 5 productos
- ✅ Validación de datos
- ✅ Muestra **solo el IVA** (principal)
- ✅ Historial de cálculos
- ✅ Conecta con API backend

### Base de Datos
- ✅ MongoDB con Mongoose
- ✅ Colección: `ivaCalculations`
- ✅ Almacena productos, totales e IVA

## 🔧 API Endpoints

### Health Check
```
GET /api/health
Response: { status: "Backend is running" }
```

### Calcular IVA
```
POST /api/calculate-iva

Body (JSON):
{
  "products": [
    { "name": "Product A", "price": 100 },
    { "name": "Product B", "price": 150 },
    ...
  ]
}

Response:
{
  "success": true,
  "data": {
    "totalPrice": 650.50,
    "ivaRate": 21,
    "ivaAmount": 136.605,
    "finalPrice": 787.105,
    "savedId": "..."
  }
}
```

### Obtener Todos los Cálculos
```
GET /api/calculations

Response:
{
  "success": true,
  "data": [
    { calculation1 }, 
    { calculation2 }, 
    ...
  ]
}
```

### Obtener un Cálculo
```
GET /api/calculation/:id

Response:
{
  "success": true,
  "data": { calculation }
}
```

## 📋 Fórmula del IVA

```
Total Precio = Suma de los 5 productos
IVA Total = Total Precio × 0.21 (21%)
Precio Final = Total Precio + IVA Total
```

**Ejemplo:**
- 5 productos: Milk ($13) + Coffee ($13) + Tea ($10) + Chocolate ($8) + Sugar ($5)
- Total: $49.00
- IVA (21%): $10.29 ← **Solo esto se muestra al usuario**
- Precio Final: $59.29

## 🧪 Testeo

### Con Postman (Backend)

1. Abre Postman
2. **POST** a `http://localhost:3000/api/calculate-iva` (local) o `https://your-backend.onrender.com/api/calculate-iva` (production)
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
  "products": [
    { "name": "Producto A", "price": 100 },
    { "name": "Producto B", "price": 150 },
    { "name": "Producto C", "price": 200 },
    { "name": "Producto D", "price": 75.50 },
    { "name": "Producto E", "price": 125 }
  ]
}
```

### En el Navegador (Frontend)

1. Abre `http://localhost:3001` (local) o tu URL de Render
2. Ingresa 5 productos con nombres y precios
3. Click en "Calculate IVA"
4. Verás el resultado del IVA
5. El historial aparece abajo

## 🔐 Variables de Entorno

### Backend (.env)
```
PORT=3000
MONGODB_URI=mongodb://localhost:27017/exam_iva
FRONTEND_URL=http://localhost:3001
```

### En Render
```
PORT=3000
MONGODB_URI=mongodb+srv://admin:PASSWORD@cluster0.xxxxx.mongodb.net/exam_iva?retryWrites=true&w=majority
FRONTEND_URL=https://exam-iva-frontend.onrender.com
```

## 📚 Tecnologías Utilizadas

| Stack | Tecnología |
|-------|-----------|
| **Backend** | Node.js, Express.js, Mongoose |
| **Frontend** | HTML5, CSS3, Vanilla JavaScript |
| **Base de Datos** | MongoDB |
| **Hosting** | Render.com |
| **Control de Versiones** | Git/GitHub |

## ✅ Checklist Antes de Subir

- [ ] Backend probado con Postman
- [ ] Frontend probado en navegador
- [ ] MongoDB conectada correctamente
- [ ] Variables de entorno configuradas
- [ ] Código subido a GitHub
- [ ] Backend deplorado en Render
- [ ] Frontend deplorado en Render
- [ ] CORS habilitado en Render
- [ ] URLs conectadas correctamente
- [ ] Screenshots capturados para documentación

## 📝 Documentación Adicional

- [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md) - Guía completa Render
- [MongoDB Atlas Docs](https://docs.atlas.mongodb.com/)
- [Render Docs](https://render.com/docs/)
- [Express.js Docs](https://expressjs.com/)

## 📞 Soporte

Si tienes problemas:

1. Revisa los logs en Render Dashboard
2. Verifica las variables de entorno
3. Comprueba que MongoDB está accesible
4. Usa Postman para debuggear la API
5. Abre la consola del navegador (F12) en Frontend

## 👨‍💻 Autor

Exam Option 2 - IVA Calculator
Febrero 2026

---

**Ready to deploy? See [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md)** 🚀
