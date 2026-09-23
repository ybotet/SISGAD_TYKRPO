# PRÁCTICA 5-8: REFINAMIENTO DEL ALCANCE DEL PROYECTO (EVENT STORMING, USER STORY MAP Y WBS PARA CLICKUP)

**Asignatura:** Tecnología de Gestión de Equipos de Desarrollo de Software  
**Institución:** RTU MIREA - IPTIP (Cátedra de Programación Industrial)  
**Estudiante:** Yaisel Botet Cesar (Grupo ÉFMO-01-25)  
**Proyecto:** SISGAD5 (Sistema de Información para la Gestión de Incidencias, Servicios y Materiales - ETECSA DAG5)  
**Cliente:** ETECSA - Dirección No. 5  
**Presupuesto:** 810 000 rublos | **Plazo:** 01.10.2025 – 01.03.2026  

---

## 1. INTRODUCCIÓN Y ENFOQUE METODOLÓGICO

El objetivo de las Prácticas 5 a 8 es precisar el alcance técnico y funcional del sistema **SISGAD5** mediante la aplicación integrada de tres metodologías clave de la ingeniería de software moderna:

1. **Domain-Driven Design (DDD) & Event Storming**: Identificación de eventos del negocio, agregados, comandos, políticas de tiempo (SLA) y delimitación de contextos (*Bounded Contexts*).
2. **User Story Mapping (USM)**: Estructuración del viaje del usuario (*User Journey*) por roles, organizando las funcionalidades en Spine (Actividades), User Tasks (Tareas) y Releases (Release 1 - MVP vs. Releases futuros).
3. **Work Breakdown Structure (WBS) & Matriz ClickUp**: Desglose jerárquico del trabajo en 4 niveles con estimación de esfuerzo en horas (total 376h), asignación de responsables, prioridades y dependencias para importación directa en la herramienta de gestión ClickUp.

---

## 2. EVENT STORMING & MODELADO DE DOMINIO (DDD)

### 2.1. Leyenda de Componentes del Event Storming

* 🟧 **Evento de Dominio (Domain Event)**: Algo que ocurrió en el negocio (tiempo pasado verbal).
* 🟦 **Comando (Command)**: Acción detonada por un usuario o sistema para provocar un evento.
* 🟨 **Agregado / Entidad (Aggregate)**: Objeto de negocio que mantiene la consistencia de datos.
* 🟨 **Actor / Rol (Actor)**: Usuario que ejecuta el comando (*Probador*, *Supervisor*, *Editor*, *Admin_Materiales*).
* 🟪 **Política / Regla de Negocio (Policy / SLA)**: Regla automática (ej. timer de SLA de 10 min para diagnóstico o 72h para reparación).
* 🟥 **Sistema Externo (External System)**: Sistema externo integrado (ej. Central Telefónica, Active Directory/LDAP, SAP ERP).
* 🟩 **Modelo de Lectura (Read Model / View)**: Vista de datos que el usuario necesita para tomar una decisión (ej. Dashboard, Tabla de Stock).

---

### 2.2. Mapa de Contextos Delimitados (Bounded Contexts)

El dominio de SISGAD5 se divide en **5 Contextos Delimitados** bien estructurados con lenguaje ubicuo (*Ubiquitous Language*):

1. **Contexto `УправлениеКлиентами` (Gestión de Clientes y Servicios)**:
   - *Agregados*: `Cliente`, `LineaTelefonica`, `Pizarra`.
   - *Responsabilidad*: Mantenimiento de la base de datos de abonados, líneas asociadas y pizarras de distribución.
2. **Contexto `УправлениеЖалобамиИРаботами` (Gestión Operativa MP - Node.js)**:
   - *Agregados*: `Queja`, `Prueba`, `Trabajo`.
   - *Responsabilidad*: Control del ciclo de vida de incidencias (`Abierta` $ightarrow$ `Probada` $ightarrow$ `Asignada` $ightarrow$ `Pendiente` $ightarrow$ `Resuelta` $ightarrow$ `Cerrada`).
3. **Contexto `УправлениеМатериалами` (Gestión de Inventario - Go)**:
   - *Agregados*: `Material`, `StockBrigada`, `ValeDescuento`.
   - *Responsabilidad*: Descuento concurrente transaccional de insumos utilizados en reparaciones mediante *goroutines*.
