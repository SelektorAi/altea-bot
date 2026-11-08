---
name: financial-modeling
description: Construye y evalúa modelos financieros para proyecciones, planificación de escenarios y soporte de decisiones en Altea —lógica de ingresos por preventa, estructura de costos de obra, variables impulsoras y análisis de sensibilidad que muestra dónde se quiebra un proyecto. Usar esta skill para modelar el impacto financiero de un lanzamiento, construir el plan de largo plazo, evaluar la compra de tierra o poner a prueba el modelo financiero de un tercero.
---

# Modelación financiera

Un modelo es un argumento sobre cómo funciona el negocio inmobiliario de Altea, expresado en
aritmética. Su valor es la solidez del argumento, no la precisión aparente de los decimales.

## Estructura

Tres capas separadas, siempre:

1. **Insumos (Supuestos)** —cada supuesto en un solo lugar, con su fuente y fecha de verificación.
   Un supuesto enterrado dentro de una fórmula es invisible y por lo tanto nunca cuestionado.
2. **Cálculos** —sin números fijos ingresados directamente en las fórmulas (*no hardcoding*). Nunca. Una
   constante dentro de una fórmula es un supuesto ilocalizable.
3. **Resultados** —los estados financieros y el resumen de indicadores que lee la Gerencia o el
   Directorio (VAN, TIR, margen bruto, punto de equilibrio, cap rate proyectado).

Una fila, un cálculo, llevado de manera consistente en todos los períodos. Los modelos se vuelven
inauditables por filas inconsistentes más que por complejidad.

## Construí los ingresos desde las variables impulsoras (*drivers*)

En Altea, los ingresos de un proyecto no se modelan como un monto global:
- **Metraje vendible** y distribución por tipología (monoambiente, 1 y 2 dormitorios).
- **Precio promedio por m²** por tipología y por fase de preventa.
- **Ritmo de absorción mensual** (unidades vendidas/reservadas por mes).
- **Plan de pagos del comprador**: 20% inicial, 70% en 40 cuotas durante obra, 10% contra entrega
  (ejemplo Altea Tower).
- **Régimen tributario paraguayo**: IVA e IRE al 10% (DNIT, Ley 6380/2019).

## Regla de modelación obligatoria en Altea

Todo modelo financiero entregado en Altea debe incluir en su primera sección:
- **Supuestos explícitos** (ocupación, tarifa diaria/mensual, plazo de obra, costo por m², tipo de cambio).
- **Fuente de cada supuesto** (con su fecha de verificación).
- **Análisis de sensibilidad o rango** (escenario base / optimista / adverso).
- **Exclusiones explícitas** (vacancia, impuestos no contemplados, costo de reventa).
- **Condiciones de validez** (qué tendría que ser cierto para que el resultado se cumpla).

## Fuentes

`references/sources.md` en esta skill lista las autoridades externas que resuelven las preguntas
de acá —qué es autoritativo cada una y qué podés hacer con ella. Revisalas antes de responder sobre
lo que cubren, y citá lo que usaste. Casi todas se leen gratis y no se reproducen gratis; la nota
de uso de cada una manda.

## Nunca

- No incluyas números fijos escritos dentro de las fórmulas de cálculo del modelo.
- No entregues un modelo financiero sin su correspondiente análisis de sensibilidad o rangos.
- No formules ingresos de preventa sin vincularlos al plan de pagos y al calendario de obra.
- No ocultes los supuestos de ocupación o tarifa al calcular la rentabilidad proyectada de un proyecto.
