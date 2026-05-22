# Registro de Trabajo en Clase - Taller 7

## Fecha de la sesión

16 de mayo de 2026

## Integrantes presentes

- Mariana Valle 
- Valentina Lopez
- Camila Leon

## Actividades realizadas en clase

Durante la sesión se trabajó en la integración de las vistas arquitectónicas del caso base **FarmApp**, una cadena de farmacias con canales físicos y digitales integrados. El equipo revisó el contexto del caso, identificó los elementos principales de cada vista y organizó la información en un tablero visual.

Se discutió cómo los procesos de negocio, los datos, las aplicaciones, la infraestructura y la seguridad se relacionan entre sí para soportar la operación del e-commerce de FarmApp. A partir de esta revisión, se decidió organizar el modelo por capas, iniciando desde la vista de negocio y descendiendo hacia información, aplicaciones e infraestructura. Además, se definió representar la seguridad como una capa transversal, ya que aplica a todos los niveles de la arquitectura.

Las principales decisiones de modelado fueron:

- Representar la arquitectura en cinco vistas: negocio, información, aplicaciones, infraestructura y seguridad.
- Ubicar los procesos de negocio en la parte superior del tablero, ya que son los que explican la necesidad de las demás capas.
- Relacionar los procesos de compra, pago, despacho y seguimiento con entidades de información como cliente, producto, pedido, pago, inventario y entrega.
- Conectar las entidades de información con las aplicaciones que las gestionan, como app móvil, plataforma web, POS, CRM, inventario, pasarela de pagos y sistema logístico.
- Mostrar que las aplicaciones dependen de componentes de infraestructura como nube híbrida, servidores regionales, base de datos replicada, red segura y monitoreo.
- Incluir la seguridad como una vista transversal mediante controles de acceso, cifrado, auditoría, monitoreo de fraude y respaldo.

La herramienta utilizada para desarrollar el tablero fue **draw.io**, ya que permite organizar visualmente las capas, conectar elementos mediante flechas y ajustar el modelo de forma colaborativa. Durante la clase se alcanzó a construir el boceto inicial del tablero integrado de FarmApp y a definir la lógica de conexión entre las vistas.

## Boceto inicial del modelo

El boceto inicial fue desarrollado en formato digital usando **draw.io**. El modelo se organizó en capas para facilitar la lectura de la arquitectura:

```text
NEGOCIO
Compra online | Validación de prescripción | Pago | Despacho | Seguimiento

↓ se apoya en

INFORMACIÓN
Cliente | Producto | Pedido | Pago | Inventario | Entrega | Historial

↓ es gestionada por

APLICACIONES
App móvil | E-commerce web | POS | CRM | Inventario | Pasarela de pagos | Logística

↓ se ejecutan sobre

INFRAESTRUCTURA
Nube híbrida | Servidores regionales | Base de datos replicada | Red segura | Monitoreo

↔ protegida por

SEGURIDAD
Roles | Cifrado | Auditoría | Monitoreo de fraude | Respaldo
```

La lectura del modelo permite entender que los procesos de negocio de FarmApp requieren información confiable, aplicaciones integradas, infraestructura disponible y controles de seguridad que protejan la operación y los datos de los clientes.

## Tareas definidas para complementar el taller

| Tarea asignada | Responsable | Fecha estimada |
|----------------|-------------|----------------|
| Ajustar el tablero final en draw.io | Mariana Valle | 21/05 |
| Hacer tablero sobre el cliente real | Valentina Lopez | 21/05 |
| Redactar notas.md | Mariana Valle | 21/05 |
| Redactar informe.md | Valentina Lopez | 21/05 |
| Hacer investigación | Camila Leon | 22/05 |
| Agregar taller completo al git hub del proyecto | Camila Leon | 22/05 |

---

_Este documento resume el trabajo colaborativo realizado durante la sesión del Taller 7 en el curso AREM - Universidad de La Sabana._
