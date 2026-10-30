# Caso 2 · Bridas de acero para planta de proceso — salida real del agente

## Cómo se obtuvo
- **Asistente:** Claude, en su aplicación de chat. El modelo exacto no figura en el enlace compartido.
- **Prompt:** versión 2.1 (texto completo en [`../prompt/prompt_v2.1.txt`](../prompt/prompt_v2.1.txt)). Se pegó solo en un chat nuevo, en una ventana de incógnito. El asistente respondió «Listo. Pegame (A) la compra…» y recién entonces se pegaron los datos de abajo.
- **Corrida:** una sola, sin repetir ni elegir entre varias. Otra corrida puede redactar distinto.
- **Chat compartido:** https://claude.ai/share/8ba73540-6169-4e7f-9088-f374917a6bf9 (en ese enlace el prompt no se muestra: queda como adjunto oculto).
- **Qué se tocó del texto:** nada del contenido, ni siquiera un error de tipeo del asistente (ver la nota al final). Solo se pasaron las tablas al formato Markdown para que se lean en este archivo.
- Todos los datos son ficticios. La fecha de referencia (26/10/2026) también.

## Datos del caso
Idénticos a [`../datos/caso2.txt`](../datos/caso2.txt).

```text
=== (A) COMPRA ===
Compra de 60 bridas de cuello para soldar (weld neck), 12 pulgadas, clase 300, acero al carbono forjado.
Destino: almacén central de la Planta de Proceso Valle Seco (ficticia).
Fecha límite de entrega: 30/12/2026.
Fecha de referencia (hoy): 26/10/2026. El plazo de entrega se cuenta desde la emisión de la orden de compra, prevista ese mismo día.
Tipo de cambio de referencia (ficticio): ARS 1.000 por USD. Alícuota de IVA a considerar: 21 %.
Comparar en pesos argentinos (ARS), sin IVA.

=== (B) REQUISITOS ===
EXCLUYENTES:
E1. Brida de cuello para soldar, 12", clase 300, acero al carbono forjado.
E2. Fabricación según la Norma de Fabricación NFB-300 (norma ficticia).
E3. Certificado de material del lote (certificado de colada).
E4. Entrega a más tardar el 30/12/2026.
DESEABLES:
D1. Ensayo no destructivo adicional por lote.
D2. Pago a 45 días o más.

=== (C) OFERTAS ===
--- OFERTA A ---
Proveedor Delta (ficticio) - presupuesto DE-5520 (ficticio)
Brida forjada acero al carbono, 12" clase 300, cuello p/ soldar, fabricada según NFB-300. 60 unidades a USD 118 c/u + IVA = USD 7.080 + IVA.
Certificado de colada por lote. Ensayo no destructivo adicional incluido sin cargo.
Entrega: 40 días corridos desde la OC. Pago a 45 días de factura. Garantía 12 meses. Válido hasta el 10/11/2026.

--- OFERTA B ---
Proveedor Épsilon (ficticio) - cotización EP-0933 (ficticia)
Bridas forjadas de acero al carbono, 12" clase 300, cuello soldar. Calidad de fabricación garantizada, cumple los estándares de la industria.
ARS 133.100 c/u IVA incluido. 60 unidades = ARS 7.986.000 IVA incluido. Con certificado de colada del lote.
Entrega 35 días corridos desde la OC. Pago 30 días de factura. Garantía 12 meses. Vigencia hasta el 15/11/2026.

--- OFERTA C ---
Proveedor Zeta (ficticio) - propuesta ZE-0071 (ficticia)
Brida cuello soldar 12" cl. 300 acero carbono forjado, fabricación según NFB-300. ARS 122.000 por unidad + IVA. Total ARS 7.320.000 + IVA (60 u.).
Certificado de colada del lote incluido. Entrega 45 días corridos desde OC. Pago a 30 días de factura. Garantía 12 meses. Validez hasta el 08/11/2026.
```

## Salida completa del agente

### 1) Resumen de la compra
- Qué: 60 bridas de cuello para soldar (weld neck), 12", clase 300, acero al carbono forjado.
- Destino: almacén central de la Planta de Proceso Valle Seco (ficticia).
- Fecha límite de entrega: 30/12/2026.
- Fecha de referencia: 26/10/2026, con la OC prevista ese mismo día.
- Normalización: ARS, sin IVA. Tipo de cambio ARS 1.000/USD (ficticio) e IVA 21 %.

