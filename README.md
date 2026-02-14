# GEMA — Asistente Educativo Modular UNIFRANZ
Repositorio: https://github.com/WinTux/20260214
## Descripción
GEMA (Generador Educativo Multiasistente Académico) es un asistente inteligente modular diseñado para
ayudar a estudiantes UNIFRANZ en tareas académicas, organización de estudio, resúmenes, investigación y evaluaciones.

## Contenido del paquete
Este archivo comprimido contiene:
- PROMPT_MAESTRO_GEMA.txt → Prompt orquestador principal.
- microPY_*.txt → Micro-proyectos especializados.
- README_GEMA.md → Documentación de uso.

## Arquitectura
GEMA funciona combinando el prompt maestro con un micro-proyecto específico según la intención del usuario.
Cada micro-proyecto define reglas específicas para que el modelo responda de forma ajustada.

## Requisitos Previos
1) Python 3.8+
2) API de Gemini/GPT configurada
3) Librería OpenAI
4) Entorno virtual

## Instalación
1) Crear entorno:
```bash
python -m venv venv
source venv/bin/activate # para GNU/Linux, MacOS
venv\Scripts\activate # para Windows
```
2) Instalar dependencias:
```bash
pip install openai python-dotenv
```
3) Crear archivo `.env` con el API Key:
```bash
OPENAI_API_KEY="mi_api_key"
```

## Lógica de Integración
1) Leer entrada del estudiante.
2) Cargar `PROMPT_MAESTRO_GEMA.txt`.
3) Detectar intención (por ejemplo: "quiero un resumen", "explícame X").
4) Cargar microPY correspondiente.
5) Concatenar maestro + microPY + pregunta del estudiante.
6) Enviar al modelo LLM.
7) Mostrar resultados.

## Ejemplo de implementación en Python
```python
from openai import OpenAI

client = OpenAI()

def run_gema(user_input):
# 1) Cargar prompt maestro
maestro = open("PROMPT_MAESTRO_GEMA.txt").read()
# 2) Detectar intención (simple regla basada en palabras clave)
if "resumen" in user_input.lower():
    micro = open("microPY_GENERADOR_RESUMENES.txt").read()
elif "estudiar" in user_input.lower():
    micro = open("microPY_PLANIFICADOR_ESTUDIO.txt").read()
else:
    micro = open("microPY_TUTOR_ACADEMICO.txt").read()

prompt = maestro + "\n" + micro + "\nUsuario: " + user_input

response = client.chat.completions.create(
    model="gemini-pro-v1",
    messages=[{"role": "user", "content": prompt}],
    max_tokens=650
)
return response.choices[0].message.content

```
## Consejos para una siguiente iteración
- Podría versioar los microPY con números (ej. v1.0, v1.1).
- Agregar validaciones de intención más elaboradas o avanzadas.
- Usar métricas de satisfacción para medir impacto.
- Se prodrían extender los micro-proyectos según carrera.

## Mantenimiento
- Se deben mantener los micro-proyectos independientes.
- Se debe procurar actualizar el prompt maestro cuando se requiera.
- Se añadirían nuevos módulos según necesidad docente.

## Puesta en marcha
1) Extraer ZIP
2) Configurar `.env`
3) Ejecutar script principal
4) Comenzar sesiones con estudiantes
