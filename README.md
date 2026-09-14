# Ecosistema de Automatización IA — Gestión de Reclamos Fintech

Trabajo final del curso de Automatización con IA. Sistema autónomo que gestiona reclamos por
transacciones no reconocidas de extremo a extremo: recibe el reclamo, lo clasifica con IA, espera
validación humana (HITL) antes de cualquier acción crítica, y comunica el resultado final al cliente.

## Stack utilizado

| Componente         | Tecnología                          |
|---------------------|--------------------------------------|
| Orquestador          | n8n (Cloud)                          |
| Base de datos         | Airtable (2 tablas relacionadas: Clientes y Reclamos) |
| Procesamiento IA      | Groq API (modelo openai/gpt-oss-20b) |
| Canal de salida       | Gmail                                |

## Arquitectura

El sistema está dividido en **2 workflows independientes**:

- **Workflow 1 — Procesamiento y clasificación:** busca reclamos pendientes, los analiza con IA,
  y notifica a un humano para su aprobación. Incluye manejo de errores con reintentos automáticos (x3)
  y una ruta de error dedicada que registra cualquier fallo sin interrumpir el sistema.
- **Workflow 2 — Validación y despacho:** revisa periódicamente qué reclamos fueron aprobados por un
  humano (checkbox `Aprobado`) y recién ahí envía la comunicación final al cliente.

El diagrama completo de arquitectura, con el detalle técnico de cada nodo, está en
[`Trabajo_Final_Arquitectura.pdf`](./Trabajo_Final_Arquitectura.pdf).

## Contenido del repositorio

- `Trabajo_Final_Arquitectura.pdf` — Diagrama de arquitectura y documentación técnica completa.
- `workflow1.json` — Blueprint exportado del Workflow 1 (n8n).
- `workflow2.json` — Blueprint exportado del Workflow 2 (n8n).
- Capturas de pantalla — Evidencia de ejecución, outputs de la IA, manejo de errores y estado final
  de los registros en Airtable.

## Base de datos (modo lectura)

Podés ver la estructura y los datos de prueba acá:
https://airtable.com/appk66TkkmDFTturs/shr4YjsMykdOXNQGB

## Pruebas realizadas

El sistema fue ejecutado más de 5 veces con distintos escenarios: camino feliz (reclamos completos),
datos faltantes (mensaje vacío), fallo forzado de la API de IA (para probar la resiliencia), y
verificación del filtro anti-bucle infinito. El detalle completo de cada prueba está documentado en
el PDF de arquitectura.
