# Principales Preocupaciones de la Arquitectura - Singletone
- [Volver al índice](/9/9.md)

A continuación, como parte de uno de los ingresos más importantes para el uso de la metodología ADD 3.0, el grupo decidió listar en base a 8 categorías las preocupaciones principales del software Singletone a nivel arquitectura. Se listó entre 1 a 3 preocupaciones por cada una de las categorías mediante un título y una descomposición basada en "Preocupación", "Impacto" y "Riesgo. De esta forma, se obtiene los puntos críticos que debemos solventar primero durante el despliegue inicial del aplicativo. El desarrollo fue el siguiente.

## 1. Escalabilidad y Rendimiento

### Gestión de Catálogo Musical
- **Preocupación**: El sistema debe manejar un extenso catálogo de artistas, álbumes y canciones con búsquedas eficientes
- **Impacto**: MongoDB con índices denormalizados y Redis como caché de autocompletado
- **Riesgo**: Latencia en búsquedas complejas y crecimiento exponencial del catálogo

### Paginación y Búsqueda
- **Preocupación**: Implementar paginación cursor-based para evitar problemas de rendimiento con skip/limit
- **Impacto**: Experiencia de usuario fluida en exploración musical
- **Riesgo**: Complejidad en la implementación de cursores y sincronización con caché

## 2. Coordinación entre Microservicios

### Comunicación Síncrona REST
- **Preocupación**: Coordinación entre 6 microservicios mediante API REST puede generar latencia acumulativa
- **Impacto**: Flujo principal de valoración requiere coordinación entre Exploración, Biblioteca y Perfil
- **Riesgo**: Fallas en cascada y timeouts en operaciones críticas

### Gestión de Estado Distribuido
- **Preocupación**: Mantener consistencia de estados "agregado" y "valorado" entre servicios
- **Impacto**: Sincronización de biblioteca personal y notificaciones en tiempo real
- **Riesgo**: Estados inconsistentes entre microservicios

## 3. Modelo de Datos Híbrido

### Persistencia Multi-Base
- **Preocupación**: Uso de PostgreSQL, MongoDB y Redis requiere sincronización de datos
- **Impacto**: Cada microservicio maneja su propia base de datos según su dominio
- **Riesgo**: Complejidad en transacciones distribuidas y backup/recovery

### Desnormalización Controlada
- **Preocupación**: Documentos embebidos en MongoDB para optimizar consultas vs. normalización
- **Impacto**: Mejora rendimiento en búsquedas jerárquicas (artista > álbum > canción)
- **Riesgo**: Inconsistencias en actualizaciones y crecimiento de documentos

## 4. Integración con Servicios Externos

### Autenticación con Auth0
- **Preocupación**: Dependencia crítica de IdP externo para toda la autenticación
- **Impacto**: Simplifica gestión de usuarios pero introduce punto de falla externo
- **Riesgo**: Indisponibilidad del servicio afecta toda la plataforma

### Procesamiento de Pagos con Stripe
- **Preocupación**: Flujo de conversión premium depende de servicio externo
- **Impacto**: Habilitación/deshabilitación de funcionalidades premium en tiempo real
- **Riesgo**: Fallas en pagos pueden afectar modelo de negocio

### Recomendaciones con Hugging Face
- **Preocupación**: Latencia y disponibilidad de API de LLM para recomendaciones
- **Impacto**: Funcionalidad diferenciadora para usuarios premium
- **Riesgo**: Costos variables y limitaciones de rate limiting

## 5. Diferenciación de Planes

### Límites de Valoración
- **Preocupación**: Control granular de límites mensuales para usuarios gratuitos
- **Impacto**: Redis con TTL para contadores y WebSockets para notificaciones
- **Riesgo**: Pérdida de contadores por expiraciones o fallas de Redis

### Funcionalidades Premium
- **Preocupación**: Activación/desactivación dinámica de features según tipo de plan
- **Impacto**: Verificación de permisos en cada operación crítica
- **Riesgo**: Bypass de restricciones o inconsistencias en verificaciones

## 6. Comunicación en Tiempo Real

### Notificaciones de Límites
- **Preocupación**: Informar al usuario sobre límites de valoración mediante WebSockets
- **Impacto**: Mejor experiencia de usuario y transparencia del modelo freemium
- **Riesgo**: Gestión de conexiones WebSocket y escalabilidad

### Actualización de Estados
- **Preocupación**: Sincronización en tiempo real de valoraciones completadas
- **Impacto**: Habilitación dinámica del botón "guardar" en valoración de álbumes
- **Riesgo**: Pérdida de eventos o desincronización entre frontend y backend

## 7. Seguridad y Confiabilidad

### Validación de Datos
- **Preocupación**: Validación consistente de datos de usuario y prevención de duplicados
- **Impacto**: Integridad de datos en múltiples bases de datos
- **Riesgo**: Inyección de datos maliciosos o corrupción de información

### Manejo de Tokens JWT
- **Preocupación**: Gestión segura de tokens entre microservicios
- **Impacto**: Autorización en cada endpoint de los 6 microservicios
- **Riesgo**: Tokens expirados, compromiso de seguridad o autorización incorrecta

## 8. Complejidad de Despliegue

### Orquestación con Kubernetes
- **Preocupación**: Coordinación de 6 microservicios, 3 bases de datos y servicios externos
- **Impacto**: Configuración compleja de networking, secrets y persistent volumes
- **Riesgo**: Dependencias circulares y tiempo de startup de servicios

### Monitoreo y Observabilidad
- **Preocupación**: Trazabilidad de requests a través de múltiples servicios
- **Impacto**: Debugging complejo en flujos que abarcan varios microservicios
- **Riesgo**: Dificultad para identificar cuellos de botella y puntos de falla

## Priorización de Preocupaciones para ADD 3.0

1. **Crítica**: Coordinación entre microservicios y gestión de estado distribuido
2. **Alta**: Escalabilidad de búsquedas y modelo de datos híbrido
3. **Media**: Integración con servicios externos y diferenciación de planes
4. **Baja**: Comunicación en tiempo real y complejidad de despliegue