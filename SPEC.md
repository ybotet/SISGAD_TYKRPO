# SPEC_es.md — Especificación Técnica y Académica (SISGAD5)

## 1. RESUMEN DEL PROYECTO
- **Título del Proyecto**: SISGAD5 (Sistema de Información para la Gestión de Incidencias, Servicios y Materiales)
- **Cliente**: ETECSA — Dirección No. 5 (Rama de Servicios Gubernamentales No. 5)
- **Institución**: RTU MIREA — IPTIP, Cátedra de Programación Industrial
- **Autor / Estudiante**: Yaisel Botet Cesar (Grupo ÉFMO-01-25)
- **Presupuesto**: 810,000 RUB
- **Duración**: 5 meses (01.10.2025 – 01.03.2026)

---

## 2. ESPECIFICACIÓN FUNCIONAL Y CONTEXTOS DE DOMINIO
SISGAD5 está dividido en 5 Contextos Acotados (DDD):

1. **Gestión de Autenticación y Usuarios (`bd_users`)**:
   - Roles RBAC: `admin`, `probador`, `editor`, `supervisor`, `admin_materiales`.
   - Tokens de Acceso JWT + Tokens de Refresh almacenados en Redis.
2. **Operaciones Principales / Gestión de Incidencias (`bd_mp`)**:
   - `Quejas` (Reclamos): Registro de tickets, datos del suscriptor, clasificación de fallos.
   - `Pruebas` (Diagnósticos): Pruebas de estaciones, parámetros de línea, SLA <10 min.
   - `Trabajos` (Órdenes de Trabajo): Asignación a técnicos de campo, registro de reparaciones, SLA <72h.
3. **Materiales e Inventario (`bd_materiales`)**:
   - Catálogo de artículos, stock por brigada, deducción transaccional mediante goroutines de Go.
4. **Análisis y Dashboards**:
   - KPIs en tiempo real, tendencias de MTTR, reportes de consumo de materiales, exportación a CSV/JSON.
5. **Seguridad del Sistema y Auditoría**:
   - Completa auditoría de operaciones CRUD, seguridad PostgreSQL 17.

---

## 3. ESPECIFICACIÓN DEL CURSO ACADÉMICO (RTU MIREA)

El proyecto cumple con 16 Prácticas Asignadas:

| Bloque de Prácticas | Tema / Entregables | Estado |
| :--- | :--- | :---: |
| **Prácticas 1-4** | Mapa del Proyecto, Lista de Partes Interesadas, Análisis de Problemas (SWOT, Fishbone, 5 Whys), Matriz Marco Lógico (LFM / ЛСМ). | **COMPLETADO** |
| **Prácticas 5-8** | Definición de Alcance, Diseño Orientado a Dominios (DDD), Event Storming, Mapa de Historias de Usuario (USM), Estructura de Desglose de Trabajos (WBS). | **PENDIENTE** |
| **Prácticas 9-10** | Marco Ágil y Scrum, Sprint Backlog, Matriz RACI, Métricas de Velocidad y Gráficos de Burndown. | **PENDIENTE** |
| **Prácticas 11-12** | Plan de Gestión de Riesgos, Escala de Harrington, Matriz de Probabilidad-Impacto, Registro de Mitigación de Riesgos. | **PENDIENTE** |
| **Prácticas 13-14** | Gestión del Cambio, Priorización MoSCoW, Proceso del Comité de Control de Cambios (CCB). | **PENDIENTE** |
| **Prácticas 15-16** | Gestión de Calidad (QA/QC), Informe Final Comprehensivo, Presentación y Despliegue del Sistema. | **PENDIENTE** |