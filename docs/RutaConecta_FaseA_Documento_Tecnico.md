# RutaConecta S.A.S. — Fase A: Visión de la Arquitectura
## Documento Técnico (TOGAF ADM 9.2)

**Asignatura:** Arquitectura de Sistemas II
**Actividad:** Trabajo asincrónico — sustitución de clase del jueves 27 de agosto de 2026
**Empresa objeto de estudio (ficticia):** RutaConecta S.A.S. — Consolidador B2B de Turismo Rural y Comunitario
**Código del documento:** RAW-FA-2026-001 (continuación técnica)
**Fecha de elaboración:** 1 de septiembre de 2026
**Elaborado por:** Equipo de Arquitectura de Sistemas (rol académico: estudiantes de Ingeniería de Sistemas)
**Versión:** 1.0

---

## Índice

1. Establecer el Proyecto de Arquitectura
2. Identificar las Partes Interesadas, sus Preocupaciones y los Requisitos del Negocio
3. Confirmar y Definir los Objetivos, Impulsores y Limitaciones del Negocio
4. Evaluar las Capacidades del Negocio
5. Evaluar la Preparación para la Transformación del Negocio
6. Definir el Alcance
7. Confirmar y Definir los Principios de Arquitectura
8. Desarrollar la Visión de la Arquitectura
9. Propuestas de Valor y KPI de la Arquitectura Objetivo
10. Riesgos de la Transformación y Actividades de Mitigación
11. Declaración de Trabajo de Arquitectura (Statement of Architecture Work) y Aprobación
12. Glosario
13. Referencias (APA 7.ª edición)

---

## 1. Establecer el Proyecto de Arquitectura

Según el estándar TOGAF, el primer paso de la Fase A consiste en confirmar el patrocinio ejecutivo y formalizar la petición de trabajo de arquitectura antes de iniciar el análisis técnico (The Open Group, 2018). Este paso ya fue documentado formalmente en el **Documento Ejecutivo de Autorización RAW-FA-2026-001**, cuyos elementos clave se resumen aquí para trazabilidad:

| Elemento | Definición en RutaConecta |
|---|---|
| Patrocinador ejecutivo (Sponsor) | Director General / Fundador de RutaConecta S.A.S. |
| Líder de arquitectura | Arquitecto Principal (rol académico: CTO Fraccional) |
| Motivo del proyecto | Migrar de una operación manual basada en hojas de cálculo (Google Drive) a una plataforma B2B centralizada, transaccional y con canales rurales de baja conectividad |
| Vigencia de la autorización | 6 meses desde la firma del documento ejecutivo |
| Marco de referencia | TOGAF 10 (ADM), adaptado (*tailored*) a una Pyme en escalamiento |
| Fases previas cerradas | Fase Preliminar (gobernanza, principios, madurez AS-IS) — ver documento *Fase Preliminar RutaConecta* |
| Fase en curso | Fase A — Visión de la Arquitectura (este documento) |

**Trigger del proyecto:** solicitud de negocio (Business Need) originada por la pérdida de ventas B2B debido a los tiempos de cotización (24–48 h), el riesgo de sobreventa y la fricción financiera con proveedores rurales — evidencia levantada en el documento *contextodelnegocio.txt*.

---

## 2. Identificar las Partes Interesadas, sus Preocupaciones y los Requisitos del Negocio

### 2.1. Matriz Poder – Interés

```
                  Estrategia de Gestión de Stakeholders
   ALTO ┌──────────────────────────────┬──────────────────────────────┐
        │      MANTENER SATISFECHO     │      GESTIONAR DE CERCA      │
  P     │  • Agencias de Viaje B2B     │  • Dirección General         │
  O     │    (Clientes)                │    (Fundadores)              │
  D     ├──────────────────────────────┼──────────────────────────────┤
  E     │      MONITOREAR (INFORMAR)   │     INVOLUCRAR ACTIVAMENTE    │
  R     │  • Proveedores Rurales       │  • Equipo de Operaciones     │
        │    (Posadas/Transporte/Guías)│  • Coordinador Financiero    │
   BAJO └──────────────────────────────┴──────────────────────────────┘
       BAJO                         INTERÉS                      ALTO
```

