# Fase Preliminar: Capacidad Arquitectónica de la Organización

**Empresa:** RutaConecta S.A.S.  
**Sector:** Turismo y Viajes (Consolidador B2B de Servicios Rurales y Comunitarios)  
**Proyecto:** Definición de la Arquitectura Empresarial (AS-IS / TO-BE)  

---

## 1. Organización de la Arquitectura

### 1.1. Estructura del Equipo de Arquitectura, Roles y Responsabilidades

Para adaptar el marco TOGAF 10 a la realidad de RutaConecta (Pyme en proceso de escalamiento), se establece una estructura de gobernanza ágil y lean. La responsabilidad del programa de arquitectura se distribuye en los siguientes roles específicos:

| Rol / Función | Perfil Sugerido | Responsabilidades Principales en RutaConecta |
| :--- | :--- | :--- |
| **Patrocinador del Proyecto (Sponsor)** | Director General / Fundador | Aprueba el presupuesto, prioriza las inversiones estratégicas, alinea los objetivos del negocio con la arquitectura y resuelve conflictos de alto nivel. |
| **Arquitecto Principal (Lead Architect)** | Consultor Externo / CTO Fraccional | Vela por la visión global de la arquitectura TO-BE, gobierna los cambios del sistema, resuelve los trade-offs técnicos/financieros y garantiza el cumplimiento del marco TOGAF. |
| **Arquitecto de Negocio & Dominio** | Gerente de Operaciones | Modela y rediseña los procesos operativos (Gestión de Reservas, Cotización Rápida y Liquidación a Proveedores), asegurando que la tecnología responda a la operación real en campo. |
| **Arquitecto de Software** | Desarrollador Senior / Tech Lead | Diseña la estructura interna del sistema, define los patrones de diseño, los componentes modulares de la aplicación, las APIs de comunicación y la lógica transaccional. |
| **Arquitecto de Datos** | Ingeniero de Datos / DBA | Diseña el modelo de datos relacional (Fuente Única de la Verdad - SSOT), gobierna la calidad de la información, establece la migración desde Google Sheets y asegura la integridad referencial. |
| **Arquitecto de Infraestructura** | Ingeniero DevOps / Cloud Engineer | Diseña la topología de nube, define la estrategia de disponibilidad, escalabilidad y seguridad perimetral, y formula los planes de contingencia (DRP/BCP). |

---

### 1.2. Mapeo de Stakeholders (Matriz Poder - Interés)

Se identifican los actores clave que afectan o son afectados por la arquitectura, definiendo su nivel de involucramiento para asegurar la viabilidad del proyecto:

```
                  Estrategia de Gestión de Stakeholders
   ALTO ┌──────────────────────────────┬──────────────────────────────┐
        │                              │                              │
        │      MANTENER SATISFECHO     │      GESTIONAR DE CERCA      │
        │                              │                              │
  P     │  • Agencias de Viaje B2B     │  • Dirección General         │
  O     │    (Clientes)                │    (Fundadores)              │
  D     │                              │                              │
  E     ├──────────────────────────────┼──────────────────────────────┤
  R     │                              │                              │
        │      MONITOREAR (INFORMAR)   │     INVOLUCRAR ACTIVAMENTE    │
        │                              │                              │
        │  • Proveedores Rurales       │  • Equipo de Operaciones     │
        │    (Posadas/Transporte/Guías)│  • Coordinador Financiero    │
   BAJO └──────────────────────────────┴──────────────────────────────┘
       BAJO                         INTERÉS                      ALTO
```

- **Dirección General (Alto Poder / Alto Interés) ➔ Gestionar de Cerca:**
  - **Interés:** Escalar el negocio sin disparar los costos operativos; eliminar cuellos de botella para aumentar el volumen de ventas.
  - **Estrategia:** Revisiones periódicas de hitos estratégicos, alineación del ROI y aprobación formal de decisiones de trade-off.

- **Equipo de Operaciones y Cotizaciones (Bajo Poder / Alto Interés) ➔ Involucrar Activamente:**
  - **Interés:** Reducir el tiempo de cotización (de 48h a minutos), eliminar la gestión manual de archivos Excel y evitar la sobreventa (overbooking).
  - **Estrategia:** Participación directa en el co-diseño de flujos TO-BE, levantamiento de requisitos operativos y pruebas de usabilidad del nuevo sistema.

- **Coordinador Financiero / Contable (Bajo Poder / Alto Interés) ➔ Involucrar Activamente:**
  - **Interés:** Automatizar los cálculos de comisiones y dispersión de pagos a proveedores rurales sin errores manuales ni pérdida de soportes.
  - **Estrategia:** Definición detallada de reglas de negocio para liquidaciones, concilaciones de cuentas y validación de reportes auditales.

- **Agencias de Viaje B2B - Clientes (Alto Poder / Bajo-Medio Interés) ➔ Mantener Satisfecho:**
  - **Interés:** Obtener confirmación inmediata de inventarios, tarifas competitivas y cotizaciones consolidadas en minutos para cerrar ventas finales.
  - **Estrategia:** Consultas informales sobre necesidades de interfaz, pruebas piloto del portal B2B y garantía de niveles de servicio (SLA).

