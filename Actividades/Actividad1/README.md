# Actividad 1: Introducción devops, devsecops

**Nombre:** Anthony Aldair Carlos Ramon\
**Fecha:** 05/09/2025

## 4.1 DevOps vs Cascada

### ¿Por qué DevOps acelera y reduce riesgo en software para la nube frente a cascada (feedback continuo, pequeños lotes, automatización)?

**Feedback continuo**

- **Cascada:** el cliente o usuario solo da retroalimentación(_feedback_) al final del proyecto. Si hay errores o malentendidos, se descubren muy tarde, alto riesgo y costo de corrección.
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
- Se exige validación formal de cada fase antes de avanzar → el modelo en cascada minimiza ambigüedad y asegura que nada quede sin documentar.


**Trade-offs (compromisos)**

**Velocidad vs Conformidad/Seguridad**

- Con cascada, el desarrollo es **más lento**, ya que cada fase debe completarse y documentarse rigurosamente antes de avanzar. Pero, se gana en **conformidad regulatoria y seguridad**, lo que es prioritario en este contexto.






