4. **Contexto `АналитикаИМониторинг` (Analítica y Dashboards)**:
   - *Agregados*: `MétricaSLA`, `ReporteEstadístico`.
   - *Responsabilidad*: Cálculo de MTTR, tasa de resolución de averías y gráficos interactivos con Recharts.
5. **Contexto `БезопасностьИИнфраструктура` (Seguridad y Plataforma)**:
   - *Agregados*: `Usuario`, `Rol`, `AuditoriaLog`.
   - *Responsabilidad*: Autenticación JWT, control RBAC, logs de auditoría y orquestación con Docker Compose.

---

### 2.3. Flujo Cronológico de Eventos de Dominio (26 Eventos)

```
[INICIO: RECEPCIÓN DE QUEJA]
  🟨 Probador 🟦 RegistrarQueja 🟨 Agregado:Queja 🟧 QuejaRegistrada
     ⬇
  🟪 Política: SLA Diagnóstico <10 min
     ⬇
  🟨 Probador 🟦 EjecutarPruebaLinea 🟥 CentralTelefónica 🟨 Agregado:Prueba 🟧 PruebaDiagnósticoCompletada
     ⬇
  🟧 EstadoQuejaCambiadoAProbada
     ⬇
[ASIGNACIÓN DE REPARACIÓN]
  🟨 Supervisor 🟦 AsignarTrabajoATecnico 🟨 Agregado:Trabajo 🟧 TrabajoAsignado
     ⬇
  🟨 TécnicoCampo 🟦 IniciarReparacionEnCampo 🟧 ReparacionEnProceso
     ⬇
[DESCUENTO TRANSACCIONAL DE MATERIALES]
  🟨 TécnicoCampo / AdminMat 🟦 RegistrarMaterialesConsumidos 🟨 Agregado:StockBrigada ⚡ Servicio Go (Goroutines)
     ⬇
  🟧 MaterialesDescontadosTransaccionalmente
     ⬇
  🟪 Política: Si Stock < StockMínimo 🟧 AlertaStockBajoGenerada
     ⬇
[CIERRE Y AUDITORÍA]
  🟨 Supervisor 🟦 ValidarYResolverTrabajo 🟧 TrabajoResuelto
     ⬇
  🟪 Política: SLA Reparación <72h 🟧 QuejaCerradaExitosamente
     ⬇
  🟩 DashboardActualizadoEnTiempoReal 🟧 AuditoriaLogRegistrado
```

---

## 3. USER STORY MAP (USM) Y DEFINICIÓN DE RELEASES

El User Story Map organiza las necesidades de los usuarios a lo largo del flujo de trabajo, dividiendo el alcance en **Release 1 (MVP)** y futuros desarrollos.

### 3.1. Estructura General del User Story Map

```
=================================================================================================================
EPICS / ACTIVIDADES  | 1. Autenticación | 2. Gestión Quejas | 3. Diagnóstico   | 4. Asignación y | 5. Control de   | 6. Dashboards
                    | y Usuarios (RBAC) | y Abonados        | de Línea         | Reparación      | Materiales (Go) | y Reportes
=================================================================================================================
USER TASKS          | • Iniciar Sesión  | • Buscar Abonado  | • Ejecutar Prueba| • Asignar Técnico| • Consultar Stock| • Ver Métricas SLA
(Tareas de Usuario) | • Gestionar Roles | • Crear Queja     | • Ver Historial   | • Modificar Estado| • Descontar Material|• Exportar CSV/JSON
-----------------------------------------------------------------------------------------------------------------
RELEASE 1 (MVP)     | US-01: Login JWT  | US-03: Búsqueda   | US-05: Prueba de | US-07: Asignación| US-09: Descuento| US-11: Dashboard
(Sprints 1 a 4)     | US-02: Roles RBAC | US-04: Registro   |      Línea       |      de Trabajo |      Concurrente|      SLA/MTTR
                    |                   |      de Queja     | US-06: Historial | US-08: Cambiar   | US-10: Alerta de| US-12: Exportación
                    |                   |                   |      Diagnóstico |      Estado     |      Stock Mínimo|      CSV/JSON
-----------------------------------------------------------------------------------------------------------------
RELEASE 2           | US-13: 2FA OAuth  | US-14: Geolocalización | US-15: Pruebas Automáticas | US-16: App Móvil para| US-17: Pedidos de
(Fase Posterior)    |                   | de Averías en Mapa     | programadas periódicas     | Técnicos en Campo    | Reaprovisionamiento
=================================================================================================================
```