### 2.2. Preocupaciones y Requisitos de Negocio derivados (Stakeholder Concerns → Business Requirements)

Esta tabla amplía la matriz de involucramiento con las **preocupaciones explícitas** de cada interesado y los **requisitos de negocio (BR)** que de ellas se derivan, insumo directo para la Fase B (Arquitectura de Negocio):

| Stakeholder | Preocupación principal | Requisito de Negocio derivado (BR) |
|---|---|---|
| Dirección General | Escalar ingresos sin disparar costos fijos de personal; visibilidad de rentabilidad | **BR-01** Soportar 3x el volumen de reservas sin aumento proporcional de nómina. **BR-02** Generar reportes gerenciales de margen por agencia/proveedor. |
| Equipo de Operaciones | Carga manual en Excel, errores humanos, tiempo perdido confirmando cupos por teléfono | **BR-03** Cotización consolidada automática en < 5 minutos. **BR-04** Panel único de disponibilidad en tiempo real. |
| Coordinador Financiero / Contable | Errores en cálculo de comisiones, pérdida de soportes, pagos duplicados | **BR-05** Motor de liquidación con cálculo automático de tarifa neta, comisión y trazabilidad auditable. **BR-06** Conciliación de servicios prestados vs. facturado sin cruces manuales. |
| Agencias de Viaje B2B (clientes) | SLA de respuesta, confiabilidad del inventario, transparencia de precios finales | **BR-07** Portal de autoservicio con cotización y confirmación inmediatas. **BR-08** Garantía contractual de cero sobreventa sobre cupos confirmados. |
| Proveedores Rurales (posadas, vans, guías) | Puntualidad de pago, simplicidad tecnológica, tolerancia a baja conectividad | **BR-09** Confirmación/rechazo de reservas vía WhatsApp/SMS sin necesidad de apps pesadas. **BR-10** Notificación de liquidación con soporte digital descargable. |

> **Nota metodológica:** conforme a TOGAF, las preocupaciones (*concerns*) de los interesados determinan qué *viewpoints* arquitectónicos se deben producir en las fases siguientes (The Open Group, 2018); por ello cada BR queda numerado para ser rastreado en la matriz de trazabilidad de requisitos de la Fase B.

---

## 3. Confirmar y Definir los Objetivos, Impulsores y Limitaciones del Negocio

### 3.1. Impulsores del Negocio (Business Drivers)

1. **Pérdida de clientes por latencia comercial:** cotizaciones de 24–48 h frente a una expectativa de mercado de minutos.
2. **Riesgo operativo y reputacional por sobreventa (overbooking):** inventario desincronizado entre archivos de Google Drive.
3. **Fricción financiera con proveedores rurales:** conciliación manual mensual propensa a error y a retraso de pagos.
4. **Incapacidad de escalar:** duplicar ventas hoy implicaría duplicar personal administrativo.

### 3.2. Objetivos del Negocio (Business Goals) — medibles

| Objetivo | Línea base (AS-IS) | Meta (TO-BE) |
|---|---|---|
| Cotización en tiempo real | 24–48 horas | < 5 minutos |
| Eliminación del overbooking | 5%–8% de reservas afectadas | 0% |
| Liquidación automatizada a proveedores | 5 días hábiles (mensual, manual) | < 2 horas (automatizada) |
| Escalabilidad operativa | Crecimiento lineal de personal vs. ventas | Soportar 3x el volumen sin aumento proporcional de plantilla |

### 3.3. Limitaciones del Proyecto (Constraints)

