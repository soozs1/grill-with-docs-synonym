# Formato de ADR

Los ADR residen en `docs/adr/` y usan una numeración correlativa: `0001-slug.md`, `0002-slug.md`, etc. Crea el directorio `docs/adr/` de forma perezosa — solo cuando haga falta el primer ADR.

## Plantilla

```md
# {Título breve de la decisión}

{1–3 frases: cuál es el contexto, qué decidieron y por qué.}
```

Y eso es todo. Un ADR puede ser un solo párrafo. El valor está en registrar *que* se tomó la decisión y *por qué* — no en rellenar secciones.

## Secciones opcionales

Inclúyelas solo cuando realmente aporten valor. La mayoría de los ADR no las necesitan.

- **Estado** en el frontmatter (`proposed | accepted | deprecated | superseded by ADR-NNNN`) — útil cuando se vuelve sobre las decisiones
- **Alternativas consideradas** — solo cuando vale la pena recordar las alternativas descartadas
- **Consecuencias** — solo cuando hay que resaltar efectos downstream no obvios

## Numeración

Escanea `docs/adr/` para encontrar el número máximo existente y auméntalo en uno.

## Cuándo proponer un ADR

Deben cumplirse las tres:

1. **Difícil de revertir** — el coste de cambiar de opinión más tarde es notable
2. **No es obvio sin contexto** — un lector futuro mirará el código y preguntará «¿por qué lo hicieron así?»
3. **Resultado de un compromiso real** — había alternativas reales, y eligieron una por razones concretas

Si la decisión es fácil de revertir — omítela, simplemente la revertirás. Si no sorprende — nadie se preguntará «por qué». Si no hubo una alternativa real — no hay nada que registrar, salvo «hicimos lo obvio».

### Qué encaja

- **Forma arquitectónica.** «Usamos un monorepo.» «El modelo de escritura es event-sourced; el modelo de lectura se proyecta en Postgres.»
- **Patrones de integración entre contextos.** «Ordering y Billing se comunican mediante eventos de dominio, no por HTTP síncrono.»
- **Elecciones tecnológicas con lock-in.** La base de datos, el bus de mensajes, el proveedor de autenticación, el target de despliegue. No todas las bibliotecas — solo aquellas que llevaría un trimestre entero reemplazar.
- **Decisiones sobre fronteras y zonas de responsabilidad.** «Los datos de Customer pertenecen al contexto Customer; los demás contextos los referencian solo por ID.» Los «no» explícitos son tan valiosos como los «sí».
- **Desviaciones conscientes del camino obvio.** «Usamos SQL puro en lugar de un ORM, porque X.» Todo aquello donde un lector razonable asumiría lo contrario. Esto evitará que el siguiente ingeniero «corrija» lo que se hizo a propósito.
- **Restricciones no visibles en el código.** «No podemos usar AWS por requisitos de compliance.» «El tiempo de respuesta debe ser menor de 200 ms por un contrato con una API de un socio.»
- **Alternativas descartadas, cuando el rechazo no es obvio.** Si consideraron GraphQL y eligieron REST por razones sutiles — regístrenlo; si no, dentro de medio año alguien volverá a proponer GraphQL.
