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

## 4.3 Principios y beneficios de DevOps (CI/CD, automatización, colaboración; Agile como precursor)

### CI y CD

- **Continuous Integration (CI):** los desarrolladores integran **pequeños cambios frecuentes** al repositorio principal. Cada commit dispara **pruebas automatizadas cercanas al código** (unitarias, de integración rápida), lo que permite detectar errores casi en tiempo real.
- **Continuous Delivery/Deployment (CD):** los cambios validados pasan por pruebas más amplias (funcionales, de performance, seguridad) y, si aprueban, se **promueven automáticamente** a entornos superiores (staging, producción).
- **Colaboración Dev–Ops:** CI/CD elimina silos porque los equipos de Dev, QA y Ops trabajan sobre el mismo pipeline. La automatización asegura que la calidad y la operabilidad se validen de manera conjunta, no como fases separadas.

### Cómo Agile alimenta decisiones del pipeline

- **Reuniones diarias (daily stand-ups):** permiten que el equipo identifique bloqueos y decida si un cambio debe **avanzar por el pipeline** o esperar hasta resolver un riesgo.
- **Retrospectivas:** ayudan a revisar métricas y experiencias de despliegues anteriores, decidiendo mejoras en el pipeline (por ejemplo, agregar una prueba de carga o ajustar reglas de rollback automático).


En ambos casos, el **_feedback_ humano** de Agile guía la **automatización técnica** del pipeline: qué se promueve, qué se bloquea y cómo se mejora.

### Indicador observable de colaboración Dev–Ops

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

## 4.4 Evolución a DevSecOps (seguridad desde el inicio: SAST/DAST; cambio cultural)

### Diferencia entre SAST y DAST

**SAST (Static Application Security Testing):**

- Analiza el código fuente, bytecode o binarios sin ejecutar la aplicación.
- Se usa en etapas tempranas del pipeline (fase de build o commit).
- Detecta fallos como inyecciones SQL en el código, uso inseguro de librerías, etc.
- Ventaja: barato de correr temprano, evita que los defectos pasen a fases posteriores.

**DAST (Dynamic Application Security Testing):**

- Analiza la aplicación en ejecución, como si fuera un atacante externo.
- Se ubica en etapas de pruebas o staging del pipeline, antes de desplegar a producción.
- Detecta fallos en ejecución: XSS, validaciones insuficientes, configuraciones incorrectas.
- Ventaja: prueba la aplicación real, no solo el código.

### Gate mínimo de seguridad

Un _gate_ es un punto de control dentro del _pipeline_ donde se ejecutan validaciones automáticas (como análisis de vulnerabilidades, escaneo de dependencia, etc.) para asegurar que el software cumple con los requisitos de seguridad antes de avanzar a la siguiente fase(staging, producción) o ser liberado. Es una política de “pasa/no pasa” en el _pipeline_. Sirve para detener software inseguro antes de que llegue a los usuarios.\ 
Ejemplo con dos **umbrales cuantitativos**:

- **Bloqueo duro:** Cualquier hallazgo de severidad crítica en componentes expuestos (servicios públicos o APIs) bloquea la promoción a staging/producción.
- **Umbral de cobertura:** La cobertura mínima de pruebas de seguridad automatizadas (SAST + DAST) debe ser al menos 80% del código y endpoints críticos.

### Política de excepción

Cuando el equipo **no puede resolver inmediatamente un hallazgo** (por dependencia externa o limitaciones técnicas), se documenta:

- **Caducidad:** máximo 30 días antes de volver a revisarlo.
- **Responsable:** líder técnico del servicio afectado.
- **Plan de corrección:** issue en el backlog con pasos definidos (ejemplo: actualizar dependencia vulnerable a versión 3.2.1).

Esto asegura que la excepción no se “olvide” indefinidamente.

### ¿Cómo evitar el "teatro de seguridad" (cumplir checklist sin reducir riesgo)? 

Señales de eficacia reales

**Disminución de hallazgos repetidos:**

- Si el mismo tipo de vulnerabilidad deja de reaparecer en cada release, significa que el equipo está **aprendiendo y corrigiendo de raíz**.
- Medición: comparar reportes de SAST/DAST entre releases, con el fin de contar vulnerabilidades de la misma categoría (ej. XSS, SQLi).

**Reducción en tiempo de remediación (MTTR de vulnerabilidades):**

- No basta con encontrarlas, hay que cerrarlas más rápido.
- Medición: registrar fecha de detección vs fecha de cierre en issues, con el fin de calcular el promedio por severidad (críticas, altas, medias).











