---

### 3.2. Detalle de Historias de Usuario del MVP y Definition of Done (DoD)

#### **US-01: Autenticación Segura con JWT y Refresh Tokens**
* **Como** Usuario del sistema (Probador, Supervisor, Editor, Admin_Materiales),
* **Quiero** iniciar sesión con mis credenciales corporativas y recibir tokens JWT seguros,
* **Para** acceder únicamente a las funcionalidades autorizadas según mi rol.
* **Criterios de Aceptación (DoD)**:
  - [x] Validación de credenciales encriptadas con `bcrypt` en PostgreSQL.
  - [x] Retorno de `accessToken` (expira en 15 min) y `refreshToken` (expira en 7 días) almacenado en Redis.
  - [x] Tiempo de respuesta del endpoint `/api/v1/auth/login` $< 200	ext{ ms}$.
  - [x] Bloqueo automático tras 5 intentos fallidos consecutivos.

#### **US-04: Registro e Ingesta de Quejas Telefónicas**
* **Como** Probador de primera línea,
* **Quiero** registrar una nueva queja vinculada al número telefónico o contrato del abonado,
* **Para** iniciar el flujo de atención técnica en el sistema.
* **Criterios de Aceptación (DoD)**:
  - [x] Formulario con autocompletado de datos del cliente al ingresar el teléfono.
  - [x] Generación de un número de ticket único con estado inicial `Abierta`.
  - [x] Registro automático de fecha, hora y usuario probador en la tabla de auditoría.
  - [x] Validación con Zod para evitar campos vacíos o teléfonos inexistentes.

#### **US-05: Pruebas Diagnósticas de Estación y Línea**
* **Como** Probador de estación,
* **Quiero** ejecutar una prueba técnica sobre la línea reportada y guardar los valores de parámetro,
* **Para** clasificar el tipo de avería y cambiar el estado de la queja a `Probada`.
* **Criterios de Aceptación (DoD)**:
  - [x] Cumplimiento del SLA de diagnóstico ($< 10	ext{ min}$ desde la apertura de la queja).
  - [x] Registro de parámetros técnicos (resistencia de aislamiento, voltaje, atenuación).
  - [x] Transición automática del estado de la queja a `Probada`.

#### **US-07: Asignación de Trabajos de Reparación a Técnicos**
* **Como** Supervisor de operaciones,
* **Quiero** crear una orden de trabajo asociada a una queja probada y asignarla a una brigada técnica,
* **Para** que los técnicos reparen la avería en campo.
* **Criterios de Aceptación (DoD)**:
  - [x] Creación de registro en la tabla `Trabajos` vinculado a la `Queja`.
  - [x] Selección de brigada técnica disponible y asignación de prioridad (`Normal`, `Urgente`).
  - [x] Transición del estado de la queja a `Asignada`.

#### **US-09: Descuento Concurrente Transaccional de Materiales (Go)**
* **Como** Mánager de Materiales / Técnico,
* **Quiero** registrar los materiales utilizados en la reparación (ej. metros de cable, conectores),
* **Para** que el servicio en Go descuente transaccionalmente el stock de la brigada en tiempo real.
* **Criterios de Aceptación (DoD)**:
  - [x] Ejecución a través del microservicio *Materials Service* escrito en **Go**.
  - [x] Manejo de peticiones concurrentes mediante *goroutines* y bloqueos de fila (`SELECT FOR UPDATE`).
  - [x] Rechazo de la transacción si el stock solicitado supera la cantidad disponible.
  - [x] Actualización instantánea en la base de datos `bd_materiales`.

