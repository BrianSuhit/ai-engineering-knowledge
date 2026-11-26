## **Qué es en criollo:** 

Es un predictor estadístico de texto. No "sabe" nada: toma una secuencia de entrada (prompt), calcula la probabilidad matemática de qué pedacito de palabra (token) debería venir después, elige uno según las reglas de muestreo, lo pega al final y repite el proceso en un bucle.

## **Qué problema resuelve:** 

En software tradicional, programar lógica para interpretar lenguaje natural, resumir contratos, extraer entidades o clasificar intenciones requiere miles de reglas manuales y modelos frágiles; el LLM resuelve esto funcionando como una interfaz universal de razonamiento y procesamiento de texto no estructurado en una sola llamada de API.

## **Variables de control:**

```python
from google import genai

client = genai.Client()

interaccion = client.interactions.create(
    model="gemini-4-hydrogen",
    system_instruction="Sos un clasificador estricto de incidencias.",
    input="Ticket #99: Error de conexión a la base de datos",
    generation_config={
        # Nivel de razonamiento interno: "low" (clasificación/extracción), "high" o "xhigh" (lógica/código complejo)
        "thinking_level": "low",
        # Límite máximo de tokens generados (incluye tokens de pensamiento y respuesta visible)
        "max_output_tokens": 1000,
        # Aleatoriedad en la selección (0.0 para código/JSON estructurado, 0.7-1.0 para redacción abierta)
        "temperature": 0.0,
        # Muestreo por núcleo: corta el conjunto acumulado de candidatos (ej. 0.1 o 0.2 para respuestas consistentes)
        "top_p": 0.1,
        # Cantidad fija de tokens candidatos más probables a considerar
        "top_k": 40,
    },
)

print(interaccion.output_text)

```

*(Nota técnica: En modelos modernos de razonamiento como Gemini 4, parámetros clásicos de muestreo probabilístico como **temperature**,* **top_p** *y* **top_k** *se ignoran porque el modelo ajusta internamente su distribución mediante sus pasos de pensamiento; tu palanca principal de control pasa a ser **thinking_level** y el corte con **max_output_tokens**).*

## **La trampa o error comun:** 

Asumir que "entiende", que tiene sentido común o que podés confiar ciegamente en lo que escupe . Como es un generador estadístico de texto, si le pedís una referencia, un cálculo complejo o un dato que no vio, te va a inventar una respuesta con una seguridad gramatical impecable (**alucinación fáctica**). Además, al ser no determinista, la misma consulta puede darte resultados distintos si no controlás la llamada.