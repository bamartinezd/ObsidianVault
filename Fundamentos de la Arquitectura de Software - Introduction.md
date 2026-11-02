

Este documento resume los conceptos clave sobre el rol, responsabilidades y componentes de la arquitectura de software extraídos de las transcripciones proporcionadas.

---

## 1. Las 8 Expectativas Centrales de un Arquitecto de Software

Independientemente del título o la descripción del puesto, se espera que un arquitecto cumpla con ocho responsabilidades principales:

1. **Tomar decisiones arquitectónicas:** Definir las decisiones de diseño y principios que guíen al equipo. El enfoque debe ser **guiar en lugar de especificar** la tecnología exacta (por ejemplo, instruir el uso de un _"framework basado en componentes reactivos"_ en vez de imponer estrictamente React, Angular o Vue).
2. **Analizar continuamente la arquitectura:** Evaluar la viabilidad del entorno tecnológico y del negocio de forma continua. La falta de análisis genera **degradación estructural**, afectando la escalabilidad, disponibilidad y rendimiento a medida que los desarrolladores modifican el código.
3. **Mantenerse al día con las últimas tendencias:** Seguir de cerca los cambios de la industria, ya que las decisiones arquitectónicas tienden a ser de largo plazo y costosas de cambiar.
4. **Garantizar el cumplimiento de las decisiones:** Verificar continuamente que las soluciones sigan las pautas establecidas, apoyándose en herramientas automatizadas y funciones de aptitud (_fitness functions_).
5. **Exposición y experiencia diversa:** Priorizar la **amplitud técnica sobre la profundidad técnica**. Es más valioso conocer diez soluciones de almacenamiento/caché con sus pros y contras que ser experto absoluto en solo una.
6. **Poseer conocimiento del dominio de negocio:** Comprender los términos, objetivos y problemas del negocio para comunicarse de manera efectiva con los ejecutivos (C-level) y no perder credibilidad.
7. **Poseer habilidades interpersonales excepcionales:** Liderar, guiar y mediar en los equipos de desarrollo. Como dice la máxima de Gerald Weinberg: _"No importa lo que te digan, siempre es un problema de personas"_.
8. **Comprender y navegar la política corporativa:** Casi todas las decisiones arquitectónicas serán desafiadas por desarrolladores, gerentes o clientes debido a costos o tiempos. Se requiere una gran capacidad de negociación.

---

## 2. Las Dos Leyes de la Arquitectura de Software

- **Primera Ley:** _Todo en la arquitectura de software es un intercambio (trade-off)._ Si crees que has encontrado una solución que no tiene desventajas, es probable que aún no las hayas identificado.
- **Segunda Ley:** _El "por qué" es más importante que el "cómo"._ Documentar las razones detrás de una decisión (por ejemplo, mediante Registros de Decisión Arquitectónica o ADRs) es fundamental, ya que el código o los diagramas muestran la estructura final, pero no los motivos de las elecciones.

---

## 3. ¿Qué compone la Arquitectura de Software?

La arquitectura no es simplemente un plano estático, sino una estructura dinámica definida por la combinación de cuatro dimensiones:

| Componente                          | Descripción                                                                                                                                                                                                |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Estructura**                      | El estilo arquitectónico utilizado en el sistema (ej. microservicios, capas, microkernel, monolito modular).                                                                                               |
| **Características Arquitectónicas** | Atributos de calidad ("ilities") independientes de las funciones del negocio (ej. escalabilidad, disponibilidad, tolerancia a fallos, rendimiento, seguridad, elasticidad, desplegabilidad, testabilidad). |
| **Decisiones Arquitectónicas**      | Las reglas duras e inflexibles que delimitan cómo se construye el sistema (ej. prohibir que la capa de presentación acceda directamente a la base de datos).                                               |
| **Principios de Diseño**            | Directrices orientativas que guían al desarrollador, pero permiten flexibilidad en su implementación (ej. _"priorizar la mensajería asíncrona entre microservicios para mejorar el rendimiento"_).         |

### El Concepto de Variación (Variance)

Cuando una regla estricta no puede cumplirse por restricciones extraordinarias, se solicita una **variación** evaluada por un Comité de Revisión Arquitectónica (ARB - _Architecture Review Board_) o el Arquitecto Jefe.

---

## 4. 📋 Clasificación Detallada de Características Arquitectónicas (_ilities_)

### 1. Operacionales (Cómo funciona en producción)