- **Proveedores Rurales - Hoteles/Vans/Guías (Bajo Poder / Bajo-Medio Interés) ➔ Monitorear / Informar:**
  - **Interés:** Transparencia y puntualidad en sus pagos; herramientas simples que no requieran conectividad intensiva ni capacitación técnica compleja.
  - **Estrategia:** Capacitación en canales ultra-livianos (WhatsApp/SMS) y comunicación clara de las políticas de pago.

---

## 2. Principios de Arquitectura

Los principios de arquitectura constituyen las reglas rectoras e innegociables que orientarán todas las decisiones de diseño, contratos con terceros, manejo de información y desarrollo tecnológico en RutaConecta.

### Categoría A: Principios de Ética, Transparencia y Gobierno

#### Principio 1: Comercio Justo y Ética en la Relación con Proveedores Rurales
- **Declaración:** Toda transacción, liquidación o acuerdo registrado en el sistema debe basarse en la transparencia de tarifas, condiciones contractuales claras y el cumplimiento puntual de los pagos a los aliados comunitarios.
- **Justificación:** El negocio de RutaConecta depende de la confianza de los pequeños prestadores locales. Cálculos erróneos o retrasos en liquidaciones destruyen la red de abastecimiento y la reputación del modelo.
- **Implicación:** El módulo de liquidación debe generar trazabilidad auditable de cada centavo (costos netos, márgenes y comisiones), emitiendo reportes claros e inalterables para ambas partes.

#### Principio 2: Protección y Tratamiento Ético de Datos (Privacidad por Diseño)
- **Declaración:** Los datos personales de viajeros, representantes de agencias clientes y cuentas bancarias de proveedores son estrictamente confidenciales y se tratarán bajo el cumplimiento normativo (Habeas Data / GDPR).
- **Justificación:** La fuga o filtración de identidades o datos bancarios expone a la empresa a sanciones legales severas y a la pérdida total de confianza en el mercado.
- **Implicación:** La información sensible debe encriptarse tanto en tránsito (TLS/HTTPS) como en reposo (AES-256). Se implementará un esquema estricto de Control de Acceso Basado en Roles (RBAC).

#### Principio 3: Confidencialidad y Propiedad de la Información B2B
- **Declaración:** Las tarifas costo negociadas con cada proveedor y los márgenes aplicados a cada agencia de viajes B2B se tratarán como secreto comercial con aislamiento total entre cuentas.
- **Justificación:** Revelar las tarifas neta de un hotel a una agencia o exponer los márgenes de un competidor destruye la ventaja comercial de RutaConecta.
- **Implicación:** La arquitectura de datos garantizará aislamiento lógico estricto (multitenancy o segmentación de vistas); un usuario cliente jamás podrá acceder a costos internos ni a datos de otras agencias.

### Categoría B: Principios Operativos y de Negocio

#### Principio 4: Primacía del Negocio y Simplicidad Operativa
- **Declaración:** Las decisiones tecnológicas deben responder a la meta de acelerar las ventas B2B y automatizar la operación, priorizando soluciones técnicas sencillas y mantenibles sobre arquitecturas sobre-diseñadas.
- **Justificación:** Como Pyme, RutaConecta no cuenta con presupuesto ni capacidad técnica para administrar infraestructuras hiper-complejas (ej. mallas de microservicios avanzadas).
- **Implicación:** Se priorizará un diseño Monolítico Modular o servicios gestionados en la nube (PaaS/SaaS) que reduzcan los costos de mantenimiento y aceleren el tiempo de salida al mercado (Time-to-Market).

#### Principio 5: Fuente Única de la Verdad (Single Source of Truth - SSOT)
- **Declaración:** Todo dato operativo (inventario de cupos, tarifas, reservas y cuentas por pagar) debe residir exclusivamente en un repositorio central estructurado, quedando prohibida la fragmentación en archivos locales.
- **Justificación:** Mantener la operación sobre múltiples archivos de Excel en Google Drive ocasiona duplicidad, sobreventas (overbooking) y errores contables en la liquidación.
- **Implicación:** Queda formalmente prohibido el uso de hojas de cálculo como base de datos de producción. Todo registro transaccional se realizará sobre la Base de Datos Relacional centralizada.

### Categoría C: Principios Tecnológicos y de Infraestructura

#### Principio 6: Inclusividad Tecnológica y Operación Offline-First en Campo
- **Declaración:** Los componentes expuestos a los proveedores rurales deben ser ultra-livianos, adaptados a dispositivos móviles de gama baja y tolerantes a la conectividad nula o intermitente.
- **Justificación:** Las posadas, transportistas y guías operan frecuentemente en zonas naturales sin señal de internet estable.
- **Implicación:** Las confirmaciones de disponibilidad no dependerán de llamadas síncronas a APIs pesadas; se implementarán integraciones asíncronas vía WhatsApp Business API, SMS o Web Apps Progresivas (PWA) de bajo consumo de datos.

