---
name: domain-modeling
description: Construye y afila el modelo de dominio del proyecto. Úsalo cuando el usuario quiera fijar la terminología del dominio o un lenguaje común (ubiquitous language), registrar una decisión arquitectónica (ADR), o cuando otro skill necesite sostener el modelo de dominio.
---

# Modelado de dominio

Construye y afila activamente el modelo de dominio del proyecto a medida que diseñas. Esta es una disciplina *activa*: cuestionar los términos, idear casos límite y registrar el glosario y las decisiones en el momento exacto en que cristalizan. (Limitarse a *leer* `CONTEXT.md` por el vocabulario — eso no es este skill; es un hábito de una línea, disponible para cualquier skill. Este skill — es para los casos en que cambias el modelo, no para cuando solo lo usas.)

## Estructura de archivos

La mayoría de los repositorios tienen un solo contexto:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

Si en la raíz hay un `CONTEXT-MAP.md`, el repositorio tiene varios contextos. El mapa indica dónde reside cada uno de ellos:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                       ← decisiones de todo el sistema
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/              ← decisiones de un contexto concreto
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Crea los archivos de forma perezosa — solo cuando haya algo que registrar. Si aún no existe `CONTEXT.md`, créalo cuando se fije el primer término. Si no hay `docs/adr/`, créala cuando haga falta el primer ADR.

## Lenguaje

Conduce la sesión en el idioma de trabajo del usuario (por defecto — español).

En el glosario, separa el *término canónico* de su *definición*.

- **El canon refleja el código.** Registra el término canónico tal como vive en el proyecto. En un proyecto nuevo (greenfield) — pregunta una vez al equipo, ofreciendo de entrada las variantes bilingües («¿lo llamamos *Order / Pedido*?»), y fija la elección.
- **Sinónimos totales — co-canónicos, con `/`.** Si los términos son idénticos en significado e intercambiables en cualquier escenario (review / revisión, parsing / análisis), escríbelos en el canon con `/` y espacios: `Order / Pedido`. Pueden ser más de dos.
- **Sinónimos parciales — en `_Avoid_`.** Si los términos se parecen, pero en algún escenario divergen en significado (cuenta / usuario / cliente), elige un canon y lleva el resto a `_Avoid_`.
- **El orden = señal suave para el código.** Para los conceptos ligados al código (que se mapean a una clase, función, tabla, módulo o archivo), el primer término del canon es el inglés/latino: `Order / Pedido`. No hay un campo explícito de «forma para el código», y esto no es una restricción: el agente se orienta por la forma inglesa para los identificadores, pero la forma de nombrar es libre. Los conceptos puramente coloquiales pueden ir con el español primero.
- **Definición** — en el idioma de trabajo, una o dos frases, «qué es». `_Avoid_` — lista separada por comas (el orden no importa), incluye tanto las formas rechazadas inglesas como las españolas.

El objetivo — que los términos del glosario coincidan con cómo se llaman en el código y en el habla del equipo. Los términos aprobados los reutiliza el agente; la capa de persistencia es `CONTEXT.md`. Si el equipo prefiere otra convención (un glosario completamente en español, transliteración en los identificadores) — ok, sé coherente y fija la elección en un ADR.

## Durante la sesión

### Coteja con el glosario

Cuando el usuario use un término en contra del lenguaje existente en `CONTEXT.md`, señálalo de inmediato: «En el glosario "cancelación" está definida como X, pero parece que te refieres a Y — ¿cuál de las dos es?»

### Afila las formulaciones difusas

Cuando el usuario use términos vagos o sobrecargados, propone el término canónico preciso: «Dices "cuenta" — ¿te refieres a Customer o a User? Son cosas distintas.»

### Establece los sinónimos totales

Cuando un término tenga candidatos a sinónimo, proponlos al usuario: aprobar, rechazar o posponer («¿lo llamamos *grilling / grillear*, o lo discutimos luego?»). «Posponer» — es un escape-hatch en el diálogo; en el registro del glosario solo entran los sinónimos aprobados.

La heurística — **la prueba de sustitución**: los términos son sinónimos totales solo si se pueden intercambiar en cualquier frase y escenario sin cambio de sentido. Si puedes construir un escenario donde divergen — **estás obligado a presentarlo explícitamente una vez** («este es un escenario donde X e Y son cosas distintas; ¿seguro que los fusionamos?»). Esto es una advertencia de riesgo, no un veto: la decisión es del usuario. Si aprueba, visto el contraejemplo — escribimos el sinónimo total con `/`. Si rechaza — un canon, el resto en `_Avoid_`. La advertencia es única, sin machacar. Los términos aprobados, reutilízalos después y fíjalos en `CONTEXT.md` en el momento.

### Discute escenarios concretos

Cuando se discutan las relaciones entre conceptos del dominio, compruébalas con escenarios concretos. Inventa escenarios que exploren los casos límite y obliguen al usuario a delimitar con claridad las fronteras entre conceptos.

### Coteja con el código

Cuando el usuario afirme cómo funciona algo, verifica si concuerda con el código. Si encuentras una contradicción — sácala a la luz: «El código cancela los pedidos por completo, pero acabas de decir que es posible una cancelación parcial — ¿cuál de las dos es correcto?»

### Actualiza CONTEXT.md en el momento

Cuando un término quede fijado, actualiza `CONTEXT.md` de inmediato. No los acumules por lotes — fíjalos a medida que aparecen. Usa el formato de [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md). `CONTEXT.md` debe estar completamente libre de detalles de implementación. No conviertas `CONTEXT.md` ni en una especificación, ni en un borrador, ni en un almacén de decisiones de implementación. Es un glosario y nada más.

### Propón ADR con moderación

Propón crear un ADR solo cuando se cumplan las tres:

1. **Difícil de revertir** — el coste de cambiar de opinión más tarde es notable
2. **No es obvio sin contexto** — un lector futuro preguntará «¿por qué lo hicieron así?»
3. **Resultado de un compromiso real** — había alternativas reales, y eligieron una por razones concretas

Si falta al menos una de las tres — omite el ADR. Usa el formato de [ADR-FORMAT.md](./ADR-FORMAT.md).