- **Availability (Disponibilidad):** Garantía del tiempo en que el sistema responde y está operativo.
- **Continuity (Continuidad):** Capacidades de recuperación ante desastres a gran escala para asegurar la continuidad del negocio.
- **Performance (Rendimiento):** Eficiencia en tiempos de respuesta, latencia y uso de recursos bajo carga.
- **Recoverability (Recuperabilidad):** Qué tan rápido y limpio vuelve a operar el sistema después de una caída.
- **Reliability / Safety (Fiabilidad):** Capacidad de operar sin fallos críticos en escenarios donde un error cuesta vidas o grandes sumas financieras.
- **Robustness (Tolerancia a fallos):** Resiliencia ante fallas de infraestructura externa como caídas de red, cortes de luz, etc.
- **Scalability (Escalabilidad):** Capacidad del sistema para manejar un volumen creciente de trabajo agregando recursos.
- **Elasticity (Elasticidad):** Capacidad de escalar y desescalar recursos de manera automática según la demanda en tiempo real.

### 2. Estructurales (Cómo está hecho el código)

- **Configurability (Configurabilidad):** Facilidad para modificar aspectos del comportamiento del software sin reescribir código.
- **Extensibility (Extensibilidad):** Capacidad de incorporar nuevas funcionalidades con el mínimo impacto en la estructura existente.
- **Installability (Facilidad de instalación):** Simplicidad del proceso de despliegue del sistema en sus entornos correspondientes.
- **Leverageability / Reuse (Reutilización):** Habilidad para aprovechar componentes existentes en múltiples partes del sistema o en otros productos.
- **Localization (Localización):** Soporte nativo para múltiples idiomas, formatos culturales y regulaciones de diferentes países.
- **Maintainability (Mantenibilidad):** Facilidad para corregir errores, realizar optimizaciones o aplicar refactorizaciones al código base.
- **Portability (Portabilidad):** Capacidad de ejecutar el software en diferentes entornos operativos o plataformas de infraestructura.
- **Supportability (Soportabilidad):** Facilidad para monitorear, rastrear errores, extraer logs y diagnosticar problemas en producción.
- **Upgradeability (Facilidad de actualización):** Simplicidad y seguridad para migrar el sistema hacia versiones más recientes de componentes o dependencias.

### 3. Transversales / Cross-cutting (Afectan todo el sistema)

- **Accessibility (Accesibilidad):** Cumplimiento de estándares que permiten el uso del software por personas con diversas capacidades.
- **Archivability (Archivabilidad):** Directrices y mecanismos para el archivado, purga o borrado seguro de datos históricos de acuerdo al negocio.
- **Authentication (Autenticación):** Mecanismos para verificar la identidad de los usuarios o sistemas que interactúan con la plataforma.
- **Authorization (Autorización):** Control de acceso para asegurar que las entidades autenticadas solo realicen acciones permitidas.
- **Legal (Cumplimiento legal):** Adherencia a normativas, leyes y regulaciones locales e internacionales aplicables a la industria.
- **Privacy (Privacidad de datos):** Protección y manejo confidencial de los datos personales y sensibles del usuario.
- **Security (Seguridad):** Medidas globales para proteger al sistema contra accesos no autorizados, ataques cibernéticos y vulnerabilidades.
- **Usability / Achievability (Usability):** Facilidad de uso y claridad de la interfaz para que el usuario cumpla sus metas de forma intuitiva.

---

## 5. Intersecciones con Prácticas de Ingeniería y Operaciones

### Prácticas de Ingeniería y Arquitectura Evolutiva

- **Proceso vs. Práctica:** El proceso se refiere a cómo se organiza el equipo (ej. Scrum), mientras que las prácticas de ingeniería son agnósticas al proceso y repetibles (ej. Integración Continua, TDD).
- **Arquitectura Evolutiva:** Introduce las **funciones de aptitud (fitness functions)** como mecanismos automáticos (pruebas unitarias, métricas, monitores) integrados en la tubería de CI/CD para asegurar que el sistema no degrade sus características arquitectónicas (como el tiempo de carga de página) con el paso del tiempo.

### DevOps y Operaciones

Históricamente, las operaciones se externalizaban, forzando a los arquitectos a diseñar soluciones muy complejas para manejar la infraestructura de manera defensiva internamente. La revolución DevOps unió el desarrollo con las operaciones, simplificando arquitecturas modernas como los microservicios, los cuales delegan la **escala elástica** a la infraestructura o proveedores de la nube.

---

_Nota de contexto histórico:_ Casos de estudio como **pets.com** en los inicios del internet demostraron que "demasiado éxito puede destruir un negocio" si no se cuenta con una infraestructura de escala elástica capaz de soportar la demanda, lo que impulsó a los ingenieros a crear las herramientas y prácticas automatizadas que disfrutamos hoy en día.