#### **US-11: Dashboard Estadístico de SLA y MTTR**
* **Como** Supervisor / Director de ETECSA-DAG5,
* **Quiero** visualizar un dashboard interactivo con gráficos de quejas por estado, tiempo medio de reparación (MTTR) y consumo de insumos,
* **Para** monitorear los indicadores KPI y tomar decisiones informadas.
* **Criterios de Aceptación (DoD)**:
  - [x] Gráficos interactivos renderizados con Recharts/Chart.js en React.
  - [x] Cálculo del MTTR en horas y porcentaje de cumplimiento del SLA ($< 72	ext{ h}$).
  - [x] Tasa de refresco de datos configurable o mediante WebSocket.

---

## 4. ESTRUCTURA DE DESGLOSE DE TRABAJO (WBS) Y MATRIZ CLICKUP

### 4.1. Estructura Jerárquica del WBS (4 Niveles)

```
1. PROYECTO SISGAD5
├── 1.1. Gestión de Proyecto y Arquitectura (PM & Team Lead)
│   ├── 1.1.1. Planificación, Matriz Lógico-Estructurada y Riesgos
│   ├── 1.1.2. Diseño de Arquitectura DDD, Contratos REST y OpenAPI/Swagger
│   └── 1.1.3. Configuración de Entorno Docker Compose (Node, Go, Postgres, Redis)
├── 1.2. Módulo de Autenticación y Usuarios (Users Service - Node.js)
│   ├── 1.2.1. Implementación de Auth JWT, Refresh Tokens y Bcrypt
│   ├── 1.2.2. Control de Acceso Basado en Roles (RBAC Middleware)
│   └── 1.2.3. Endpoints de Gestión de Usuarios y Perfiles
├── 1.3. Módulo Operativo MP (MP Service - Node.js + Express)
│   ├── 1.3.1. API de Abonados, Líneas Telefónicas y Pizarras
│   ├── 1.3.2. API de Ingesta y Gestión de Quejas (CRUD + Filtros)
│   ├── 1.3.3. API de Pruebas Diagnósticas de Estación
│   └── 1.3.4. API de Trabajos de Reparación y Control de Estados
├── 1.4. Módulo Transaccional de Materiales (Materials Service - Go)
│   ├── 1.4.1. Configuración de Servicio Go con Gin Framework y GORM
│   ├── 1.4.2. Algoritmo Transaccional Concurrente con Goroutines
│   └── 1.4.3. API de Consulta de Stock, Descuento y Alertas de Mínimo
├── 1.5. Frontend y Dashboards (React 18 + Vite + Tailwind)
│   ├── 1.5.1. Maquetación UI y Componentes de Formularios
│   ├── 1.5.2. Integración de Estado Global (Redux/Zustand) y Axios
│   ├── 1.5.3. Módulo Bilingüe i18n (Español / Ruso)
│   └── 1.5.4. Dashboards de Métricas SLA, MTTR y Recharts
└── 1.6. Garantía de Calidad, Pruebas y Despliegue (QA & DevOps)
    ├── 1.6.1. Pruebas Unitarias Backend (Jest + Go Testing)
    ├── 1.6.2. Configuración de Pipeline CI/CD en GitHub Actions
    └── 1.6.3. Despliegue MVP y Documentación Técnica Final (`SISGAD5_doc`)
```

---

### 4.2. Matriz de Tareas para Importación en ClickUp

La siguiente tabla contiene las **22 tareas principales del WBS** formateadas con atributos compatibles para importación directa en ClickUp:

