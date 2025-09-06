# Actividad 1: Introducción devops, devsecops

**Nombre:** Anthony Aldair Carlos Ramon\
**Fecha:** 05/09/2025

## 4.1 DevOps vs Cascada

### ¿Por qué DevOps acelera y reduce riesgo en software para la nube frente a cascada (feedback continuo, pequeños lotes, automatización)?

**Feedback continuo**

- **Cascada:** el cliente o usuario solo da retroalimentación (_feedback_) al final del proyecto. Si hay errores o malentendidos, se descubren muy tarde, alto riesgo y costo de corrección.
- **DevOps:** cada despliegue, prueba automatizada y monitoreo genera _feedback_ inmediato. Los problemas se detectan temprano y se corrigen rápido.
 Esto acelera el desarrollo porque no se espera meses para validar.

**Pequeños lotes de cambio**

- **Cascada:** los cambios se acumulan en grandes entregas. Cada “release” implica alto riesgo porque es mucho código nuevo de golpe.
- **DevOps:** los cambios se liberan en **pequeños lotes** (micro-releases). Es más fácil detectar qué línea de código causó un problema y revertirla.
Esto reduce riesgo porque los fallos son más fáciles de aislar y corregir.

**Automatización**

- **Cascada:** las pruebas, despliegues y revisiones suelen ser manuales, lentos, propensos a error y difíciles de repetir.
- **DevOps:** usa pipelines automatizados (CI/CD) que compilan, prueban y despliegan sin intervención humana.
 Esto acelera la entrega y elimina muchos errores humanos, reduciendo riesgos.

### Contexto donde un enfoque en cascada sigue siendo razonable 

**Desarrollo de software para equipos médicos** (Por ejemplo, software que controla un marcapasos).\
En este tipo de proyectos, el software debe cumplir con **certificaciones regulatorias estrictas** (FDA en EE.UU., CE en Europa). Además, suele estar fuertemente acoplado con hardware crítico.

**Criterios verificables que justifican un enfoque cercano a cascada:**

**Regulación y conformidad obligatoria**

- Verificable porque el proyecto debe presentar documentación formal y pruebas exhaustivas para obtener la certificación antes de ser usado en pacientes.
- Aquí, la trazabilidad estricta (qué requisito generó qué diseño, qué prueba lo valida) se logra más naturalmente con un ciclo en cascada.

**Seguridad y criticidad del sistema**

- Verificable porque el fallo del software puede tener consecuencias de vida o muerte.
- Se exige validación formal de cada fase antes de avanzar, el modelo en cascada minimiza ambigüedad y asegura que nada quede sin documentar.


**Trade-offs (compromisos)**

**Velocidad vs Conformidad/Seguridad**

- Con cascada, el desarrollo es **más lento**, ya que cada fase debe completarse y documentarse rigurosamente antes de avanzar. Pero, se gana en **conformidad regulatoria y seguridad**, lo que es prioritario en este contexto.

## 4.2 Ciclo tradicional de dos pasos y silos (limitaciones y anti-patrones)

### Limitaciones del ciclo "construcción -> operación" sin integración continua

**Grandes lotes de cambios**

- En lugar de integrar cambios pequeños y probados, el software avanza en grandes paquetes.
- Problema: el costo de integración aumenta, porque mezclar mucho código de golpe genera conflictos, errores ocultos y retrabajos extensos.

**Colas de defectos acumuladas**

- Los errores se descubren recién en pruebas finales o en producción.
- Problema: aparecen largas colas de bugs pendientes, lo que retrasa la entrega y genera incertidumbre sobre la calidad.

### Anti-patrones y cómo agravan incidentes

**"Throw over the wall" (tirar el trabajo "por encima del muro")**

Ocurre cuando desarrollo finaliza un producto y lo entrega como handoff a operaciones o QA sin contexto ni colaboración.

Efectos negativos:

- **Asimetrías de información:** operaciones recibe código sin entender bien cómo funciona ni qué riesgos trae.
- **Mayor MTTR (Mean Time to Recovery):** al ocurrir un incidente, los operadores tardan más en resolverlo porque dependen de volver a preguntar al equipo de desarrollo.
- Resultado: degradaciones repetitivas y retrabajos costosos.

**Seguridad como auditoría tardía**

La revisión de seguridad se hace al final del ciclo, como un trámite previo a producción.

Efectos negativos:
- Se descubren vulnerabilidades muy tarde, lo que multiplica el **costo de integración tardía** (arreglar un bug de seguridad en producción es más caro y lento).
- Los hallazgos fuerzan re-trabajos sobre código ya “cerrado”, retrasando lanzamientos.
- Al no estar integrada desde el inicio, se repiten **degradaciones** por los mismos patrones inseguros.

### Principios y beneficios de DevOps (CI/CD, automatización, colaboración; Agile como precursor)

**CI y CD**

- **Continuous Integration (CI):** los desarrolladores integran **pequeños cambios frecuentes** al repositorio principal. Cada commit dispara **pruebas automatizadas cercanas al código** (unitarias, de integración rápida), lo que permite detectar errores casi en tiempo real.
- **Continuous Delivery/Deployment (CD):** los cambios validados pasan por pruebas más amplias (funcionales, de performance, seguridad) y, si aprueban, se **promueven automáticamente** a entornos superiores (staging, producción).
- **Colaboración Dev–Ops:** CI/CD elimina silos porque los equipos de Dev, QA y Ops trabajan sobre el mismo pipeline. La automatización asegura que la calidad y la operabilidad se validen de manera conjunta, no como fases separadas.

**Cómo Agile alimenta decisiones del pipeline**

- **Reuniones diarias (daily stand-ups):** permiten que el equipo identifique bloqueos y decida si un cambio debe **avanzar por el pipeline** o esperar hasta resolver un riesgo.
- **Retrospectivas:** ayudan a revisar métricas y experiencias de despliegues anteriores, decidiendo mejoras en el pipeline (por ejemplo, agregar una prueba de carga o ajustar reglas de rollback automático).


En ambos casos, el **_feedback_ humano** de Agile guía la **automatización técnica** del pipeline: qué se promueve, qué se bloquea y cómo se mejora.

**Indicador observable de colaboración Dev–Ops**

**Tiempo desde que un Pull Request está listo hasta que se despliega en entorno de pruebas.**\
Mide qué tan coordinados están Dev y Ops. Si el tiempo baja, significa menos colas y más colaboración.

Cómo recolectarlo sin herramientas de paga:

1.	Metadatos de PRs (GitHub/GitLab):
    - Fecha/hora en que el PR fue marcado como “ready to merge”.
    - Fecha/hora en que el merge generó un build en CI.

2. Registros de despliegue (logs de Jenkins, GitHub Actions, GitLab CI, etc.):
    - Timestamp del despliegue al entorno de pruebas.

3. Comparación de timestamps:
    - Restar ambos tiempos para cada PR.
    - Guardar los resultados en una hoja de cálculo o bitácora.

No hace falta pagar herramientas, basta con usar logs del repositorio y del pipeline, que ya guardan los eventos con fecha y hora.















































