---
name: agent-hierarchy
description: Diseña jerarquías de orquestador y subagentes para un repositorio —dividiendo agentes por superficie de escritura exclusiva, emparejando cada productor con un auditor independiente y haciendo cumplir la división con un script que se ejecuta en CI. Usar esta skill cuando el usuario quiera configurar, expandir, auditar o reparar la estructura multi-agente de un código; pregunte cómo dividir el trabajo entre agentes; necesite redactar charters, roles o un mapa de superficies; o esté encontrando agentes que colisionan en los mismos archivos, revisan su propio trabajo o se desvían de su ámbito. También usar al dimensionar un equipo de agentes o decidir si se justifica un nuevo agente.
---

# Jerarquía de agentes

Un método para montar una jerarquía orquestador → subagentes especialistas, extraído de una
implementación de ~24 agentes sobre un monorepositorio de 1.500 archivos, verificado en cada PR.

## Todo el método en un párrafo

Dividí agentes por **superficie de escritura, no por tema**. Dos clases únicamente: **constructores**,
que editan dentro de exactamente una superficie exclusiva y nunca commitean, y **revisores**, que son
permanentemente de solo lectura y siempre pueden ejecutarse en paralelo. El orquestador —el chat
principal— es el único que commitea. Escribí el mapa de superficies **antes** que cualquier charter,
mantenelo en un solo archivo Markdown y hacelo cumplir con un script que se ejecute en CI. Cada fila
lleva también una **autoridad** —`autonomous`, `proposes` o `escalates`— que responde la pregunta
separada de si el trabajo de ese agente puede ingresar sin una decisión explícita. Productor y auditor
nunca son el mismo agente. Para cada clase de dato, exactamente un archivo es el dueño y todos los
demás derivan.

## Por qué fallan las divisiones por tema

"Un agente para SEO, uno para UI" es la división intuitiva y se rompe inmediatamente: ambos terminan
editando `tokens.css`. Ninguno está equivocado, y ninguno puede hacerse responsable. Una división
por superficie no tiene ese solapamiento por construcción —que es exactamente lo que la hace
verificable.

## Orden de operaciones

No empieces a escribir charters temprano; el orden es el método.

1. **Inventariá el árbol real** — `git ls-files | sed 's|/[^/]*$||' | sort -u`. Reportá lo que está
   realmente ahí antes de proponer nada.
2. **Proponé el equipo** — el conjunto más pequeño donde dos agentes no compartan un archivo. Cada uno
   necesita un id, una clase, un ámbito de una línea y sus patrones de archivos exactos. Un agente cuya
   superficie no se pueda expresar en patrones no es un agente; integralo en otro.
3. **Escribí el mapa de superficies** — un archivo Markdown, una fila por agente.
4. **Conectá la guardia** — `scripts/agent-guard.mjs check` prueba que el mapa sea coherente (ninguna
   ruta reclamada dos veces, ninguna ruta sin dueño); `agent-guard.mjs diff <agent>` prueba que un
   cambio dado lo obedeció. Ambos son necesarios: una vez que el orquestador commitea, la autoría que
   `diff` verifica se pierde, así que debe ejecutarse mientras el trabajo todavía sea atribuible.
5. **Escribí charters al final**, en el formato del manual: por qué existe el agente, qué no debe hacer
   nunca, la verificación que su superficie implica y un contrato de retorno de seis secciones.

## Reglas que llevan una falla detrás

- **Productor y auditor nunca son el mismo agente.** Un agente que revisa su propio trabajo aprueba
  confiablemente su resultado.
- **El orquestador no es una de las dos clases.** Es el único que commitea, y darle una superficie lo
  convierte en un constructor que también puede fusionar.
- **Un archivo es dueño de cada clase de dato.** Todo lo demás deriva de él, o las dos copias divergen
  y nadie nota cuál está desactualizada.
- **Nunca elimines una exportación compartida porque parezca no usada.** No podés ver los consumidores
  desde dentro del núcleo. Marcá como obsoleta, anunciá y después eliminá.

## El registro propio de este repositorio

`docs/DECISION-LOG.md` es la instancia viva del registro de decisiones descrito en el manual. Cuando se
plantea una decisión, asignale el siguiente número inmediatamente —antes de responderla— y dale opciones
con letras y una recomendación explícita. Nunca renumeres, nunca reuses un número y registrá las
resoluciones en el lugar en vez de eliminarlas.

## Fuentes

`references/sources.md` en esta skill lista las autoridades externas que resuelven las preguntas
de acá —qué es autoritativo cada una y qué podés hacer con ella. Revisalas antes de responder sobre
lo que cubren, y citá lo que usaste. Casi todas se leen gratis y no se reproducen gratis; la nota de
uso de cada una manda.

## Nunca

- No agregues un agente antes de que la superficie que poseerá exista. El mapa va primero, el agente segundo.
- No dejes que dos agentes sostengan un reclamo de escritura sobre la misma ruta. El solapamiento se resuelve como una carrera.
- No edites a mano un artefacto generado en lugar de la fuente de la que deriva.
- No saltees la guardia porque el cambio sea pequeño. Los cambios pequeños son como las superficies se desvían.