| Tipo | Limitación | Implicación arquitectónica |
|---|---|---|
| Presupuestal | Pyme sin capital para licenciamiento corporativo o infraestructura dedicada | Priorizar servicios en la nube bajo demanda (PaaS/SaaS), pago por uso |
| Técnica / entorno | Cobertura móvil inestable o nula (2G/3G) en zonas rurales | Prohibido depender de APIs síncronas pesadas hacia el proveedor; se requieren canales asíncronos |
| Humana / adopción | Baja alfabetización digital de posaderos, transportistas y guías | Interfaces ultra-simples sobre canales ya conocidos (WhatsApp/SMS) |
| Tiempo (Time-to-Market) | El núcleo del sistema debe estar operativo antes de la siguiente temporada alta | Ventana máxima de 6 meses para el MVP |
| Legal / regulatoria *(incorporada en esta versión)* | Tratamiento de datos personales de viajeros y proveedores sujeto a la Ley 1581 de 2012 (Habeas Data) en Colombia, y a estándares de referencia internacional como el RGPD para buenas prácticas | El diseño de datos debe incorporar principios de privacidad por diseño, consentimiento y derecho de actualización/supresión (Congreso de Colombia, 2012; Parlamento Europeo y Consejo de la Unión Europea, 2016) |

---

## 4. Evaluar las Capacidades del Negocio

| Capacidad del Negocio | AS-IS | TO-BE | Brecha |
|---|---|---|---|
| Gestión de Inventario y Cupos | Descentralizada y manual en Excel/Drive, dependiente de llamadas | Control centralizado con bloqueo transaccional de cupos en tiempo real | **Alta** — requiere motor transaccional único |
| Cotización y Venta B2B | Lenta (24–48 h), cálculo manual de márgenes | Cotización automática consolidada en < 5 min | **Alta** — falta motor de reglas de precios/disponibilidad |
| Liquidación a Proveedores | Conciliación mensual cruzando WhatsApp vs. Excel | Cálculo automático de tarifas netas, comisiones y reportes auditables | **Media** — reglas existen, falta herramienta |
| Atención e Integración Rural | Informal, uno a uno por teléfono | Confirmación asíncrona por canales livianos (WhatsApp/PWA) | **Media** — requiere capa de integración asíncrona |
| Gestión de Cancelaciones y Reembolsos *(capacidad añadida)* | Inexistente como proceso formal; se resuelve caso a caso por WhatsApp | Política y motor de reglas de cancelación/penalidad integrados al motor de cotización y liquidación | **Alta** — no hay reglas ni sistema; es un vacío detectado en el material original |

> **Corrección/ajuste incorporado:** el material fuente dejaba abierta la pregunta de si incluir "Gestión de Cancelaciones y Reembolsos" como capacidad; se incorpora aquí como brecha alta porque impacta directamente los BR-05, BR-06 y BR-08.

---

## 5. Evaluar la Preparación para la Transformación del Negocio

| Dimensión de Preparación | Nivel | Riesgo identificado | Acción de mitigación |
|---|---|---|---|
| Visión y Patrocinio | Alto | Impaciencia por resultados inmediatos que fuercen un despliegue sin pruebas suficientes | Despliegue por fases (MVP) con entregas funcionales cada 4 semanas |
| Cultura Operativa | Medio | Resistencia del personal administrativo a abandonar Excel | Co-diseño de pantallas con el equipo de operaciones y formación de superusuarios |
| Capacidad Digital Rural | Bajo | Rechazo o abandono de posadas/guías ante herramientas complejas o fallas de red | Flujos basados en WhatsApp API/SMS con tolerancia a operación fuera de línea |
| Capacidad Técnica Interna | Bajo | No existe equipo de TI interno para soporte continuo | Priorizar arquitectura serverless administrada para reducir carga de soporte |
| Estandarización de Procesos | Medio | Ambigüedad en políticas de cancelación, penalidades y comisiones | Aprobar formalmente las reglas de negocio antes de codificar |

**Estrategia de gestión del cambio:**
- Diseño sin fricción: adaptar la tecnología a herramientas que el proveedor rural ya usa (chat).
- Incentivo a la disponibilidad: priorizar en resultados de búsqueda a proveedores que mantengan su inventario actualizado de forma autónoma.
- Acompañamiento en paralelo: operar el sistema nuevo junto al modelo tradicional durante un ciclo mensual antes del corte definitivo (*cutover*).
- Piloto controlado: iniciar con 5 posadas y 2 agencias B2B aliadas antes del lanzamiento masivo.

---

## 6. Definir el Alcance

Conforme a TOGAF, el alcance de un proyecto de arquitectura se define en cuatro dimensiones: amplitud (unidades de negocio cubiertas), dominios arquitectónicos, nivel de detalle y horizonte temporal (The Open Group, 2018). Se aplican a RutaConecta así:

