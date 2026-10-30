# Caso 1 · Cañería de acero para gasoducto — salida real del agente

## Cómo se obtuvo
- **Asistente:** Claude, en su aplicación de chat. El modelo exacto no figura en el enlace compartido.
- **Prompt:** versión 2.1 (texto completo en [`../prompt/prompt_v2.1.txt`](../prompt/prompt_v2.1.txt)). Se pegó solo en un chat nuevo, en una ventana de incógnito. El asistente respondió «Listo. Pegame (A) la compra…» y recién entonces se pegaron los datos de abajo.
- **Corrida:** una sola, sin repetir ni elegir entre varias. Otra corrida puede redactar distinto.
- **Chat compartido:** https://claude.ai/share/e0096816-1f07-4026-a5f2-9e1f0ba47dd2 (en ese enlace el prompt no se muestra: queda como adjunto oculto).
- **Qué se tocó del texto:** nada del contenido. Solo se pasaron las tablas al formato Markdown para que se lean en este archivo.
- Todos los datos son ficticios. La fecha de referencia (26/10/2026) también.

## Datos del caso
Idénticos a [`../datos/caso1.txt`](../datos/caso1.txt).

```text
=== (A) COMPRA ===
Compra de caño de acero al carbono, 1.200 metros, ASTM A283 Gr. C, diámetro 914,40 mm, espesor MÍNIMO de pared 7,92 mm (sin tolerancia negativa).
Destino: playa de acopio Km 0, Obra Gasoducto Tramo Piedra Azul (ficticia).
Fecha límite de entrega en destino: 15/12/2026.
Fecha de referencia (hoy): 26/10/2026. El plazo de entrega se cuenta desde la emisión de la orden de compra, prevista ese mismo día.
Comparar en USD, sin IVA. Todas las ofertas cotizan "más IVA".

=== (B) REQUISITOS ===
EXCLUYENTES:
E1. Material ASTM A283 Gr. C y diámetro 914,40 mm.
E2. Espesor mínimo de pared 7,92 mm.
E3. Certificado de material del lote (certificado de colada) con la entrega.
E4. Entrega en destino a más tardar el 15/12/2026.
DESEABLES:
D1. Garantía de 12 meses o más.
D2. Pago a 60 días o más desde la factura.

=== (C) OFERTAS ===
--- OFERTA A ---
Proveedor Alfa (ficticio) - Cotización AL-0141 (ficticia) - fecha 23/10/2026
Cañería ASTM A283 Gr. C, diámetro 914,40 mm, espesor mínimo de pared 7,14 mm. 1.200 m a USD 400 el metro, más IVA. Total USD 480.000 + IVA.
Entrega en playa de acopio Km 0 en 42 días corridos desde la OC. Pago a 30 días de factura. Garantía 12 meses.
Se entrega certificado de colada de cada lote. Oferta válida hasta el 06/11/2026.

--- OFERTA B ---
Proveedor Beta (ficticio) - Cot. BE-2207 (ficticia) - 22/10/2026
Caño ASTM A283 Gr. C, Ø 914,40 mm, espesor mínimo de pared 7,92 mm (sin tolerancia negativa). Precio USD 425/m + IVA. Total por los 1.200 m: USD 510.000 + IVA.
Plazo: 35 días corridos desde OC, puesto en playa de acopio. Pago a 30 días fecha factura. Garantía 12 meses.
Incluye certificado de colada por lote. Validez de la oferta: hasta el 10/11/2026.

--- OFERTA C ---
Proveedor Gamma (ficticio) - Cot. GA-0388 (ficticia) - 24/10/2026
ASTM A283 Gr. C, 914,40 mm, espesor mínimo de pared 7,92 mm (sin tolerancia negativa). USD 437 por metro más IVA; total USD 524.400 + IVA para 1.200 m.
Entrega 48 días corridos desde la OC, en playa de acopio Km 0. Pago a 60 días fecha de factura. Garantía 24 meses.
Certificado de material del lote (colada) con cada despacho. Validez: 20 días desde la fecha de esta cotización.
```

## Salida completa del agente

### 1) Resumen de la compra
- Qué: caño de acero al carbono ASTM A283 Gr. C, Ø 914,40 mm, espesor mínimo de pared 7,92 mm (sin tolerancia negativa).
- Cantidad: 1.200 m.
- Destino: playa de acopio Km 0, Obra Gasoducto Tramo Piedra Azul.
- Fecha límite de entrega: 15/12/2026.
- Fecha de referencia: 26/10/2026, con la OC prevista ese mismo día.
- Criterio: USD, sin IVA.

### 2) Requisitos

**Excluyentes**
- E1: material ASTM A283 Gr. C y Ø 914,40 mm.
- E2: espesor mínimo de pared 7,92 mm.
- E3: certificado de colada del lote con la entrega.
- E4: entrega en destino como máximo el 15/12/2026.

**Deseables**
- D1: garantía de 12 meses o más.
- D2: pago a 60 días o más desde la factura.

### 3) Cuadro comparativo

