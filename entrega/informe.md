# Informe Narrativo — Integración de Vistas Arquitectónicas
## Cliente real: Logística de Aplicación de la Encuesta de Autoevaluación Institucional y por Programas
### Universidad de La Sabana · Arquitectura Empresarial · 2026

**Equipo:** Valentina López · Mariana Valle · Camila León

---

## 1. Introducción

Este informe documenta la integración de las cinco vistas arquitectónicas —negocio, información, aplicaciones, infraestructura y seguridad— aplicadas al cliente real del proyecto: la **Dirección de Desarrollo Estratégico** de la Universidad de La Sabana, específicamente la jefatura de **Cultura, Innovación y Servicio**, responsable de la logística de aplicación de la **Encuesta de Autoevaluación Institucional y por Programas**.

El objetivo de esta integración es mostrar cómo los entregables desarrollados a lo largo del curso —BPMN, ERD, mapa de infraestructura, análisis STRIDE, cumplimiento normativo y análisis de riesgos— se articulan en una arquitectura coherente, evidenciando las brechas del estado actual (AS-IS) y fundamentando la propuesta de mejora (TO-BE).

---

## 2. Vista de negocio

### 2.1 Procesos identificados

El BPMN elaborado como primer entregable permitió delimitar el alcance del proceso en cuatro etapas secuenciales:

| Proceso | Descripción | Actor responsable |
|---|---|---|
| Recolección de información | Citación al director, reunión de coordinación, envío y diligenciamiento del formato Excel | Coordinadora de Encuestas / Director de Programa |
| Planeación logística | Asignación manual de horarios, salones y estudiantes PAT con base en la información consolidada | Coordinadora de Encuestas (Johanna) |
| Aplicación de la encuesta | Estudiante PAT aplica la encuesta presencialmente en el salón mediante código QR | Estudiantes PAT |
| Entrega de resultados | El proveedor externo procesa los datos y genera informes para el CNA y la institución | Proveedor externo / Área de Autoevaluación |

### 2.2 Observación clave

El proceso de negocio revela una **dependencia crítica en una sola persona**: la coordinadora (Johanna) es el único punto de control en las dos primeras etapas. Esta decisión arquitectónica del AS-IS genera un cuello de botella estructural que, desde la perspectiva de negocio, no es sostenible. Si la coordinadora no está disponible, el proceso colapsa.

---

## 3. Vista de información

### 3.1 Entidades del modelo ERD

El modelo entidad-relación construido como segundo entregable identificó las siguientes entidades principales del proceso:

| Entidad | Atributos clave | Problema en AS-IS |
|---|---|---|
| Programa | Facultad, nombre, director asignado | Información llega en formatos distintos por cada director |
| Docente | ID, nombre, programas a los que pertenece | Un mismo docente puede estar en varios programas sin relación formal |
| Asignatura | Horario, salón, semestre, cupos | Datos dispersos en múltiples archivos Excel |
| Cronograma | Fechas de aplicación, bloques, sesiones PAT | Construido manualmente sin estructura centralizada |
| PAT | Disponibilidad, asignación de salón | Cruce manual con horarios, propenso a errores |

### 3.2 Observación clave

La entidad **Asignatura** es el nodo central del modelo: articula docentes, programas, horarios y la asignación de estudiantes PAT. En el AS-IS, esta entidad no existe formalmente en ningún sistema; vive distribuida en correos electrónicos y archivos Excel heterogéneos. Esta ausencia de estructura formal en los datos es la **causa raíz** de la mayoría de los riesgos identificados.

---

## 4. Vista de aplicaciones

### 4.1 Estado actual (AS-IS)

| Herramienta | Uso actual | Problema |
|---|---|---|
| Correo Outlook | Citaciones, envío del formato Excel, recordatorios manuales a profesores | Sin trazabilidad ni registro centralizado de intercambios |
| Excel / OneDrive | Consolidación de datos académicos y planeación logística | Sin control de versiones, acceso no restringido, propenso a errores al copiar |

### 4.2 Propuesta TO-BE

| Herramienta | Uso propuesto | Beneficio |
|---|---|---|
| Microsoft Forms | Captura estructurada de información académica por programa | Elimina los formatos heterogéneos; valida datos desde el origen |
| Power Automate | Automatización del flujo: recepción → validación → notificación → almacenamiento | Reduce la carga manual de la coordinadora; genera trazabilidad automática |
| SharePoint | Repositorio centralizado con control de acceso por rol | Centraliza la información; restringe modificaciones no autorizadas |
| Excel Online | Planeación logística sobre datos ya validados y centralizados | Mantiene la herramienta conocida por el equipo pero con datos confiables |

