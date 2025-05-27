"Hola buen día, necesito de tu ayuda de forma muy seria. Tengo que codear una DEMO de mi software, esto solo es un adicional y no tiene un peso ni complejidad enorme. Digamos que el profesor del curso de universidad me ha dado un repositorio en GitHub y espera que lo que hemos trabajado en 8 semanas (la primera mitad del curso) en documentación del software (decisiones y tácticas), pueda verse reflejado en el repositorio. Tipo, si dijimos que usaremos react + note + postores para el aplicativo web, que el repo esté levantado eso. Eso sí, bastan con que esté levantado. Por ejemplo el app en react basta con crear el aplicativo y que se muestre el hello world básico, y con las DB igualmente, basta con levantarlas y hacer una quería sencilla.
Solo debes consideras 2 cosas. Primero, te pasaré tanto mi diagrama de contenedores como documentación escala del aplicativo. Y lo segundo, es que debemos hacer ese repo de GitHub… un mono repo (con Nx o el que tu creas más fácil de implementar), en el que podamos meter las cosas como el frontend, backend y 3 Database. Incluso el tema microservicios, tengo entendido que docker es más fácil que kuberntes. Y eso más que nada, necesito que me ayudes con la implementación de lo siguiente:
Monorepo que contenga mi React con Code y express
3 bases de datos
Microservicios mediante docker
API Stripe
API IDP (Elige el mas sencillo porfa)
Inicia con todo lo que debo considerar, paso a paso porfa. Pero no pases todo de golpe porfavor, primero comencemos con una parte, esperas a que termine y vamos con lo otro. Por ejemplo, iniciemos la monorepo y tengamos las configuraciones básicas  + implementar react como app."


De momento estoy en la parte 2 y ya configuré el primer microservicio, me ayudas?

# Paso 2: Configuración de Microservicios Backend

## 1. Generar las Aplicaciones de Microservicios

npx nx g @nx/node:app user-service --framework=express

… y aquí los otros

## 2. Instalar Dependencias Comunes para Backend


## 3. Crear Estructura de Configuración Compartida

Crea el directorio para configuraciones compartidas:
```bash
mkdir -p libs/shared/config
```

Crea el archivo `libs/shared/config/database.js`:
```javascript
// libs/shared/config/database.js
const { Sequelize } = require('sequelize');
const mongoose = require('mongoose');
const redis = require('redis');

// PostgreSQL Connection
const sequelize = new Sequelize(
  process.env.POSTGRES_DB || 'singletone_db',
  process.env.POSTGRES_USER || 'admin',
  process.env.POSTGRES_PASSWORD || 'password',
  {
    host: process.env.POSTGRES_HOST || 'localhost',
    port: process.env.POSTGRES_PORT || 5432,
    dialect: 'postgres',
    logging: false
  }
);

// MongoDB Connection
const connectMongoDB = async () => {
  try {
    await mongoose.connect(
      process.env.MONGODB_URI || 'mongodb://localhost:27017/singletone_music',
      { useNewUrlParser: true, useUnifiedTopology: true }
    );
    console.log('MongoDB connected successfully');
  } catch (error) {
    console.error('MongoDB connection error:', error);
  }
};

// Redis Connection
const redisClient = redis.createClient({
  host: process.env.REDIS_HOST || 'localhost',
  port: process.env.REDIS_PORT || 6379,
  password: process.env.REDIS_PASSWORD || undefined
});

module.exports = {
  sequelize,
  connectMongoDB,
  redisClient
};
```

## 4. Configurar el Microservicio de Usuarios (Ejemplo Base)

Modifica `apps/user-service/src/main.ts`:

```typescript
import express from 'express';
import cors from 'cors';
import helmet from 'helmet';
import morgan from 'morgan';
import dotenv from 'dotenv';

dotenv.config();

const app = express();
const PORT = process.env.USER_SERVICE_PORT || 3001;

// Middlewares
app.use(helmet());
app.use(cors());
app.use(morgan('combined'));
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Health Check
app.get('/health', (req, res) => {
  res.json({ 
    service: 'user-service',
    status: 'healthy',
    timestamp: new Date().toISOString(),
    version: '1.0.0'
  });
});

// Routes básicas según tu documentación
app.get('/api/users', (req, res) => {
  res.json({ 
    message: 'Gestión de Usuarios - Demo Ready',
    endpoints: [
      'POST /api/users/register',
      'POST /api/users/login', 
      'GET /api/users/profile',
      'PUT /api/users/profile'
    ]
  });
});

app.post('/api/users/register', (req, res) => {
  res.json({ 
    message: 'Usuario registrado exitosamente (Demo)',
    userId: 'demo-user-' + Date.now()
  });
});

app.post('/api/users/login', (req, res) => {
  res.json({ 
    message: 'Login exitoso (Demo)',
    token: 'demo-jwt-token-' + Date.now(),
    user: { id: 1, email: 'demo@singletone.com' }
  });
});

// Error handling
app.use((err: any, req: any, res: any, next: any) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});

app.listen(PORT, () => {
  console.log(`🔐 User Service running on port ${PORT}`);
  console.log(`📍 Health check: http://localhost:${PORT}/health`);
});
```

## 5. Configurar Puertos para cada Microservicio

Crea el archivo `.env` en la raíz del proyecto:
```bash
# Database Configuration
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_DB=singletone_db
POSTGRES_USER=admin
POSTGRES_PASSWORD=password

MONGODB_URI=mongodb://localhost:27017/singletone_music
REDIS_HOST=localhost
REDIS_PORT=6379

# Microservices Ports
USER_SERVICE_PORT=3001
PROFILE_SERVICE_PORT=3002
MUSIC_SERVICE_PORT=3003
LIBRARY_SERVICE_PORT=3004
RECOMMENDATION_SERVICE_PORT=3005
PLANS_SERVICE_PORT=3006

# External APIs
STRIPE_SECRET_KEY=sk_test_your_stripe_key
JWT_SECRET=your_jwt_secret_key
```

## Próximos Pasos

Una vez que confirmes que el primer microservicio funciona:
1. Configuraremos los otros 5 microservicios
2. Setup de las bases de datos con Docker
3. Configuración de Docker Compose