| Dimensión TOGAF | Definición en RutaConecta |
|---|---|
| **Amplitud (Enterprise)** | RutaConecta S.A.S. como consolidador completo: operaciones, comercial B2B, finanzas/liquidaciones. No incluye subsidiarias ni líneas de negocio B2C. |
| **Dominios de arquitectura** | Negocio, Datos, Aplicaciones e Infraestructura/Seguridad (los cuatro dominios TOGAF). |
| **Nivel de detalle** | Definición de visión y principios (Fase A); el detalle de procesos, modelo de datos y diseño técnico se abordará en las Fases B, C y D. |
| **Horizonte temporal** | 6 meses para el MVP; 12 meses para alcanzar el Nivel de Madurez 3 (Definido). |

### 6.1. Dentro del Alcance (In-Scope)

- Rediseño y automatización de Gestión de Inventarios y Reservas, Motor de Cotización Instantánea B2B y Módulo de Liquidación a Proveedores.
- Modelo de datos relacional transaccional (SSOT) y plan de migración desde Google Drive.
- Portal Web para Agencias B2B y Canal de Notificaciones Asíncronas para proveedores rurales (WhatsApp API / PWA).
- Selección de servicios en la nube administrados (PaaS/SaaS) con RBAC y cifrado de datos sensibles.

### 6.2. Fuera del Alcance (Out-of-Scope, esta fase)

- Aplicación móvil nativa (iOS/Android) para el turista final (B2C).
- Pasarelas de pago internacionales multimoneda (se limita a pasarelas nacionales y transferencias locales).
- Modificación de sistemas contables externos preexistentes (solo se definen interfaces de integración/exportación).

---

## 7. Confirmar y Definir los Principios de Arquitectura

Los principios se agrupan en tres categorías: ética/gobierno, negocio/operación y tecnología. Cada uno sigue la estructura estándar Declaración–Justificación–Implicación (The Open Group, 2018).

### Categoría A: Ética, Transparencia y Gobierno

**P1 — Comercio Justo con Proveedores Rurales.** Toda transacción o liquidación debe basarse en tarifas transparentes y pago puntual. *Implicación:* el módulo de liquidación genera trazabilidad auditable e inalterable de cada transacción.

**P2 — Protección y Tratamiento Ético de Datos (Privacidad por Diseño).** Los datos de viajeros, agencias y cuentas bancarias de proveedores se tratan conforme a normativa de protección de datos vigente. *Implicación:* cifrado en tránsito (TLS/HTTPS) y en reposo (AES-256), control de acceso basado en roles (RBAC), y cumplimiento del principio de minimización y consentimiento del titular establecido en la Ley 1581 de 2012 en Colombia (Congreso de Colombia, 2012), tomando además como referencia de buenas prácticas internacionales el RGPD europeo (Parlamento Europeo y Consejo de la Unión Europea, 2016).

**P3 — Confidencialidad y Propiedad de la Información B2B.** Tarifas costo y márgenes se tratan como secreto comercial con aislamiento total entre cuentas. *Implicación:* segmentación lógica de vistas; ninguna agencia accede a costos internos ni a datos de otras agencias.

### Categoría B: Principios Operativos y de Negocio

**P4 — Primacía del Negocio y Simplicidad Operativa.** Las decisiones tecnológicas priorizan soluciones simples y mantenibles sobre arquitecturas sobre-diseñadas. *Implicación:* diseño Monolítico Modular o servicios gestionados (PaaS/SaaS).

**P5 — Fuente Única de la Verdad (SSOT).** Todo dato operativo reside en un repositorio central; se prohíbe el uso de hojas de cálculo como base de datos de producción.

### Categoría C: Principios Tecnológicos y de Infraestructura

**P6 — Inclusividad Tecnológica y Operación Offline-First.** Componentes rurales ultra-livianos, tolerantes a conectividad nula/intermitente, vía WhatsApp Business API, SMS o PWA de bajo consumo.

