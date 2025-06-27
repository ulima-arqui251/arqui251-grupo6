# Lista Priorizada de Requisitos Funcionales - Singletone

## Funcionalidad Crítica (Must Have) - Core del Sistema

| Prioridad | ID | Nombre | Módulo | Justificación |
|-----------|----|---------|---------|--------------| 
| 1 | RF1 | Registro de usuario | Gestión de Usuarios | Registro de usuario básico que permita crear una cuenta |
| 2 | RF3 | Inicio de sesión | Gestión de Usuarios | Acceso básico al sistema, pero que incluirá integración un IDP y tiene un alto impacto en la arquitectura |
| 3 | RF15 | Búsqueda general con filtros | Exploración Musical | **CORE**: Primer paso del flujo principal |
| 4 | RF16 | Visualización de resultados por pestañas | Exploración Musical | **CORE**: Organización de resultados de búsqueda |
| 5 | RF17 | Vista detallada de álbum | Exploración Musical | **CORE**: Segundo paso del flujo principal |
| 6 | RF23 | Valoración de canciones | Gestión de Biblioteca | **CORE**: Tercer paso del flujo principal |
| 7 | RF25 | Guardado de valoración completa | Gestión de Biblioteca | **CORE**: Completar valoración del álbum |
| 8 | RF7 | Visualización de datos básicos | Visualización de Perfiles | **CORE**: Cuarto paso - mostrar en perfil |
| 9 | RF10 | Biblioteca musical en el perfil | Visualización de Perfiles | **CORE**: Mostrar álbumes valorados, el resultado final del flujo|

## Funcionalidad Importante (Should Have) - Soporte Operacional

| Prioridad | ID | Nombre | Módulo | Justificación |
|-----------|----|---------|---------|--------------| 
| 10 | RF2 | Validación de correo y nickname | Gestión de Usuarios | No existen usuarios duplicados |
| 11 | RF4 | Visualización de tipo de cuenta | Gestión de Usuarios | Diferenciación free/premium que almacena el valor en PostgreSQL y Redis |
| 12 | RF5 | Seguridad en mis datos | Gestión de Usuarios | Confianza del usuario a través de medidas de seguridad |
| 13 | RF18 | Vista detallada de artista | Exploración Musical | Ampliamos la búsqueda del usuario |
| 14 | RF22 | Estados de artistas y álbumes | Gestión de Biblioteca | Control de progreso en cuanto a la valoración |
| 15 | RF24 | Límite de valoraciones semanales | Gestión de Biblioteca | Se implementa uno de los factores clave del modelo de negocio |
| 16 | RF26 | Guardado como borrador | Gestión de Biblioteca | Funcionalidad adicional para guardar publicaciones parciales |
| 17 | RF9 | Estadísticas personales | Visualización de Perfiles | Datos básicos que aparecen en la página de perfil |
| 18 | RF20 | Eliminar artistas y álbumes | Gestión de Biblioteca | Complemento al modelo actual, ahora permite la eliminación de los artistas y albums no deseados |

## Funcionalidad Deseable (Could Have) - Mejoras de UX

| Prioridad | ID | Nombre | Módulo | Justificación |
|-----------|----|---------|---------|--------------| 
| 19 | RF19 | Vista detallada de usuario | Exploración Musical | Aumenta la interacción entre usuarios |
| 20 | RF21 | Eliminación en cascada de álbumes | Gestión de Biblioteca | Aplicación de lógica de negocio secundario |
| 21 | RF27 | Reevaluación de canciones | Gestión de Biblioteca | Flexibilidad para el usuario |
| 22 | RF28 | Notificación de valoraciones restantes | Gestión de Biblioteca | Transparencia del límite en base al tipo de suscripción |
| 23 | RF8 | Indicador de límite de valoraciones | Visualización de Perfiles | Debajo del perfil indicará la cantidad de consultas |
| 24 | RF11 | Acceso a todos los artistas | Visualización de Perfiles | Navegación avanzada para resumir todos los artistas |
| 25 | RF12 | Acceso a todos los álbumes | Visualización de Perfiles | Navegación avanzada para resumir todos los álbumes |
| 26 | RF13 | Edición de perfil | Visualización de Perfiles | Edición para alterar datos básicos |

## Funcionalidad Premium (Could Have) - Monetización

| Prioridad | ID | Nombre | Módulo | Justificación |
|-----------|----|---------|---------|--------------| 
| 27 | RF6 | Activación de cuenta premium | Gestión de Usuarios | Modelo de negocio que activa funcionlidades extras |
| 28 | RF36 | Acceso a página de incentivo premium | Gestión de Planes | Conversión a premium con alta importancia para el negocio, pero baja en la arquitectura principal |
| 29 | RF37 | Selección de duración y método de pago | Gestión de Planes | Proceso de compra básico con Stripe |
| 30 | RF38 | Activación de funcionalidades premium | Gestión de Planes | Habilitación de features extras |
| 31 | RF39 | Distintivo visual de cuenta premium | Gestión de Planes | Diferenciación visuales en todas las pantallas |
| 32 | RF29 | Valoraciones ilimitadas para Premium | Gestión de Biblioteca | Desactivación del rastreo de límites para los premium |
| 33 | RF14 | Listas adicionales para usuarios premium | Visualización de Perfiles | Valor agregado de premium |
| 34 | RF30 | Vistas adicionales para el plan Premium | Gestión de Biblioteca | Valor agregado de premium |

## Funcionalidad Avanzada (Won't Have - Fase 1) - Features Secundarias

| Prioridad | ID | Nombre | Módulo | Justificación |
|-----------|----|---------|---------|--------------| 
| 35 | RF31 | Recomendaciones personalizadas | Gestión de Recomendaciones | Feature avanzada - LLMs de Hugging Face |
| 36 | RF32 | Sección destacada en el header | Gestión de Recomendaciones | UX de recomendaciones para los usuario premium |
| 37 | RF33 | Mostrar artista y álbumes sugeridos | Gestión de Recomendaciones | Resultado primordial del módulo de recomendaciones |
| 38 | RF34 | Recomendación automatizada | Gestión de Recomendaciones | Prompt diseñado en base a un cirterio musical previamente establecido |
| 39 | RF35 | Botón de volver a recomendar | Gestión de Recomendaciones | Se aceptan múltiples consultas al LLMs |
| 40 | RF40 | Fecha de vencimiento visible al hacer hover | Gestión de Planes | Detalle de UX para mejorar la presentación del sistema |

---

## Notas para el Desarrollo ADD 3.0

### **Iteración 1 (Semana 1)**: MVP Core
- **RF1-RF9**: Implementar el flujo principal completo
- **Objetivo**: Usuario puede registrarse, buscar, valorar y ver su perfil

### **Iteración 2 (Semana 2)**: Estabilización y Modelo de Negocio  
- **RF10-RF18**: Completar funcionalidades de soporte
- **Objetivo**: Sistema robusto con diferenciación free/premium

### **Iteración 3 (Semana 3)**: Premium y Pulimiento
- **RF19-RF34**: Implementar funcionalidades premium y UX avanzada
- **Objetivo**: Sistema completo listo para producción

### **Backlog Futuro**
- **RF35-RF40**: Features de IA y detalles avanzados de UX