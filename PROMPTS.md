# PROMPTS_es.md — Prompts de Ejecución Secuencial para Agentes de IA

Utilice estos prompts listos para ejecutar secuencialmente para instruir a los agentes de ejecución a completar cada fase restante del proyecto.

---

### PROMPT 1: Ejecutar Prácticas 5-8 (Definición de Alcance, USM y WBS)
```text
Actúa como un Senior IT Project Manager y Arquitecto de Sistemas. Basado en SPEC_es.md, AGENT_es.md y el contexto del proyecto SISGAD5, genera el informe académico completo para "Práctica Asignada 5-8: Definición de Proyecto, DDD & WBS" en español (formato .md).

Requisitos:
1. Análisis de Event Storming DDD y mapeo de 5 Contextos Acotados.
2. Tabla de Mapa de Historias de Usuario (USM) definiendo actividades, tareas de usuario y porciones de historias para MVP (Sprint 1-3) vs. Versión 2.0.
3. Estructura de Desglose de Trabajos (WBS) en formato markdown jerárquico y matriz estructurada adecuada para importar en ClickUp.
4. Guardar el resultado como 'practica-5-8-scope-wbs-es.md'.
```

---

### PROMPT 2: Ejecutar Prácticas 9-10 (Ágil/Scrum y Matriz RACI)
```text
Actúa como un Certified Scrum Master y Líder de Equipo Ágil. Basado en SPEC_es.md, AGENT_es.md y el contexto del proyecto SISGAD5, genera el informe académico completo para "Práctica Asignada 9-10: Ágil, Scrum y Liderazgo de Equipo" en español (formato .md).

Requisitos:
1. Product Backlog y Sprint Backlog para 4 Sprints (iteraciones de 2 semanas).
2. Matriz RACI completa cubriendo todas las entregas del proyecto a través de roles del Cliente y del Desarrollador.
3. Estructura de datos de Gráfico de Burndown y cálculos de Métricas de Velocidad del Equipo.
4. Estrategia de comunicación y resolución de conflictos del equipo basada en materiales de la cátedra.
5. Guardar el resultado como 'practica-9-10-scrum-raci-es.md'.
```

---

### PROMPT 3: Ejecutar Prácticas 11-12 (Plan de Gestión de Riesgos)
```text
Actúa como un IT Risk Manager. Basado en SPEC_es.md, AGENT_es.md y el contexto del proyecto SISGAD5, genera el informe académico completo para "Práctica Asignada 11-12: Plan de Gestión de Riesgos" en español (formato .md).

Requisitos:
1. Identificación de al menos 10 riesgos técnicos, organizacionales y operativos.
2. Evaluación cualitativa de riesgos usando la Escala de Harrington y Matriz de Probabilidad-Impacto.
3. Registro de Mitigación de Riesgos completo (Dueño del Riesgo, Gatillo, Estrategia de Respuesta, Asignación de Reservas).
4. Cálculo de Reservas de Contingencia y Gestión para el presupuesto de 810,000 RUB.
5. Guardar el resultado como 'practica-11-12-risk-management-es.md'.
```

---

### PROMPT 4: Ejecutar Prácticas 13-14 (Control de Cambios y MoSCoW)
```text
Actúa como un Business Analyst y Change Manager. Basado en SPEC_es.md, AGENT_es.md y el contexto del proyecto SISGAD5, genera el informe académico completo para "Práctica Asignada 13-14: Control de Cambios y Priorización MoSCoW" en español (formato .md).

Requisitos:
1. Matriz de Priorización MoSCoW de Requisitos para todos los 24 Casos de Uso de SISGAD5.
2. Proceso Formal del Comité de Control de Cambios (CCB) y procedimiento de Evaluación de Solicitudes de Cambio (CR).
3. Caso de Estudio: Análisis de impacto de un nuevo requisito (por ejemplo, modo offline móvil para técnicos de campo) en alcance, costo y cronograma.
4. Guardar el resultado como 'practica-13-14-change-control-es.md'.
```

---

### PROMPT 5: Ejecutar Prácticas 15-16 (Informe Final y Defensa del Sistema)
```text
Actúa como un Director de Tecnología (CTO) y Asesor Académico. Basado en SPEC_es.md, AGENT_es.md y todos los informes de prácticas previas, genera el informe final para "Práctica Asignada 15-16: Defensa Final del Proyecto y Métricas de Calidad" en español (formato .md).

Requisitos:
1. Resumen Ejecutivo de los logros del proyecto SISGAD5, cumplimiento presupuestario y mejoras en SLA.
2. Métricas de Aseguramiento y Control de Calidad (QA/QC) (reducción de MTTR, cobertura de pruebas concurrentes de Go, latencia de API).
3. Especificación de Despliegue del Sistema (Docker Compose, PostgreSQL 17, Redis, GitHub Actions).
4. Esquema de Diapositivas para Presentación Final (10-12 diapositivas) ante el comité de RTU MIREA.
5. Guardar el resultado como 'practica-15-16-final-report-es.md'.
```

---

### PROMPT 6: Agente de Desarrollo y Pruebas de Código
```text
Actúa como Lead Full-Stack Developer en SISGAD5. Revisa el código fuente en 'ybotet/SISGAD5_1.0' y genera pruebas unitarias para el Servicio de Materiales de Go (probando la deducción concurrente de goroutines) e integración pruebas para los endpoints del Servicio MP de Node.js. Asegura 100% de compatibilidad con la configuración de Docker Compose.
```