**P7 — Interoperabilidad mediante Estándares Abiertos (APIs).** Comunicación entre módulos vía APIs RESTful/JSON documentadas, sin acoplamiento directo a la base de datos desde componentes externos.

> Estos 7 principios consolidan y reconcilian las dos listas preliminares generadas durante el debate del equipo (una de 4 y otra de 7 principios); se conserva la versión de 7 por ser la más completa y la que ya fue aprobada en la Fase Preliminar.

---

## 8. Desarrollar la Visión de la Arquitectura

### 8.1. Declaración de la Visión

> "Convertir la operación manual y fragmentada de RutaConecta en una plataforma digital integrada, elástica y de bajo costo en la nube, que automatice la gestión de inventario rural en tiempo real, habilite la cotización B2B instantánea (< 5 minutos) y garantice la liquidación transparente a los proveedores locales, utilizando canales accesibles e inmunes a los problemas de conectividad rural."

### 8.2. Concepto de Solución — Vista de Alto Nivel (TO-BE)

```
+-----------------------------------------------------------------------------------+
| CANALES DE ENTRADA                                                                 |
| Agencias B2B (Portal Web) | Proveedores (WhatsApp Bot/PWA) | Operaciones (Dashboard)|
+-----------------------------------------------------------------------------------+
                                          |
+-----------------------------------------------------------------------------------+
| CAPA DE SERVICIOS / APIS                                                           |
| API Gateway + Autenticación / Roles                                                |
+-----------------------------------------------------------------------------------+
                                          |
+-----------------------------------------------------------------------------------+
| LÓGICA DE NEGOCIO                                                                  |
| Motor de Inventario | Motor de Cotizaciones | Motor de Liquidaciones               |
| (Control de Cupos/Locks) | (Reglas de Margen/Paquetes) | (Conciliación Automática) |
+-----------------------------------------------------------------------------------+
                                          |
+-----------------------------------------------------------------------------------+
| CAPA DE DATOS E INFRAESTRUCTURA                                                    |
| Base de Datos Transaccional (SQL/NoSQL PaaS) + Cola de Eventos / Bus               |
+-----------------------------------------------------------------------------------+
```

### 8.3. Matriz de Valor para Stakeholders

| Stakeholder | Preocupación principal | Valor entregado por la arquitectura objetivo |
|---|---|---|
| Gerencia General | Rentabilidad y escalabilidad | Crecimiento 3x en ventas sin aumento proporcional de costos fijos |
| Agencias B2B (Clientes) | Lentitud y riesgo de cancelación | Respuesta < 5 min con disponibilidad garantizada, sin overbooking |
| Proveedores Rurales | Errores en pagos, complejidad técnica | Confirmación vía WhatsApp y liquidación transparente automatizada |
| Equipo de Operaciones | Sobrecarga manual y errores | Eliminación de trabajo repetitivo; foco en gestión de excepciones |

### 8.4. Matriz de Requerimientos No Funcionales (RNF) — Escenarios de Calidad (ISO/IEC 25010)

Siguiendo la estructura formal Estímulo–Entorno–Fuente–Artefacto–Respuesta–Medida de Respuesta y el modelo de calidad ISO/IEC 25010 (International Organization for Standardization, 2011):

**Escenario 1 — Disponibilidad**
- Fuente: agencia B2B / usuario interno.
- Estímulo: consulta de inventario o cotización en hora pico.
- Entorno: operación normal en producción en la nube.
- Artefacto: módulo de Gestión de Inventario y Cotizaciones.
- Respuesta: el sistema atiende sin caídas ni degradación.
- Medida: 99.5% de *uptime* en horario comercial (8:00–20:00 GMT-5); MTTR < 15 minutos.

**Escenario 2 — Rendimiento / Latencia**
- Fuente: agente de ventas de agencia B2B.
- Estímulo: solicitud de cotización consolidada para un grupo de 15 personas.
- Entorno: hasta 50 solicitudes concurrentes.
- Artefacto: motor de reglas y cotizaciones (backend API).
- Respuesta: cálculo de tarifa consolidada y márgenes en tiempo real.
- Medida: tiempo de respuesta < 3.0 segundos (percentil 95).

