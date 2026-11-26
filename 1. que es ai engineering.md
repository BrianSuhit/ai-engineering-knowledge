## **Qué es en criollo:**

Es construir software tradicional conectándole modelos de lenguaje (a partir de ahora le decimos LLM) ya entrenados mediante APIs, sin meterse a programar redes neuronales ni entrenar modelos desde cero.

## **Qué problema resuelve:** 

Resuelve el abismo entre hacer una demo rápida y tener un producto estable en producción; si no existe, tenés una app que alucina datos falsos, tarda 10 segundos por petición, se vuelve impagable por consumo desmedido de tokens y queda vulnerable a inyecciones de código o prompt.

### **El 90% del trabajo sera meter cosas ahi dentro:**

```python
from google import genai

client = genai.Client()

# Llamada atómica conectando el modelo con rol y datos delimitados
interaccion = client.interactions.create(
    model="gemini-4-hydrogen",
    system_instruction="Sos un clasificador estricto. Respondé únicamente: 'URGENTE' o 'NORMAL'.",
    input="Ticket #402: El servidor principal de base de datos no responde."
)

print(interaccion.output_text)
```

## **La trampa o error común:** 

Asumir que un LLM es determinista como una base de datos o un backend clásico; si no implementás esquemas estrictos de validación, reintentos controlados y límites de contexto, el sistema va a fallar silenciosamente en producción con respuestas impredecibles.

> **Y acordate:** *no piensa, no razona, no imagina, no te miente ni tiene malas intenciones; es simplemente un cálculo matemático prediciendo la siguiente palabra más probable. Si falla, el error no es de la "inteligencia", es de tu arquitectura.*