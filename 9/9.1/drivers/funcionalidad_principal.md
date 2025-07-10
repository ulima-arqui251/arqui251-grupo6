# Justificación de la Funcionalidad Primaria - Singletone

## Definición de la Funcionalidad Primaria

**Flujo Principal**: Búsqueda → Visualizar Álbum → Valorar Álbum → Mostrar en Perfil

## Justificación de Selección

### 1. Alineación con el Propósito del Sistema

**Singletone** se define como una plataforma de valoración musical que permite a los usuarios construir y compartir su biblioteca personal de álbumes valorados. La funcionalidad primaria seleccionada encapsula completamente este propósito:

- **Búsqueda**: Punto de entrada para el descubrimiento musical
- **Visualización**: Exploración detallada del contenido antes de la valoración
- **Valoración**: Actividad core que genera valor para el usuario
- **Perfil**: Resultado tangible y compartible de la actividad del usuario

### 2. Representatividad Arquitectónica

Esta funcionalidad primaria ejercita los componentes más críticos del sistema:

#### Cobertura de Microservicios (4 de 6)
- **Exploración Musical**: Búsqueda con filtros y visualización de álbum
- **Gestión de Biblioteca**: Proceso de valoración y guardado
- **Visualización de Perfil**: Mostrar álbumes valorados
- **Gestión de Usuarios**: Autenticación requerida en todo el flujo

#### Utilización del Stack Tecnológico Completo
- **MongoDB**: Consultas complejas de catálogo musical
- **PostgreSQL**: Gestión de relaciones usuario-álbum
- **Redis**: Caché de búsquedas y contadores de límites
- **Auth0**: Autenticación en cada paso del flujo

### 3. Validación de Decisiones Arquitectónicas

#### Comunicación Síncrona REST
El flujo valida la decisión de usar comunicación síncrona entre microservicios:
- Búsqueda → Exploración Musical
- Valoración → Gestión de Biblioteca → Notificación a Perfil
- Verificación de límites → Gestión de Planes

#### Modelo de Datos Híbrido
- **MongoDB**: Búsqueda eficiente en catálogo desnormalizado
- **PostgreSQL**: Consistencia en relaciones de valoración
- **Redis**: Performance en verificación de límites

### 4. Casos de Uso de Mayor Valor

#### Para Usuarios Gratuitos
- Representa el 80% de la actividad esperada
- Demuestra el valor del producto antes de conversión premium
- Ejercita límites de valoración (modelo freemium)

#### Para Usuarios Premium
- Flujo base para valoraciones ilimitadas
- Fundamento para funcionalidades premium adicionales (listas, recomendaciones)

### 5. Complejidad Representativa

#### Aspectos Técnicos Validados
- **Paginación cursor-based**: En resultados de búsqueda
- **Estados distribuidos**: "agregado" vs "valorado" entre servicios
- **Comunicación tiempo real**: Notificaciones de límites via WebSockets
- **Validación de permisos**: Verificación de tipo de plan en cada valoración

#### Flujos de Error Críticos
- Falla en autenticación (Auth0)
- Límite de valoraciones alcanzado
- Timeout entre microservicios
- Inconsistencia en estados de álbum

### 6. Impacto en Calidad y Rendimiento

#### Atributos de Calidad Ejercitados
- **Performance**: Búsquedas con índices denormalizados
- **Usabilidad**: Flujo intuitivo de 4 pasos
- **Confiabilidad**: Consistencia de datos entre servicios
- **Security**: Autenticación en cada operación

#### Métricas Clave Validadas
- Tiempo de respuesta en búsquedas (<500ms)
- Latencia acumulativa entre servicios (<2s)
- Consistencia de estados distribuidos (>99.5%)

### 7. Escalabilidad del Flujo

#### Crecimiento de Usuarios
- Búsquedas concurrentes con caché Redis
- Distribución de carga entre microservicios
- Gestión eficiente de sesiones con Auth0

#### Crecimiento de Catálogo
- Índices optimizados en MongoDB
- Paginación eficiente sin degradación
- Caché inteligente de resultados frecuentes

### 8. Diferenciación Competitiva

#### Valor Único
- Valoración granular por canción vs. rating simple de álbum
- Biblioteca personal como perfil musical
- Integración transparente de límites freemium

#### Experiencia de Usuario
- Flujo cohesivo end-to-end
- Retroalimentación inmediata en cada paso
- Progreso visible hacia completar valoración

## Cobertura de Requisitos Funcionales

### Requisitos Críticos Validados
- **RF15**: Búsqueda general con filtros
- **RF17**: Vista detallada de álbum  
- **RF23**: Valoración de canciones
- **RF25**: Guardado de valoración completa
- **RF10**: Biblioteca musical en perfil

### Requisitos de Soporte Activados
- **RF24**: Límite de valoraciones menduales
- **RF28**: Notificación de valoraciones restantes
- **RF22**: Estados de artistas y álbumes

## Riesgos Mitigados

### Técnicos
- Validación de coordinación entre 4 microservicios críticos
- Prueba de resistencia del modelo de datos híbrido
- Verificación de patrones de comunicación síncrona

### Negocio
- Demostración del valor del producto en flujo completo
- Validación del modelo freemium con límites
- Fundamento para conversión premium

## Conclusión

La funcionalidad primaria seleccionada (Búsqueda → Visualizar → Valorar → Perfil) constituye la representación más fiel del sistema Singletone, ejercitando los componentes arquitectónicos más críticos y validando las decisiones técnicas fundamentales. Su implementación exitosa garantiza que la arquitectura puede soportar el propósito central del sistema y escalar hacia funcionalidades adicionales.