**Escenario 3 — Consistencia e Integridad de Datos**
- Fuente: dos agencias solicitando el último cupo disponible simultáneamente.
- Entorno: concurrencia pico.
- Artefacto: base de datos relacional / gestor de bloqueos de inventario.
- Respuesta: bloqueo de fila (*row-level locking*) o transacción serializable.
- Medida: 0 sobreventas; la segunda transacción recibe "inventario agotado" en < 1 segundo.

**Escenario 4 — Usabilidad e Inclusividad en Campo**
- Fuente: posadero o guía local en zona rural.
- Estímulo: recepción de solicitud de reserva.
- Entorno: conexión móvil 2G/3G intermitente.
- Artefacto: canal de integración con proveedores (WhatsApp API / PWA).
- Respuesta: interfaz de bajo consumo de datos, confirmación con un toque.
- Medida: *payload* < 50 KB; confirmación asíncrona dentro de una ventana de hasta 4 horas.

**Escenario 5 — Seguridad *(añadido para completar cobertura ISO/IEC 25010)***
- Fuente: usuario no autorizado o intento de acceso indebido a datos de otra agencia/proveedor.
- Estímulo: intento de acceso a costos internos o datos bancarios sin permisos.
- Entorno: operación normal del sistema en producción.
- Artefacto: capa de autenticación/autorización (RBAC) y cifrado de datos sensibles.
- Respuesta: el sistema deniega el acceso y registra el evento en el log de auditoría.
- Medida: 100% de los intentos de acceso no autorizado bloqueados y registrados; retención de logs de auditoría mínima de 12 meses conforme a buenas prácticas de trazabilidad de datos personales (Congreso de Colombia, 2012).

### 8.5. Análisis de Trade-offs

**Disponibilidad vs. Consistencia (adaptación del Teorema CAP al negocio):**

- *Opción A — Priorizar disponibilidad:* permitir reservar sin confirmar físicamente el inventario con el proveedor. Riesgo: overbooking; impacto: pérdida de reputación.
- *Opción B — Priorizar consistencia:* bloquear la reserva hasta confirmar el inventario. Ventaja: cero overbooking; costo: espera breve en la respuesta.

**Decisión de trade-off:** se selecciona la **Opción B (Consistencia sobre Disponibilidad total)** para el inventario crítico (habitaciones y asientos), porque el costo reputacional de dejar a un grupo de turistas sin hospedaje supera el costo de una breve espera de confirmación (estimada en minutos, no en horas).

---

## 9. Propuestas de Valor y KPI de la Arquitectura Objetivo

### 9.1. Propuestas de Valor por Stakeholder

- **Agencias B2B:** respuesta inmediata a cotizaciones y garantía de disponibilidad sin sobreventas.
- **Proveedores Rurales:** canal ultra-simple de baja conectividad y liquidaciones puntuales y transparentes.
- **Equipo de Operaciones:** eliminación de la carga administrativa repetitiva; panel de control para supervisión y gestión de excepciones.
- **Gerencia General:** modelo operativo elástico, capaz de multiplicar por 3 el volumen de ventas sin incrementar costos fijos de personal, bajo un esquema de costos de nube variable.

### 9.2. Matriz de KPI

| KPI | AS-IS | Meta TO-BE | Impacto en el negocio |
|---|---|---|---|
| Tiempo de cotización B2B | 24–48 horas | < 5 minutos | Incremento directo en tasa de conversión de ventas |
| Tasa de overbooking | 5%–8% de reservas | 0% | Preservación de la reputación y confianza B2B |
| Tiempo de liquidación a proveedores | 5 días hábiles | < 2 horas (automatizado) | Fidelización de la red rural |
| Tasa de adopción de proveedores | 0% (gestión telefónica) | > 85% activa en plataforma | Descentralización de la actualización de inventario |
| Costo operativo por reserva | Alto, crece linealmente | Reducción del 60% | Margen escalable por automatización serverless |
| Tasa de conversión de solicitudes B2B | ~15% | > 35% | Captura de demanda antes perdida por lentitud |
| Disponibilidad del sistema *(KPI añadido, alineado al Escenario 1 de RNF)* | No medido | ≥ 99.5% *uptime* mensual | Confiabilidad percibida por agencias B2B |