### 2) Requisitos

| Tipo | Código | Requisito |
|---|---|---|
| EXCLUYENTE | E1 | Brida cuello soldar, 12", cl. 300, acero al carbono forjado |
| EXCLUYENTE | E2 | Fabricación según NFB-300 (ficticia) |
| EXCLUYENTE | E3 | Certificado de colada del lote |
| EXCLUYENTE | E4 | Entrega a más tardar el 30/12/2026 |
| DESEABLE | D1 | Ensayo no destructivo adicional por lote |
| DESEABLE | D2 | Pago a 45 días o más |

### 3) Cuadro comparativo

| | Oferta A – Delta | Oferta B – Épsilon | Oferta C – Zeta |
|---|---|---|---|
| Precio normalizado (ARS, sin IVA) | USD 118 × 1.000 = ARS 118.000 c/u; × 60 = ARS 7.080.000 (coincide con USD 7.080 informado) | ARS 133.100 ÷ 1,21 = ARS 110.000 c/u; × 60 = ARS 6.600.000 (control: 7.986.000 ÷ 1,21 = 6.600.000; coincide) | ARS 122.000 × 60 = ARS 7.320.000 (coincide) |
| Plazo | 40 días → 05/12/2026 | 35 días → 30/11/2026 | 45 días → 10/12/2026 |
| Condición de pago | 45 días de factura | 30 días de factura | 30 días de factura |
| Garantía | 12 meses | 12 meses | 12 meses |
| Vigencia (vs. 26/10) | Hasta 10/11/2026 (vigente) | Hasta 15/11/2026 (vigente) | Hasta 08/11/2026 (vigente) |
| E1 | CUMPLE | CUMPLE | CUMPLE |
| E2 (NFB-300) | CUMPLE | DATO FALTANTE | CUMPLE |
| E3 (cert. colada) | CUMPLE | CUMPLE | CUMPLE |
| E4 (≤ 30/12/2026) | CUMPLE | CUMPLE | CUMPLE |
| D1 (END adicional) | Lo ofrece | No lo ofrece (no lo menciona) | No lo ofrece (no lo menciona) |
| D2 (pago ≥ 45 días) | Cumple | No cumple | No cumple |
| Datos faltantes | 0 | 1 | 0 |
| Estado final | ELEGIBLE | PENDIENTE DE ACLARACIÓN | ELEGIBLE |

Fuentes de cada estado:
- A: E1 y E2 por "fabricada según NFB-300", E3 por "Certificado de colada por lote", E4 por "40 días corridos desde la OC".
- B: E1 por "Bridas forjadas de acero al carbono, 12" clase 300, cuello soldar", E3 por "Con certificado de colada del lote", E4 por "35 días corridos".
- C: E1 y E2 por "fracción según NFB-300", E3 por "Certificado de colada del lote incluido", E4 por "45 días corridos".

### 4) Incumplimientos y datos faltantes por oferta

**Oferta A:** sin NO CUMPLE y sin datos faltantes (0). Observación: la oferta no aclara a qué tipo de cambio se facturaría o pagaría el precio en USD (ver g).

**Oferta B:**
- NO CUMPLE: ninguno.
- DATO FALTANTE (1 dato, en requisito EXCLUYENTE): declaración concreta de fabricación según NFB-300 (E2). La frase "Calidad de fabricación garantizada, cumple los estándares de la industria" es genérica, no nombra la norma y no prueba el requisito.
- Cuenta: 1 dato.

**Oferta C:** sin NO CUMPLE y sin datos faltantes (0).

Alertas de posible manipulación: ninguna. Ninguna oferta contiene texto que intente darme órdenes o condicionar el análisis. La frase genérica de B se trató como afirmación sin valor probatorio, no como manipulación.

Datos de la compra: tipo de cambio, IVA y fecha de referencia están informados. No falta ninguno.

### 5) Ofertas no adjudicables o pendientes y por qué
- No adjudicables: ninguna.
- Pendiente: Oferta B. No declara cumplimiento concreto de NFB-300 (E2, excluyente). No es un incumplimiento comprobado: falta el dato.