### 4.3 Decisión arquitectónica clave

Se decidió **no reemplazar toda la tecnología existente**, sino aprovechar la **Suite Microsoft 365** con la que ya cuenta la universidad. Esta decisión fue fundamental porque elimina barreras de adopción, no requiere presupuesto adicional en licencias y permite una transición gradual del AS-IS al TO-BE sin interrumpir la operación actual.

---

## 5. Vista de infraestructura

### 5.1 Componentes disponibles

El mapa de infraestructura elaborado como tercer entregable identificó que la universidad ya dispone de:

| Componente | Estado | Rol en el TO-BE |
|---|---|---|
| Correo institucional (@unisabana.edu.co) | Activo | Canal de autenticación y comunicación oficial |
| Licencias Microsoft 365 | Activas para toda la comunidad | Plataforma base del TO-BE (Forms, Power Automate, SharePoint) |
| SharePoint / OneDrive institucional | Disponible, subutilizado | Repositorio central de información del proceso |
| Proveedor externo de encuestas | Activo, contrato vigente | Mantiene el rol de procesamiento y generación de informes QR |

### 5.2 Observación clave

La infraestructura disponible es suficiente para soportar el TO-BE **sin inversión adicional**. El problema no es la falta de tecnología, sino la falta de uso estructurado de la tecnología ya disponible. Esta conclusión fue determinante para la propuesta: la solución es organizativa y de configuración, no de adquisición.

---

## 6. Vista de seguridad

### 6.1 Amenazas STRIDE identificadas

El análisis de seguridad con el marco STRIDE identificó seis categorías de amenazas aplicadas al proceso:

| Categoría STRIDE | Amenaza principal en el cliente | Riesgo | Control propuesto |
|---|---|---|---|
| Spoofing | Suplantación de un director al enviar información falsa por correo | Alto | Autenticación institucional obligatoria + MFA en Forms |
| Tampering | Modificación no autorizada de archivos Excel compartidos | Alto | Control de versiones en SharePoint + permisos por rol |
| Repudiation | Un director niega haber enviado cierta información | Medio | Logs de auditoría en Power Automate con timestamp |
| Information Disclosure | Archivos con datos de docentes o estudiantes compartidos sin cifrado | Alto | Segmentación de accesos en SharePoint; evitar envíos por correo |
| Denial of Service | Saturación de Forms durante la aplicación de la encuesta | Medio | Restricción de respuestas por usuario autenticado |
| Elevation of Privilege | Un estudiante PAT accede a listados completos de programas | Alto | Principio de mínimo privilegio; cuentas con permisos mínimos |

### 6.2 Cumplimiento normativo

El análisis normativo evidenció brechas frente a la **Ley 1581 de Protección de Datos Personales** de Colombia:

- El titular de los datos no tiene un canal claro para consultar, modificar o eliminar su información.
- No existe control formal sobre quién accede a los archivos que contienen datos personales de docentes y estudiantes.
- El proveedor externo no tiene un acuerdo de seguridad de la información formalmente definido.

Estas brechas deben ser resueltas en el TO-BE mediante la implementación de controles de acceso por rol, acuerdos de tratamiento de datos con el proveedor y la creación de un canal de gestión de datos personales.

---

## 7. Articulación entre vistas: cómo se relacionan

La integración de las cinco vistas permite entender que los problemas del proceso no son aislados sino **sistémicos**. La siguiente tabla muestra cómo cada entregable alimenta a los siguientes:

| Relación entre vistas | Articulación identificada |
|---|---|
| Negocio → Información | El proceso de recolección define qué entidades se necesitan (Programa, Docente, Asignatura). Sin ese proceso claro, el ERD no tiene base real. |
| Información → Aplicaciones | Las entidades del ERD determinan qué campos debe capturar el formulario (Forms) y qué datos debe manejar SharePoint. |
| Aplicaciones → Infraestructura | La propuesta de Forms + Power Automate + SharePoint solo es viable porque la infraestructura Microsoft 365 ya existe en la universidad. |
| Infraestructura → Seguridad | Los controles STRIDE se implementan sobre los componentes de infraestructura: permisos en SharePoint, autenticación en Outlook, logs en Power Automate. |
| Seguridad → Negocio (retroalimentación) | Los controles de seguridad impactan directamente el proceso de negocio: agregan pasos de validación y auditoría que antes no existían, haciendo el proceso más confiable pero también más formal. |

### Flujo vertical del proceso de recolección de información

