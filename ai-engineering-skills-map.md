# Mapa de habilidades de ingeniería de IA (DeepLearning.AI)

Fuente: [https://www.deeplearning.ai/resources/ai-engineering-skills#map](https://www.deeplearning.ai/resources/ai-engineering-skills#map)  
Extraído del contenido de la página (no fue necesario usar Playwright). Última actualización indicada en la página: 17 de septiembre de 2026.

> Usa esta lista como autoevaluación: marca `[x]` las habilidades que ya dominas y `[ ]` las que te faltan o quieres reforzar.

## Versión en español

### 01. Construcción y despliegue de aplicaciones de IA
*Comprender los fundamentos de la IA e iterar con habilidad —construir, examinar resultados y decidir qué probar después— para crear sistemas confiables a partir de componentes de IA impredecibles.*

- [ ] **1.1 Fundamentos de los LLM** — Entender cómo funcionan los LLM ayuda a saber cuándo confiar en ellos, elegir modelos/parámetros y adaptarlos cuando sea necesario.
  - [ ] Aprender tokenización, ventanas de contexto, muestreo (*sampling*), razonamiento (*reasoning*) y uso de herramientas (*tool calling*).
  - [ ] Elegir el modelo o la combinación de modelos adecuada, y aplicar ajuste fino (*fine-tuning*) o autoalojamiento (*self-hosting*) cuando sea necesario.

- [ ] **1.2 Fundamentar modelos con datos** — Proporcionar a los modelos datos relevantes, precisos y actualizados les da el contexto que necesitan para generar resultados útiles.
  - [ ] Decidir qué incluir en el *prompt* frente a qué recuperar bajo demanda, y elegir la forma adecuada de representar y buscar los datos.
  - [ ] Construir pipelines para procesar documentos y mantener los datos limpios y actualizados.

- [ ] **1.3 Construcción de sistemas agénticos** — Diseñar flujos de trabajo predefinidos o agentes autónomos, dotándolos de las herramientas, memoria y gestión de contexto adecuadas, y hacerlos confiables, seguros y protegidos en producción.
  - [ ] Diseñar flujos de trabajo y agentes con las herramientas, memoria, gestión de contexto y mecanismos de respaldo (*fallbacks*) adecuados.
  - [ ] Hacerlos confiables, seguros y protegidos con barreras de seguridad (*guardrails*), mitigación de riesgos y gobernanza.

- [ ] **1.4 Desarrollo guiado por evaluación** — Guiar el desarrollo con un ciclo disciplinado de evaluaciones/análisis de errores para identificar qué mejorar y avanzar de forma sistemática.
  - [ ] Analizar trazas y salidas para decidir qué medir.
  - [ ] Elegir entre evaluaciones basadas en código, LLM como juez (*LLM-as-a-judge*) y evaluaciones humanas.
  - [ ] Usar el análisis de errores para decidir qué mejorar a continuación.

- [ ] **1.5 Operación en producción** — Operar sistemas de IA en producción implica observabilidad para seguir el rendimiento, detectar desviaciones/fallos/incidentes, probar cambios antes del lanzamiento y optimizar costo y latencia.
  - [ ] Implementar observabilidad, detección de deriva (*drift*), pruebas de regresión, CI/CD y respuesta a incidentes.
  - [ ] Optimizar costo y latencia mediante elección de modelo, destilación (*distillation*), ajuste fino y simplificación de flujos de trabajo.

- [ ] **1.6 Fundamentos de aprendizaje automático** — Comprender el aprendizaje automático ayuda a construir mejor con LLM, elegir y entrenar modelos cuando la aplicación lo requiera y razonar sobre sistemas con salidas inciertas.
  - [ ] Aprender aprendizaje supervisado y por refuerzo, y los compromisos (*tradeoffs*) de los modelos populares de ML y aprendizaje profundo.
  - [ ] Practicar la ingeniería de datos de entrenamiento y evaluación, análisis de sesgo/varianza (*bias/variance*) y análisis de errores.

### 02. Fundamentos de ingeniería de software
*Comprender los fundamentos del software para orientar a los agentes de codificación hacia los compromisos adecuados de latencia, fiabilidad, mantenibilidad y costo para tu aplicación.*

- [x] **2.1 Construcción de aplicaciones full-stack** — Entender toda la pila de la aplicación ayuda a usar agentes de codificación en sistemas front-end y back-end y a tomar decisiones informadas.
  - [x] Aprender fundamentos de front-end y back-end: componentes de UI, APIs, autenticación, gestión de estado, persistencia de datos y seguridad.
  - [x] Construir en toda la pila con agentes de codificación comprendiendo cómo funciona cada capa.

- [x] **2.2 Gestión de datos** — Saber gestionar datos ayuda a elegir modelos, almacenamiento e infraestructura adecuados, mantenerlos limpios, consistentes y cumplir normativas, y dar contexto relevante a los sistemas de IA.
  - [x] Aprender modelado de datos, patrones de acceso, tipos de almacenamiento, transacciones y concurrencia.
  - [x] Mantener calidad de datos, privacidad y gobernanza, y adaptar la arquitectura de datos a medida que evolucionan la aplicación y sus sistemas de IA.

- [x] **2.3 Diseño de arquitecturas de sistemas** — Comprender los componentes de software y las necesidades de la aplicación ayuda a diseñar la arquitectura correcta y elegir la pila tecnológica, y evolucionarlas con el tiempo.
  - [x] Estudiar descomposición del sistema, ubicación del estado, elección de plataforma y compromisos entre monolito y microservicios.
  - [x] Evaluar opciones tecnológicas mediante  (I+D, POC, Pragmatismo) y evolucionar las arquitecturas desde el prototipo hasta producción y escala.

- [x] **2.4 Hacer los sistemas seguros y confiables** — Saber probar el sistema, diseñar ante fallos e incorporar seguridad desde el inicio ayuda a crear software correcto y con menos vulnerabilidades.
  - [x] Desarrollar estrategias de pruebas con pruebas unitarias y de integración adecuadas(Mutation checks).
  - [x] Diseñar ante fallos con degradación elegante y un radio de impacto limitado.
  - [x] Aplicar prácticas de seguridad desde etapas tempranas, usando herramientas de IA para revisar código, dependencias y configuración en la nube.

- [ ] **2.5 Escalado y operación en producción** — Saber desplegar, operar y escalar software ayuda a atender usuarios reales de forma confiable y mantener el sistema a medida que crece.
  - [ ] Configurar entornos de despliegue, elegir estrategias de lanzamiento y automatizar despliegues con CI/CD.
  - [ ] Practicar observabilidad, alertas y respuesta a incidentes, y escalar con balanceo de carga, particionamiento (*sharding*) y replicación.
  - [x] Usar control de versiones y revisiones de código, mantener dependencias y gestionar la deuda técnica.

### 03. Uso de agentes de codificación
*Saber personalizar, dirigir y orquestar agentes de codificación para optimizar el flujo de desarrollo manteniendo el control sobre lo que se construye y su calidad.*

- [x] **3.1 Dirigir el flujo de trabajo** — Conducir los proyectos decidiendo estratégicamente cuánto esfuerzo dedicar a la planificación, la arquitectura y la delegación en agentes.
  - [x] Equilibrar velocidad, costo y riesgo técnico al dirigir el flujo de trabajo de codificación.
  - [x] Elegir estratégicamente cuánto tiempo y esfuerzo invertir en investigación, planificación, iteración y revisión (SDD), y cuánta autonomía otorgar a los agentes.
  - [x] Planificar el trabajo definiendo objetivos finales claros y medibles, descomponiendo el trabajo en piezas verificables y creando artefactos de planificación.

- [x] **3.2 Habilitar la autonomía del agente** — Permitir que los agentes asuman trabajo de forma independiente manteniendo calidad y control.
  - [x] Elegir el nivel adecuado de autonomía para los agentes, desde una guía interactiva hasta la ejecución autónoma de tareas de larga duración.
  - [x] Escalar el trabajo autónomo gestionando el contexto, registrando aprendizajes clave y/u orquestando agentes en paralelo.
  - [x] Aplicar permisos y salvaguardas adecuados para limitar el riesgo de pérdida de datos, fugas u otros daños.

- [ ] **3.3 Revisar el trabajo** — Revisar el trabajo del agente garantiza que cumpla con los estándares funcionales, de comportamiento, seguridad y fiabilidad.
  - [x] Diseñar pruebas, validación, conjuntos de evaluación y análisis de errores adecuados a la tarea.
  - [x] Combinar pruebas automatizadas, revisión de código por agentes, auditorías de seguridad y arquitectura, y revisión humana cuando sea necesario.
  - [ ] Verificar despliegues y operacionalizar el monitoreo y la gestión de incidentes con agentes.

- [x] **3.4 Personalizar el agente y su entorno** — Personalizar un agente y su entorno ayuda a proporcionarle el contexto y las herramientas que necesita para trabajar eficazmente.
  - [x] Configurar habilidades (*skills*), complementos (*plugins*), servidores MCP, *hooks*, herramientas y permisos del agente.
  - [x] Mantener contexto permanente del código, incluyendo supuestos arquitectónicos, convenciones y patrones de acceso a datos.
  - [x] Preservar el estado entre sesiones y agentes, capturar aprendizajes y eliminar la deuda técnica generada por los agentes.

- [x] **3.5 Fundamentos de los agentes de codificación** — Comprender cómo funcionan los agentes de codificación ayuda a reconocer sus limitaciones y modos de fallo.
  - [x] Aprender cómo buscan en el código, gestionan el contexto, usan herramientas, interactúan con subagentes y cómo funciona el arnés (*harness*) del agente.
  - [x] Estudiar modos de fallo comunes (sobreingeniería, detenerse antes de alcanzar el objetivo, etc.) e intervenir cuando el agente se desvía.

### 04. Dar forma a la construcción (*Shaping the build*)
*Asumir la responsabilidad de proyectos de principio a fin e impulsar ciclos rápidos de construcción para entregar valor real mediante buen criterio de producto, alta agencia y liderazgo transversal.*

- [ ] **4.1 Impulsar el ciclo de construcción** — Mantener el desarrollo en movimiento usando cada iteración para aprender y reducir la incertidumbre.
  - [x] Pasar rápidamente de ideas a prototipos (MVP) y finalmente a sistemas en producción.
  - [x] Entregar en lotes pequeños y usar retroalimentación de usuarios o experimentos técnicos para guiar los siguientes pasos.
  - [x] Tomar decisiones basadas en el contexto del proyecto: visión del producto, viabilidad, riesgos, esfuerzo y presupuesto.
  - [ ] Definir métricas para medir e impulsar mejoras en el producto.

- [x] **4.2 Tomar decisiones de producto** — Desarrollar criterio de producto, negocio y empatía con el usuario ayuda a construir soluciones que satisfagan necesidades reales.
  - [x] Desarrollar sentido de producto, diseño y negocio.
  - [x] Construir empatía y comprensión del usuario mediante entrevistas, encuestas, pruebas A/B y análisis de comportamiento.
  - [x] Equilibrar necesidades del usuario, viabilidad, oportunidad de mercado y economía.

- [ ] **4.3 Comunicar y liderar** — Las habilidades de comunicación y liderazgo ayudan a alinear a las partes interesadas y guiar a la organización.
  - [ ] Alinear a las partes interesadas y comunicarse claramente con usuarios y tomadores de decisiones.
  - [ ] Colaborar con equipos multifuncionales, incluidos ingeniería, producto, marketing, finanzas y legal.
  - [ ] Explicar conceptos técnicos, restricciones y oportunidades a partes interesadas no técnicas.

- [ ] **4.4 Propiedad con alta agencia (*High agency ownership*)** — Impulsar proyectos de principio a fin, actuar con decisión ante la ambigüedad y persistir frente a los contratiempos.
  - [ ] Asumir iniciativas de principio a fin, persistir ante obstáculos y medir los resultados por el valor creado.
  - [ ] Identificar oportunidades, proponer soluciones y actuar sin esperar instrucciones detalladas.
  - [ ] Priorizar según el impacto y las restricciones organizacionales.