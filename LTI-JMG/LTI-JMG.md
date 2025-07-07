#ChatGPT 4.0
# LTI - Applicant Tracking System (ATS) del Futuro

## 1. Descripción breve del software LTI

LTI es una solución SaaS de nueva generación orientada a revolucionar la forma en que las empresas gestionan el reclutamiento de talento. Es un sistema ATS (Applicant Tracking System) centrado en la eficiencia, colaboración y automatización. LTI proporciona una experiencia fluida tanto para equipos de Recursos Humanos como para managers y candidatos.

### Valor añadido y ventajas competitivas

- **Colaboración en tiempo real** entre reclutadores y managers
- **Automatización de tareas** repetitivas como filtrado de CVs, emails o programación de entrevistas
- **IA integrada** para recomendaciones de candidatos, análisis de soft skills, predicción de rotación
- **Panel único y personalizable** para cada usuario
- **API-first**: integración sencilla con LinkedIn, InfoJobs, Google Calendar, Slack, Teams, etc.
- **Seguridad y cumplimiento**: GDPR-ready, con permisos granulares y auditoría de acceso

## 2. Funciones principales

- Publicación multicanal de ofertas de empleo
- Recogida y parsing automático de CVs
- Motor de búsqueda semántica de candidatos
- Bandeja de entrada colaborativa
- Gestión de entrevistas, notificaciones y calendario
- Sistema de puntuación y ranking de candidatos
- Generación de informes de rendimiento del funnel de selección

## 3. Lean Canvas

| Sección | Descripción |
|--------|------------|
| Problemas | Proceso de selección lento, colaboraciones fragmentadas, exceso de tareas manuales |
| Segmentos de cliente | Startups, pymes, consultoras de selección, departamentos de RRHH |
| Propuesta de valor | ATS colaborativo, con IA, fácil de usar e integrable |
| Solución | Plataforma web con automatizaciones, integraciones y analítica avanzada |
| Canales | Ventas directas, marketing digital, ferias de empleo y HRtech |
| Ingresos | Suscripción mensual por número de empleados, modelo freemium inicial |
| Costes | Desarrollo, cloud, atención al cliente, marketing |
| Métricas clave | Tasa de conversión de ofertas, tiempo medio de contratación, uso de funcionalidades |
| Ventaja competitiva | IA embebida, enfoque colaborativo en tiempo real, UX de alta calidad |

## 4. Casos de uso principales

### 4.1 Publicar una oferta de empleo

**Actores:** Reclutador

**Descripción:** El reclutador redacta una oferta desde una plantilla, elige los canales de difusión y la publica. LTI se encarga de distribuirla automáticamente.

**Diagrama:**
```
[Reclutador] --> (Crear oferta)
(Crear oferta) --> (Seleccionar canales)
(Seleccionar canales) --> (Publicar oferta)
(Publicar oferta) --> [LTI System]
```

### 4.2 Gestionar candidatos

**Actores:** Reclutador, Manager

**Descripción:** Visualizan los candidatos en cada fase del proceso, añaden notas, etiquetas, y planifican entrevistas. Pueden colaborar comentando directamente en cada perfil.

**Diagrama:**
```
[Reclutador] --> (Ver candidatos)
(Ver candidatos) --> (Evaluar perfil)
[Manager] --> (Comentar candidato)
(Evaluar perfil) --> (Mover a siguiente fase)
```

### 4.3 Filtrado inteligente con IA

**Actores:** Sistema (IA)

**Descripción:** LTI analiza los CVs, experiencia, skills y soft skills de cada candidato, y sugiere los más adecuados según el perfil de la oferta.

**Diagrama:**
```
[LTI IA] --> (Analizar CVs)
(Analizar CVs) --> (Evaluar perfil VS oferta)
(Evaluar perfil VS oferta) --> (Proponer candidatos destacados)
```

## 5. Modelo de datos

### Entidades y atributos principales

#### Usuario
- id: UUID
- nombre: string
- email: string
- rol: enum [Reclutador, Manager, Admin]

#### Oferta
- id: UUID
- titulo: string
- descripcion: text
- fecha_publicacion: date
- estado: enum [borrador, publicada, cerrada]
- canal: string[]
- id_usuario_creador: FK -> Usuario

#### Candidato
- id: UUID
- nombre: string
- email: string
- cv_url: string
- puntuacion_IA: float
- estado_proceso: enum [nuevo, en revisión, entrevista, rechazado, contratado]
- id_oferta: FK -> Oferta

#### Comentario
- id: UUID
- texto: text
- autor: FK -> Usuario
- id_candidato: FK -> Candidato

### Relaciones
- Un Usuario puede crear muchas Ofertas
- Una Oferta tiene muchos Candidatos
- Un Candidato puede tener muchos Comentarios

## 6. Diseño del sistema (alto nivel)

### Componentes principales

- **Frontend Web (Angular/React)**
- **Backend REST (FastAPI/Django)**
- **Módulo de IA (Python + MLlib propia)**
- **Servicio de parsing de CVs (API externa)**
- **Motor de notificaciones (email, Slack, etc.)**
- **Base de datos SQL + motor de búsqueda (PostgreSQL + ElasticSearch)**

### Diagrama de alto nivel (simplificado)
```
[Usuario] --> [Frontend Web] --> [API Gateway]
                                 |
         +-----------------------+------------------+
         |                       |                  |
  [Backend REST]      [Módulo IA]      [Parsing CVs API]
         |                       |
  [PostgreSQL]         [ElasticSearch]
         |
   [Sistema de notificaciones]
```

## 7. Diagrama C4 (Nivel 3: Componente "Filtrado con IA")

### Contenedor: Backend (FastAPI)
### Componente: Motor de recomendación IA

**Responsabilidad:** Analiza candidatos en base a soft skills, experiencia, similitud con la oferta y feedback anterior

**Tecnologías:** Python, scikit-learn, pandas, PostgreSQL

```
+--------------------------+
| Módulo IA: Recomendador |
+--------------------------+
| - analizar_cv(cv)        |
| - calcular_match(oferta, candidato) |
| - sugerir_top_N(oferta)  |
+--------------------------+
        |
        v
+------------------+
| DB Candidatos    |
| DB Feedback      |
+------------------+
```

Este componente se comunica con:
- API interna del backend (REST)
- ElasticSearch para buscar por similitud semántica
- DB para feedback histórico