#### Principio 7: Interoperabilidad mediante Estándares Abiertos (APIs)
- **Declaración:** Todos los módulos del sistema deben comunicarse mediante interfaces de programación de aplicaciones estandarizadas (APIs RESTful con payloads JSON).
- **Justificación:** La plataforma debe ser capaz de integrarse a futuro con pasarelas de pago (Bold, Wompi), facturación electrónica o sistemas de agencias externas sin rehacer el código central.
- **Implicación:** Se prohíbe el acoplamiento directo entre capas o el acceso sin control a la base de datos desde componentes externos; toda integración se normará mediante contratos de API documentados.

---

## 3. Evaluación de Madurez Arquitectónica Actual (AS-IS)

Para establecer la brecha entre el estado actual y el deseado, se evalúa a RutaConecta utilizando el modelo de madurez de capacidad arquitectónica (adaptación del modelo CMMI / TOGAF ACMM).

### 3.1. Estado de Madurez Global

- **Nivel Actual:** Nivel 1 – Inicial / Ad-hoc (Puntaje: 1.2 / 5.0)  
- **Nivel Objetivo (TO-BE a 12 meses):** Nivel 3 – Definido (Puntaje: 3.0 / 5.0)  

```
Nivel 1: Inicial (Ad-hoc)    ████████████░░░░░░░░  (1.2 / 5.0)  ◄── ESTADO ACTUAL
Nivel 2: Repetible           ░░░░░░░░░░░░░░░░░░░░
Nivel 3: Definido            ░░░░░░░░░░░░░░░░░░░░               ◄── OBJETIVO 12M
Nivel 4: Gestionado          ░░░░░░░░░░░░░░░░░░░░
Nivel 5: Optimizado          ░░░░░░░░░░░░░░░░░░░░
```

---

### 3.2. Evaluación Detallada por Dominios de Arquitectura

1. **Arquitectura de Negocio (Madurez: Nivel 1.5 - Ad-hoc / Informal)**
   - **Fortalezas:** Existe un modelo comercial validado con demanda activa de agencias B2B y una red funcional de proveedores locales.
   - **Debilidades / Brechas:** Los procesos clave (Cotización, Confirmación de Reservas y Liquidaciones) no están formalmente modelados. Dependen de la memoria del personal operativo y de chats informales en WhatsApp.

2. **Arquitectura de Datos (Madurez: Nivel 1.0 - Caótica)**
   - **Fortalezas:** Se recopila información operativa de manera constante en la nube (Google Drive).
   - **Debilidades / Brechas:** Ausencia total de un esquema relacional. Inexistencia de claves primarias o foráneas, alta redundancia de datos, riesgo de sobre-escritura accidental y falta de controles de integridad referencial.

3. **Arquitectura de Aplicaciones (Madurez: Nivel 1.0 - Inexistente)**
   - **Fortalezas:** Utilización de herramientas SaaS de colaboración (Google Workspace).
   - **Debilidades / Brechas:** No existe un software propio ni un core transaccional. La "aplicación" de la empresa es una malla artesanal de hojas de cálculo interconectadas manualmente, lo que satura la operación y genera cuellos de botella.

4. **Arquitectura de Infraestructura y Seguridad (Madurez: Nivel 1.2 - Básica)**
   - **Fortalezas:** Cero costo de mantenimiento de hardware físico al utilizar la nube de Google para almacenar archivos.
   - **Debilidades / Brechas:** Inexistencia de políticas de seguridad basadas en roles (RBAC). Carece de registros de auditoría (logs), copias de seguridad transaccionales automatizadas o mecanismos de recuperación ante desastres (DRP).

---

### 3.3. Brechas Prioritarias y Plan de Evolución

| Dominio | Situación Actual (AS-IS) | Brecha a Resolver | Acción Prioritaria (TO-BE) |
| :--- | :--- | :--- | :--- |
| **Negocio** | Cotizaciones demoran entre 24h y 48h por llamadas manuales. | Alta latencia operativa y pérdida de oportunidades de venta B2B. | Implementar motor de reglas de precios y disponibilidad instantánea (< 5 min). |
| **Datos** | Datos fragmentados en múltiples archivos de Excel en Drive. | Inconsistencia de datos, duplicidad y riesgo de overbooking. | Diseñar e implementar la Base de Datos Relacional Centralizada (SSOT). |
| **Aplicación** | Gestión manual de reservas y liquidaciones por tablas de Excel. | Proceso propenso a errores humanos y liquidaciones tardías a proveedores. | Desarrollar el Core Transaccional Web (Portal Agencias + Módulo de Liquidaciones). |
| **Seguridad** | Permisos globales a carpetas de Drive sin auditoría de cambios. | Riesgo de alteración deliberada o accidental de tarifas y registros contables. | Definición de autenticación centralizada y control de acceso por roles (RBAC). |