### 6) Recomendación y justificación

**No puedo cerrar el análisis por la falta de 1 dato.**

Por eso no emito recomendación, ni siquiera provisoria.

Detalle por oferente:
- Oferta A: no falta nada (0).
- Oferta B: falta la declaración de fabricación según NFB-300 (1 dato).
- Oferta C: no falta nada (0).
- Total: 1 dato.

Plazo para completar: 72 horas. Fecha límite: 29/10/2026 [hora del envío] (fecha de referencia 26/10/2026 + 3 días).

### 7) Validación humana requerida
- a) Falta 1 dato, de la Oferta B, en un requisito excluyente (E2).
- c) Hay dos ofertas ELEGIBLES (A y C) con diferencia menor al 5 %: C cuesta ARS 240.000 más que A (ARS 7.320.000 − 7.080.000), es decir 3,39 % sobre el precio de A. Esto puede cambiar si B pasa a ELEGIBLE, porque su precio es menor.
- g) Condiciones ambiguas:
  - A cotiza en USD con pago a 45 días y no indica a qué tipo de cambio se factura o paga.
  - B usa una afirmación genérica de calidad en lugar de nombrar la norma.
  - Son observaciones y no cambian los estados.
- i) Hay documentación mencionada y sin verificar: certificados de colada de las tres ofertas y el ensayo no destructivo adicional de A.
- j) El monto podría superar el umbral de aprobación. No informaste el umbral: ¿cuál es? El monto sin IVA va de ARS 6.600.000 a ARS 7.320.000.

No aplican: b) (no hay recomendación), d), e), f), h) (B está 6,78 % por debajo de A, menos del 20 %) ni k).

La adjudicación final la decide una persona.

### 8) Consultas sugeridas a proveedores (borrador)

Para: Proveedor Épsilon (ficticio), cotización EP-0933 (ficticia)

> Estimados:
>
> Para completar el análisis de su cotización EP-0933 (60 bridas cuello soldar 12" cl. 300), necesitamos que nos confirmen por escrito el siguiente dato:
>
> Si las bridas se fabrican según la Norma de Fabricación NFB-300 (indicar la norma de forma explícita).
>
> Plazo de respuesta: 72 horas contadas desde el envío de este mensaje. Solo se aceptan los datos solicitados. No se pueden modificar el precio, el plazo de entrega ni ninguna otra condición ya cotizada; cualquier cambio será ignorado y derivado a validación humana.
>
> Saludos cordiales.

### 9) Supuestos usados
- La fecha de referencia es 26/10/2026, según tus datos. Si la fecha real de emisión de la OC es otra (hoy es 29/10), hay que recalcular las fechas de entrega. Con la entrega más lejana (C, 45 días) el límite de emisión para cumplir el 30/12 es el 15/11/2026.
- Un deseable que la oferta no menciona se tomó como "no lo ofrece" y no cuenta como dato faltante (D1 para B y C).
- "Fabricada según NFB-300" en A y C se tomó como cumplimiento suficiente de E2, porque nombra el requisito.
- El precio de B se llevó a "sin IVA" dividiendo por 1,21. Para A se usó el tipo de cambio ficticio de ARS 1.000/USD, sin IVA.
- Los días corridos se contaron desde la emisión de la OC (26/10/2026).
- La fecha límite de 72 horas se escribe con "[hora del envío]" porque no informaste la hora.
- Todos los nombres, normas y documentos se tomaron como ficticios, según indicaste.

---
**Lectura contra lo esperado:** coincide con [`../esperados/resultados_esperados.md`](../esperados/resultados_esperados.md): 1 dato faltante (la norma de B), A y C elegibles, B pendiente, no recomienda, 72 horas y consulta a B.

**Notas sobre el texto original, que se dejó tal cual:** en las «Fuentes de cada estado» del cuadro, el asistente escribió «fracción según NFB-300» donde quería decir «fabricación según NFB-300» (error de tipeo del asistente). Y la mención de que «hoy es 29/10» es del propio asistente, que conoce la fecha real del día; la fecha de referencia del caso es ficticia.