---

## 10. Riesgos de la Transformación y Actividades de Mitigación

| Riesgo | Categoría | Impacto / Probabilidad | Mitigación | Indicador de control |
|---|---|---|---|---|
| Resistencia al cambio del equipo interno | Humana / Cultural | Alto / Media | Redefinir roles hacia supervisión analítica; involucrar al personal en el diseño desde el inicio | 100% del personal operativo capacitado en la nueva interfaz |
| Baja adopción de proveedores rurales | Entorno / Operativa | Alto / Alta | Flujos asíncronos vía WhatsApp API/SMS con soporte fuera de línea | > 80% de confirmaciones rurales autónomas |
| Errores en cálculo de tarifas durante la migración desde Excel | Negocio / Financiera | Alto / Media | Auditar y congelar reglas de margen con gerencia antes de codificar; simulaciones en paralelo con datos reales | 0% de discrepancias en liquidación durante pruebas |
| Desviación del alcance (*scope creep*) | Proyecto / TI | Medio / Alta | Congelar alcance del MVP y derivar extras al backlog | Cumplimiento de la ventana de 6 meses |
| Fallas o sobreventas durante el *cutover* | Técnica / Operativa | Alto / Baja | Migración en temporada baja, despliegue progresivo y plan de reversión (*rollback*) probado | 0 interrupciones en la toma de reservas B2B durante la transición |
| Incumplimiento normativo de protección de datos personales *(riesgo añadido)* | Legal / Cumplimiento | Alto / Baja | Incorporar principios de privacidad por diseño desde el modelado de datos (Fase C) y designar un responsable del tratamiento conforme a la Ley 1581 de 2012 | 0 hallazgos de incumplimiento en auditoría interna previa al lanzamiento |

**Plan de control adicional:** despliegue piloto con 5 posadas y 2 agencias B2B aliadas; canal de soporte prioritario en campo durante el primer mes de operación.

---

## 11. Declaración de Trabajo de Arquitectura (Statement of Architecture Work) y Aprobación

Este es el entregable formal de cierre de la Fase A según TOGAF: un documento que solicita y confirma la aprobación de un proyecto de arquitectura, describiendo el trabajo a realizar y cómo se hará (The Open Group, 2018). Se estructura a continuación:

### 11.1. Identificación

| Campo | Valor |
|---|---|
| Título del proyecto | Transformación de la Arquitectura Empresarial de RutaConecta S.A.S. — Fase A |
| Código | RAW-FA-2026-001 |
| Patrocinador | Director General / Fundador |
| Arquitecto responsable | Arquitecto Principal (rol académico) |
| Fecha | 1 de septiembre de 2026 |

### 11.2. Propósito, Objetivos y Alcance del Trabajo

Diseñar la Visión de la Arquitectura (Fase A del ADM) que sustente la migración de RutaConecta de un modelo operativo manual a una plataforma digital centralizada, conforme a los objetivos definidos en la sección 3.2 y al alcance definido en la sección 6.

### 11.3. Enfoque (Approach)

- Ciclo ADM de TOGAF 10, adaptado (*tailored*) al tamaño y presupuesto de una Pyme (gobernanza ágil, sin comité corporativo pesado).
- Trabajo iterativo y colaborativo entre el equipo de arquitectura y el equipo de operaciones/finanzas de RutaConecta (8 horas semanales de dedicación conjunta).
- Validación continua con la Dirección General en cada hito de fase.

### 11.4. Roles y Responsabilidades

Ver tabla de roles en la sección 1 del documento *Fase Preliminar RutaConecta* (Patrocinador, Arquitecto Principal, Arquitecto de Negocio, Arquitecto de Software, Arquitecto de Datos, Arquitecto de Infraestructura).

### 11.5. Plan y Cronograma de Referencia