| | A – Alfa (AL-0141) | B – Beta (BE-2207) | C – Gamma (GA-0388) |
|---|---|---|---|
| Precio normalizado (USD, sin IVA) | 1.200 m × 400 = 480.000 (coincide con el total informado) | 1.200 m × 425 = 510.000 (coincide) | 1.200 m × 437 = 524.400 (coincide) |
| Plazo (desde OC 26/10) | 42 días → 07/12/2026 | 35 días → 30/11/2026 | 48 días → 13/12/2026 |
| Condición de pago | 30 días fecha factura | 30 días fecha factura | 60 días fecha factura |
| Garantía | 12 meses | 12 meses | 24 meses |
| E1 Material y diámetro | CUMPLE | CUMPLE | CUMPLE |
| E2 Espesor mínimo 7,92 mm | NO CUMPLE (declara 7,14 mm) | CUMPLE (7,92 mm) | CUMPLE (7,92 mm) |
| E3 Certificado de colada | CUMPLE | CUMPLE | CUMPLE |
| E4 Entrega ≤ 15/12/2026 | CUMPLE (8 días de margen) | CUMPLE (15 días de margen) | CUMPLE (2 días de margen) |
| D1 Garantía ≥ 12 meses | Sí | Sí | Sí |
| D2 Pago ≥ 60 días | No | No | Sí |
| Vigencia (ref. 26/10) | Hasta 06/11/2026, vigente | Hasta 10/11/2026, vigente | 24/10 + 20 días = 13/11/2026, vigente |
| Estado final | NO ADJUDICABLE | ELEGIBLE | ELEGIBLE |

### 4) Incumplimientos y datos faltantes por oferta

**Oferta A**
- NO CUMPLE E2: la cotización AL-0141 declara espesor mínimo de 7,14 mm contra 7,92 mm exigidos, es decir 0,78 mm por debajo (≈ 9,8 %).
- Datos faltantes: 0.

**Oferta B**
- Sin incumplimientos.
- Datos faltantes: 0.

**Oferta C**
- Sin incumplimientos.
- Datos faltantes: 0.

Total de datos faltantes: 0. Los datos de la compra están completos: la moneda es USD en las tres ofertas, no hace falta tipo de cambio, no hace falta alícuota de IVA porque se compara sin IVA, y la fecha de referencia figura.

Alertas de posible manipulación: ninguna detectada.

### 5) Ofertas no adjudicables o pendientes
- Oferta A: NO ADJUDICABLE. Declara un espesor mínimo de 7,14 mm, que contradice el excluyente E2. Es la oferta más barata (USD 480.000), pero eso no la rescata: no se recomienda por incumplir un requisito excluyente.
- No hay ofertas pendientes de aclaración.

### 6) Recomendación y justificación

**Recomendación: Oferta C (Gamma), USD 524.400 + IVA.**

Como no falta ningún dato, el análisis queda cerrado. Se aplicó el orden de prioridad por defecto (no se indicó ponderación): excluyentes → precio → plazo → deseables → pago.

- Las elegibles son B (USD 510.000, la de menor precio) y C (USD 524.400).
- C está a USD 14.400 (2,82 %) de B, dentro del 5 % (14.400 / 510.000).
- C es mejor en condiciones: cumple D1 y D2, mientras B solo cumple D1. Tiene pago a 60 días contra 30 y garantía de 24 meses contra 12.
- Por la regla fija del Paso 5 corresponde recomendar C, aunque B entrega 13 días antes (30/11 contra 13/12).

Punto de atención: C entrega el 13/12, solo 2 días antes del límite. Si la OC se emitiera después del 28/10, C pasaría el 15/12 y dejaría de cumplir E4 (ver sección 7, g).

### 7) Validación humana requerida

La adjudicación final la decide una persona.

Disparadores que aplican:
- b) La recomendada (C) no es la más barata entre las elegibles: +USD 14.400 (+2,82 %) sobre B. La más barata en total es A (USD 480.000), pero es NO ADJUDICABLE.
- c) Una oferta elegible (C) está a menos del 5 % de la elegible de menor precio (B): diferencia de USD 14.400, 2,82 %.
- g) Condiciones ambiguas o inusuales, solo como observación (no cambian ningún estado):
  - La fecha de referencia indicada es 26/10/2026, pero el mensaje se envió el 29/10/2026. Si la OC se emite el 29/10, los plazos serían: A → 10/12, B → 03/12 y C → 16/12/2026, que incumpliría E4. Confirmar la fecha real de emisión de la OC.
  - B dice "puesto en playa de acopio" sin nombrar "Km 0". Se tomó como el destino requerido.
- i) Documentación mencionada sin verificar: certificados de colada de A, B y C, y los espesores declarados. Verificar contra la documentación real antes de adjudicar.
- j) El monto (USD 524.400 + IVA) podría superar el umbral de aprobación. ¿Cuál es el umbral de aprobación aplicable?

No aplican: a), d), e), f), h) (A está 5,9 % por debajo de B, lejos del 20 %) ni k).

### 8) Consultas sugeridas a proveedores (borrador)
No corresponde: ninguna oferta tiene datos faltantes.

### 9) Supuestos usados
- Los requisitos vienen clasificados por la persona, así que no se aplicó el supuesto conservador.
- Fecha de referencia 26/10/2026 (según lo indicado); OC emitida ese día; plazos en días corridos.
- Comparación en USD y sin IVA: no se necesitó tipo de cambio ni alícuota.
- Precio total = precio por metro × 1.200 m; los tres totales informados coinciden.
- La vigencia de C (20 días desde el 24/10) se tomó como hasta el 13/11/2026.
- "Puesto en playa de acopio" (B) se tomó como entrega en el destino requerido.
- Sin ponderación indicada, se usó el orden: excluyentes → precio → plazo → deseables → pago, con la regla del 5 %.

---
**Lectura contra lo esperado:** coincide con [`../esperados/resultados_esperados.md`](../esperados/resultados_esperados.md) (A no adjudicable por espesor, recomienda C con la cuenta de USD 14.400 / 2,82 %). La mención del 29/10 en el punto g) es del propio asistente, que conoce la fecha real del día; la fecha de referencia del caso es ficticia.