```
NEGOCIO       La coordinadora inicia el proceso y envía un formulario estandarizado al director
      ↓
INFORMACIÓN   Se capturan las entidades: Programa + Asignatura + Docente + Horario + Salón
      ↓
APLICACIONES  AS-IS: correo + Excel manual / TO-BE: Microsoft Forms + Power Automate
      ↓
INFRAESTRUCTURA  Microsoft 365 disponible: Forms, SharePoint, Power Automate, Exchange
      ↓
SEGURIDAD     Autenticación institucional + permisos por rol + logs de auditoría + Ley 1581
```

---

## 8. Decisiones arquitectónicas clave

A lo largo del proyecto se tomaron tres decisiones que determinan la coherencia de la arquitectura propuesta:

**Decisión 1 — Aprovechar la infraestructura existente.**
En lugar de proponer un sistema nuevo o una base de datos independiente, se decidió usar la Suite Microsoft 365 ya disponible. Esto garantiza viabilidad económica y facilita la adopción por parte de los usuarios actuales.

**Decisión 2 — Estandarizar la captura desde el origen.**
El principal problema del AS-IS no es la consolidación, sino la heterogeneidad de la información en el origen. Microsoft Forms resuelve esto al obligar a todos los directores a usar el mismo formato con validaciones automáticas, eliminando el problema antes de que ocurra.

**Decisión 3 — Separar la responsabilidad de una sola persona.**
La arquitectura TO-BE distribuye las responsabilidades que hoy recaen exclusivamente en la coordinadora: Power Automate se encarga de la validación y enrutamiento automático, SharePoint del almacenamiento con control de acceso, y Excel Online de la planeación sobre datos ya confiables. La coordinadora pasa de ser ejecutora a supervisora del proceso.

---

## 9. Reflexión crítica sobre la coherencia de la arquitectura

### 9.1 Fortalezas del modelo integrado

- **Coherencia vertical:** Cada capa depende lógicamente de la anterior. No se propone una aplicación sin infraestructura que la soporte, ni una entidad de datos sin un proceso de negocio que la justifique.
- **Viabilidad real:** La propuesta no requiere herramientas externas ni presupuesto adicional. Usa lo que ya existe, lo que aumenta la probabilidad de implementación efectiva.
- **Trazabilidad completa:** El TO-BE introduce logs automáticos en Power Automate y control de versiones en SharePoint, cerrando la brecha de trazabilidad que fue el riesgo más frecuente en el análisis.

### 9.2 Debilidades y riesgos de la arquitectura propuesta

- **Dependencia de adopción:** Si los directores de programa no usan el formulario y siguen enviando Excel por correo, el modelo colapsa. La arquitectura técnica no puede resolver un problema de cultura organizacional.
- **Gobernanza no definida:** El taller 6 (Gobernanza) quedó pendiente en el repositorio. Sin definir quién administra los formularios, los permisos de SharePoint y los flujos de Power Automate, la solución puede desordenarse con el tiempo.
- **Proveedor externo sin acuerdo formal:** El proveedor que gestiona los links QR y los informes finales no tiene un acuerdo de seguridad de la información documentado, lo que genera un riesgo de exposición de datos fuera del control de la universidad.
- **Riesgo de parametrización inicial:** Si la estructura de Programas, Docentes, Asignaturas y PAT no queda bien definida desde el inicio en SharePoint, la solución perderá efectividad desde el primer ciclo de uso.

### 9.3 Conclusión

La arquitectura propuesta es internamente coherente: cada vista se articula con las demás y las decisiones tomadas en una capa tienen justificación en las demás. Sin embargo, la coherencia técnica no es suficiente. Para que el modelo TO-BE sea sostenible, la universidad necesita complementarlo con una estrategia de gobernanza de datos, un proceso de cambio organizacional que asegure la adopción, y acuerdos formales con el proveedor externo.

El mayor aprendizaje del proceso es que **la arquitectura empresarial no es solo tecnología**: es la forma en que una organización estructura sus procesos, sus datos y sus sistemas para alcanzar sus objetivos estratégicos. En el caso de la Universidad de La Sabana, el objetivo estratégico es garantizar la acreditación institucional ante el CNA, y ese objetivo solo es posible si el proceso de autoevaluación es confiable, trazable y libre de los riesgos que hoy lo amenazan.

---


_Documento elaborado como parte del Taller 07 — Integración de Vistas de Arquitectura Empresarial._  
_Universidad de La Sabana · 2026 · Valentina López · Mariana Valle · Camila León_
_Este documento hace parte de la entrega del taller X del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
