# Formato de CONTEXT.md

## Estructura

```md
# {Nombre del contexto}

{Una o dos frases: qué es este contexto y por qué existe.}

## Lenguaje

**Order / Pedido**: {Una o dos frases: qué es}
_Avoid_: Purchase, transaction, compra

**Invoice / Factura**: Solicitud de pago enviada al cliente tras la entrega.
_Avoid_: Bill, payment request

**Customer / Cliente**: Persona física o jurídica que realiza pedidos.
_Avoid_: Client, buyer, account, comprador
```

## Reglas

- **Distingue sinónimos totales y parciales.** Los sinónimos totales (intercambiables en cualquier escenario: review / revisión) escríbelos como co-canónicos con `/` y espacios. Los parciales (divergen en algún escenario: cliente / comprador) — uno al canon, el resto en `_Avoid_`.
- **Para los conceptos ligados al código — el inglés primero.** Si el término se mapea a código (clase, función, tabla, archivo), la primera variante del canon es la inglesa/latina: `Order / Pedido`. Esto es una señal, no una restricción; no hay un campo explícito de «forma para el código».
- **Sé categórico.** Fija el canon — un término o un conjunto de sinónimos totales con `/` — y todo lo sobrante llévalo a `_Avoid_`.
- **Mantén las definiciones cortas.** Máximo una o dos frases. Define qué ES, no qué hace. La definición — en el idioma de trabajo del equipo.
- **Incluye solo términos específicos del contexto de este proyecto.** Los conceptos generales de programación (timeouts, tipos de error, patrones utilitarios) no pertenecen aquí, aunque el proyecto los use intensamente. Antes de añadir un término, pregúntate: ¿este concepto es único de este contexto o es un concepto general de programación? Al glosario solo pertenece lo primero.
- **Agrupa los términos bajo subtítulos**, cuando surjan clústeres naturales. Si todos los términos pertenecen a una misma área coherente, una lista plana es aceptable.

## Repositorios de uno y varios contextos

**Un contexto (la mayoría de los repositorios):** un `CONTEXT.md` en la raíz del repositorio.

**Varios contextos:** `CONTEXT-MAP.md` en la raíz enumera los contextos, dónde residen y cómo se relacionan entre sí:

```md
# Mapa de contextos

## Contextos

- [Ordering](./src/ordering/CONTEXT.md) — recibe y rastrea los pedidos de los clientes
- [Billing](./src/billing/CONTEXT.md) — emite facturas y procesa los pagos
- [Fulfillment](./src/fulfillment/CONTEXT.md) — gestiona la preparación y el envío desde el almacén

## Relaciones

- **Ordering → Fulfillment**: Ordering publica eventos `OrderPlaced`; Fulfillment los consume para empezar la preparación
- **Fulfillment → Billing**: Fulfillment publica eventos `ShipmentDispatched`; Billing los consume para emitir la factura
- **Ordering ↔ Billing**: Tipos compartidos para `CustomerId` y `Money`
```

El skill determina por sí mismo qué estructura aplica:

- Si hay `CONTEXT-MAP.md` — lo lee para encontrar los contextos
- Si solo hay un `CONTEXT.md` en la raíz — un contexto
- Si no hay ninguno de los dos — crea el `CONTEXT.md` de la raíz de forma perezosa, cuando se fije el primer término

Cuando haya varios contextos, determina a cuál pertenece el tema actual. Si no está claro — pregunta.