| Código WBS | Nombre de la Tarea en ClickUp | Prioridad | Est. (Horas) | Asignado | Estado | Dependencia |
| :---: | :--- | :---: | :---: | :--- | :---: | :---: |
| **WBS 1.1.1** | Elab. Matriz Lógica, SWOT y Matriz de Riesgos | **High** | 16 h | PM | Complete | - |
| **WBS 1.1.2** | Diseñar Arquitectura DDD y Contratos REST OpenAPI | **Urgent** | 24 h | Team Lead | Complete | WBS 1.1.1 |
| **WBS 1.1.3** | Configurar Docker Compose (PostgreSQL 17, Redis, Services) | **Urgent** | 16 h | DevOps | Complete | WBS 1.1.2 |
| **WBS 1.2.1** | Impl. Autenticación JWT y Refresh Tokens (Node.js) | **High** | 16 h | Dev Backend Node | In Progress | WBS 1.1.3 |
| **WBS 1.2.2** | Crear Middleware de Seguridad RBAC (5 Roles) | **High** | 12 h | Dev Backend Node | In Progress | WBS 1.2.1 |
| **WBS 1.2.3** | Desarrollar Endpoints de Usuarios y Perfiles | **Normal** | 12 h | Dev Backend Node | To Do | WBS 1.2.2 |
| **WBS 1.3.1** | Impl. CRUD de Abonados, Líneas y Pizarras | **High** | 20 h | Dev Backend Node | To Do | WBS 1.1.3 |
| **WBS 1.3.2** | Impl. API de Quejas y Máquina de Estados MP | **Urgent** | 24 h | Dev Backend Node | To Do | WBS 1.3.1 |
| **WBS 1.3.3** | Impl. API de Pruebas Diagnósticas (SLA <10 min) | **High** | 16 h | Dev Backend Node | To Do | WBS 1.3.2 |
| **WBS 1.3.4** | Impl. API de Trabajos de Campo (SLA <72 h) | **High** | 20 h | Dev Backend Node | To Do | WBS 1.3.3 |
| **WBS 1.4.1** | Configurar Microservicio Go (Gin + GORM) | **Urgent** | 16 h | Dev Backend Go | To Do | WBS 1.1.3 |
| **WBS 1.4.2** | Impl. Descuento Concurrente de Stock con Goroutines | **Urgent** | 24 h | Dev Backend Go | To Do | WBS 1.4.1 |
| **WBS 1.4.3** | Crear Endpoints de Inventario y Alertas de Mínimo | **High** | 16 h | Dev Backend Go | To Do | WBS 1.4.2 |
| **WBS 1.5.1** | Diseñar UI/UX Responsiva en React + Tailwind | **Normal** | 24 h | Dev Frontend | To Do | WBS 1.1.2 |
| **WBS 1.5.2** | Integrar Zustand/Axios con API Gateway | **High** | 20 h | Dev Frontend | To Do | WBS 1.5.1, 1.2.1 |
| **WBS 1.5.3** | Impl. Soporte Bilingüe i18n (Español / Ruso) | **Normal** | 12 h | Dev Frontend | To Do | WBS 1.5.1 |
| **WBS 1.5.4** | Construir Dashboards Estadísticos con Recharts | **High** | 24 h | Dev Frontend | To Do | WBS 1.5.2, 1.3.4 |
| **WBS 1.6.1** | Escribir Pruebas Unitarias (Jest + Go Testing) | **High** | 20 h | QA Engineer | To Do | WBS 1.3.4, 1.4.3 |
| **WBS 1.6.2** | Configurar Workflow de CI/CD en GitHub Actions | **High** | 12 h | DevOps | To Do | WBS 1.6.1 |
| **WBS 1.6.3** | Realizar Pruebas de Carga y Despliegue de MVP | **Urgent** | 16 h | DevOps / QA | To Do | WBS 1.6.2 |
| **WBS 1.7.1** | Redactar Manual de Usuario y Guía de Despliegue | **Normal** | 12 h | Team Lead / PM | To Do | WBS 1.6.3 |
| **WBS 1.7.2** | Preparar Informe Final de Proyecto (`SISGAD5_doc`) | **High** | 16 h | PM / Team Lead | To Do | WBS 1.7.1 |

* **Total de Horas Estimadas:** **376 horas de esfuerzo de ingeniería de software.**
* **Duración Prevista:** 16 semanas efectivas de desarrollo (8 Sprints de 2 semanas).

---

## 5. CONCLUSIONES DE LA PRÁCTICA 5-8

1. **Claridad en el Dominio**: La combinación de Event Storming y Bounded Contexts eliminó ambigüedades sobre los límites de responsabilidad entre el backend en Node.js (gestión MP) y el backend en Go (inventario transaccional).
2. **Priorización Eficiente**: El User Story Map permitió separar claramente el **MVP (Release 1 - 12 Historias clave)** de funcionalidades secundarias, asegurando el cumplimiento del plazo de 5 meses y el presupuesto de 810 000 rublos.
3. **Listabilidad Operativa**: La Estructura de Desglose de Trabajo (WBS) de 22 tareas principales (376h) proporciona una hoja de ruta directamente importable a ClickUp para el seguimiento diario de los Sprints en Scrum.
