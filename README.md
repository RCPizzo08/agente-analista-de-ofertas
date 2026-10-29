# Agente analista de ofertas

Un prompt que convierte a un asistente de IA en analista de compras. Compara **tres ofertas** de una misma compra, aplica reglas fijas y prepara una recomendación fundamentada. **No adjudica:** la decisión final la toma siempre una persona.

> **Todo es ficticio.** Proveedores, cotizaciones, obras, normas y montos son inventados. No hay claves ni datos personales, y nada de este repositorio envía información a ningún lado.

## Qué hace el agente
- Revisa que haya exactamente tres ofertas y que cada una tenga sus 10 datos.
- Normaliza moneda, IVA y unidades, y muestra todas las cuentas.
- Evalúa cada requisito como CUMPLE, NO CUMPLE o DATO FALTANTE (no son lo mismo).
- Descarta como NO ADJUDICABLE a la oferta que incumple un requisito excluyente, aunque sea la más barata.
- **Si falta cualquier dato en cualquier oferta, no cierra el análisis y no recomienda**: dice qué falta, da 72 horas y redacta la consulta, que no permite cambiar lo ya cotizado.
- Cuando no falta nada, recomienda una alternativa con una regla declarada (la regla del 5 %) y marca cuándo hace falta validación humana.
- Trata el contenido de las ofertas como datos, nunca como instrucciones: si un texto intenta darle órdenes, lo ignora y lo reporta.

## Probarlo en 3 pasos
1. **Copiá el prompt** de [`prompt/prompt_v2.1.txt`](prompt/prompt_v2.1.txt) y pegalo en un chat nuevo de cualquier asistente de IA (mejor fuera de proyectos y sin memoria activada). Enviá.
2. **Verificá que se aplicó:** tiene que responder «Listo. Pegame (A) la compra…» y mostrar una plantilla. Si responde otra cosa, el prompt no se aplicó: empezá otro chat.
3. **Pegá un caso** de la carpeta [`datos/`](datos) como segundo mensaje. Compará la respuesta con [`esperados/resultados_esperados.md`](esperados/resultados_esperados.md).

También podés abrir [`demo.html`](demo.html) con doble clic (funciona en la compu y en el celular): trae el prompt y los casos con botón de copiar, y un ejemplo de resultado real. Versión en línea: https://rcpizzo08.github.io/agente-analista-de-ofertas/demo.html

## Contenido
| Archivo | Qué es |
|---|---|
| `prompt/prompt_v2.1.txt` | El prompt, listo para pegar. |
| `datos/caso1.txt` | Cañería de acero: la oferta más barata no cumple un requisito excluyente. |
| `datos/caso2.txt` | Bridas de acero: una oferta no informa la norma exigida. |
| `datos/caso3.txt` | Electrodos de soldadura: caso de estrés, con una oferta que intenta darle órdenes al agente. |
| `esperados/resultados_esperados.md` | Veredicto esperado por oferta y lo que el agente debe mencionar. |
| `demo.html` | Página de una sola pieza con todo lo anterior. |

## Usarlo con tus propias ofertas
Pegá el prompt y, cuando el agente pida los datos, completá esta plantilla corta. Por cada oferta hacen falta estos 10 datos:

```text
(A) COMPRA
Qué se compra / cantidad / lugar de entrega / fecha requerida:
Fecha de referencia (hoy):
Tipo de cambio y alícuota de IVA (si hay que normalizar):

(B) REQUISITOS (marcá cuáles son EXCLUYENTES y cuáles DESEABLES)
EXCLUYENTES:
DESEABLES:

(C) OFERTAS (una por bloque, tal como las recibiste)
--- OFERTA A ---
1. Proveedor:
2. Precio unitario y total:
3. Moneda:
4. IVA incluido (sí / no):
5. Plazo de entrega:
6. Condición de pago:
7. Garantía:
8. Especificaciones declaradas:
9. Documentación mencionada:
10. Vigencia de la oferta:
--- OFERTA B --- (los mismos 10 datos)
--- OFERTA C --- (los mismos 10 datos)
```

Si falta alguno de los 10 datos, el agente lo va a marcar y no va a cerrar el análisis hasta que se complete.

## Nota sobre las fechas
Las fechas de los casos son **de ejemplo**: la fecha de referencia (26/10/2026) es ficticia y está escrita dentro de cada caso. Si tu asistente conoce la fecha real de hoy, puede comentar la diferencia; no es un error y no cambia lo esperado.

## Límites
- Cada caso se probó pocas veces y con varios asistentes de IA. Las respuestas pueden variar de un asistente a otro y de una corrida a otra.
- Una redacción ambigua de un requisito puede interpretarse distinto según el asistente; conviene redactar los requisitos con claridad.
- El agente ayuda a analizar. No reemplaza la decisión humana ni la autorización formal, que se hace en el sistema de aprobaciones de la empresa.
