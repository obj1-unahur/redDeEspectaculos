# Final Red de Espectáculos

Una empresa busca lanzar una red de suscripciones a espectáculos en vivo.  
Los clientes podrán acceder a distintos tipos de espectáculos (conciertos, obras de teatro, stand-ups, etc.) a través de planes de membresía mensual.  
El objetivo es que el sistema pueda calcular el costo mensual de cada cliente y permitir flexibilidad en la gestión de tipos de espectáculos y planes, aprovechando las ventajas de la programación orientada a objetos.

---

## Requerimientos

### Espectáculos
La red ofrece inicialmente:
- **Conciertos**
- **Obras de teatro**
- **Stand-ups**

Pero podrían sumarse nuevos tipos de espectáculos a futuro.  
De cada espectáculo se conoce:
- Su **nombre**.
- Las **fechas** del espectáculo (representadas como enteros, ej: `20251003` para 3/10/2025).  
  - Las fechas no intervienen en el cálculo de costos, pero deben almacenarse para registrar la asistencia.
- Un **costo base de entrada** (mismo para todas las fechas, puede variar).

---

#### Conciertos
- Son espectáculos individuales.
- **Costo = costo base.**

#### Obras de Teatro
- **Costo = costo base + canon cultural.**
- El **canon cultural**:
  - Es un valor común para todas las obras de teatro (inicialmente `$8`).
  - Lo establece el **CLAP** (Colectivo Libre de Actores y Performers).
  - Puede cambiar en cualquier momento.

#### Stand-Ups
- Se componen de múltiples comediantes.
- De cada comediante se conoce:
  - **Nombre**
  - **Cachet** (costo de participación).
- **Costo = costo base + suma de los cachets de todos sus comediantes.**

---

### Planes de Membresía
Existen diferentes planes que determinan acceso y costo mensual.

#### Plan Clásico
- Permite disfrutar hasta **N espectáculos por mes**.
- **Abono mensual = N * canon cultural**.
- Restricciones:
  - El valor de **N** no puede cambiarse luego de creado.
  - Los primeros **N espectáculos del mes** se consideran cubiertos.
  - Espectáculos adicionales: se cobra **costo base**.

**Ejemplo:**
- Cliente con Plan Clásico de **2 espectáculos** (canon = 8).  
  Valor del plan = `$16`.
- Asiste a 4 espectáculos:
  1) concierto1 ($15) el 5/10
  2) teatro1 ($20) el 7/10
  3) standup1 ($10) el 8/10
  4) concierto2 ($25) el 15/10

**Cálculo:**
- Plan: `$16` (incluye los 2 primeros).
- Excedentes: standup1 ($10) + concierto2 ($25).
- **Total mensual = $51.**

> 💡 Tip: puede ser útil usar los mensajes `take(n)` o `drop(n)`.

---

#### Plan Premium
- Acceso **ilimitado** a todos los espectáculos.
- Costo mensual fijo.
- El costo puede **modificarse aplicando un incremento porcentual**.

---

#### Plan Amigos
- Similar al Plan Clásico.
- Se aplica un **descuento del 15%** sobre el total mensual.

---

### Clientes
- Cada cliente tiene un único plan, pero puede cambiarlo en cualquier momento.
- Cada cliente registra:
  - **Espectáculos asistidos** en el mes.
  - **Fecha de asistencia**.
- Validación:
  - Si la fecha no existe → lanzar excepción **“Fecha de espectáculo no disponible”**.

---

### Cálculo de Costo Mensual
Se debe calcular el importe total de un cliente según:
1. Su plan.
2. Espectáculos asistidos.

Al cierre del mes, la red debe:
- Calcular la **facturación total**.
- Indicar a los clientes que **limpien su historial de espectáculos** (para iniciar nuevo período).

---

### Migración de Plan
La red debe migrar clientes con Plan Clásico a un **Plan Premium** si:
- El **costo Premium < costo Clásico real** (según espectáculos asistidos ese mes).

Si se cumple → migración automática.  
Si no → el plan no cambia.

---

### Test
Plantear algunos casos significativos:
- Cliente con Plan Clásico que **migra a Plan Premium**.
- Validación de fechas de espectáculos.
- Cálculo de excedentes en Plan Clásico.
- Aplicación del 15% de descuento en Plan Amigos.