| Fase ADM | Estado | Entregable principal | Horizonte estimado |
|---|---|---|---|
| Fase Preliminar | Cerrada | Gobernanza, principios, madurez AS-IS | Completada |
| Fase A — Visión de la Arquitectura | En curso (este documento) | Documento de Visión, RNF, trade-offs, KPI, riesgos | Mes 1 |
| Fase B — Arquitectura de Negocio | Próxima | Procesos AS-IS/TO-BE (Reservas, Cotización, Liquidación) | Mes 2 |
| Fase C — Arquitectura de Datos y Aplicaciones | Planificada | Modelo relacional SSOT, diseño de APIs | Meses 3–4 |
| Fase D — Arquitectura Tecnológica | Planificada | Topología en la nube, seguridad, DRP/BCP | Mes 5 |
| Fases E–F — Oportunidades, Migración | Planificada | Hoja de ruta de implementación, plan de migración | Mes 6 |

### 11.6. Caso de Negocio (Business Case, resumen)

La inversión en la plataforma se justifica frente al costo de oportunidad actual: pérdida de ventas por lentitud (tasa de conversión ~15%), riesgo reputacional por overbooking (5–8% de reservas) y sobrecostos administrativos crecientes de forma lineal con las ventas. Las metas TO-BE de la sección 9.2 constituyen el caso de negocio cuantificado.

### 11.7. Gestión de Riesgos del Programa

Ver matriz completa en la sección 10. Se destaca como riesgo de mayor probabilidad la baja adopción de proveedores rurales, mitigado mediante canales ultra-livianos y un despliegue piloto controlado.

### 11.8. Requisitos de Conformidad

Todo diseño posterior (Fases B–D) debe demostrar conformidad con los 7 Principios de Arquitectura (sección 7) y con los Escenarios de Calidad definidos (sección 8.4). Las desviaciones deben documentarse y aprobarse por el Arquitecto Principal.

### 11.9. Presupuesto y Recursos Comprometidos

Ver Documento Ejecutivo de Autorización RAW-FA-2026-001, sección 4 (equipo humano dedicado, tiempo de disponibilidad de Operaciones, presupuesto preliminar para herramientas de modelado, entornos de prueba PaaS y licenciamiento de APIs).

### 11.10. Aprobación

Con la firma de esta Declaración de Trabajo de Arquitectura, el patrocinador ejecutivo autoriza el cierre de la Fase A y el inicio de la Fase B (Arquitectura de Negocio).

| Rol | Nombre | Firma | Fecha |
|---|---|---|---|
| Patrocinador Ejecutivo (Director General) | ______________________ | ______________________ | __________ |
| Arquitecto Principal / Líder del Proyecto | ______________________ | ______________________ | __________ |
| Arquitecto de Negocio | ______________________ | ______________________ | __________ |

---

## 12. Glosario

- **ADM (Architecture Development Method):** método cíclico de TOGAF para desarrollar arquitectura empresarial.
- **SSOT (Single Source of Truth):** repositorio único y autoritativo de datos.
- **RBAC (Role-Based Access Control):** control de acceso basado en roles.
- **RNF:** Requerimientos No Funcionales, medidos como escenarios de calidad.
- **MVP (Minimum Viable Product):** versión mínima funcional del producto.
- **PaaS/SaaS:** Platform/Software as a Service, servicios de nube administrados.

## 13. Referencias (APA 7.ª edición)

Congreso de Colombia. (2012). *Ley 1581 de 2012, por la cual se dictan disposiciones generales para la protección de datos personales*. Diario Oficial No. 48.587. https://www.funcionpublica.gov.co/eva/gestornormativo/norma.php?i=49981

International Organization for Standardization. (2011). *ISO/IEC 25010:2011 — Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models*. https://www.iso.org/standard/35733.html

Parlamento Europeo y Consejo de la Unión Europea. (2016). *Reglamento (UE) 2016/679, relativo a la protección de las personas físicas en lo que respecta al tratamiento de datos personales (RGPD)*. Diario Oficial de la Unión Europea. https://eur-lex.europa.eu/eli/reg/2016/679/oj

The Open Group. (2018). *The TOGAF® Standard, Version 9.2 — Phase A: Architecture Vision*. https://pubs.opengroup.org/architecture/togaf9-doc/arch/chap06.html

The Open Group. (2018). *The TOGAF® Standard, Version 9.2*. https://www.opengroup.org/togaf-standard-version-92-overview
