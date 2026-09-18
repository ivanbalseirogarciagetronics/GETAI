# 🚀 IA Local con Ollama: De Cero a Experto

Ollama se ha consolidado como el estándar de la industria para ejecutar Modelos de Lenguaje Grande (LLMs) localmente de forma eficiente, privada y sin dependencias de la nube. Esta guía técnica proporciona los conocimientos, enlaces y configuraciones necesarias para dominar el ecosistema de IA local desde computadoras portátiles básicas hasta clústeres de supercomputación corporativa.

---

## 🛑 1. Descarga e Instalación por Sistema Operativo

Ollama se ejecuta de forma nativa aprovechando las mejores API de aceleración por hardware (CUDA, ROCm, Metal). Utiliza los siguientes enlaces oficiales y métodos de instalación seguros:

### 🪟 Windows
*   **Enlace de descarga directa:** [Instalador de Ollama para Windows](https://ollama.com "Ollama Windows Download")
*   **Requisitos:** Windows 10 o posterior. Detecta automáticamente GPUs NVIDIA (vía CUDA) y AMD (vía ROCm) para derivar la computación de forma nativa.

### 🍏 macOS
*   **Enlace de descarga directa:** [Ollama para macOS (Zip oficial)](https://ollama.com "Ollama macOS Download")
*   **Requisitos:** Se recomienda ampliamente procesadores Apple Silicon (M1, M2, M3, M4 en todas sus variantes: Base, Pro, Max, Ultra). macOS aprovecha la **Memoria Unificada** convirtiendo toda la RAM del sistema en VRAM de alta velocidad para el modelo.

### 🐧 Linux
Para sistemas basados en Linux, la instalación se realiza de forma directa mediante una sola línea de comando que configura los controladores de GPU y el servicio `systemd`:
```bash
curl -fsSL https://ollama.com | sh
```
*   *Nota avanzada para contenedores:* Si prefieres aislar tu entorno, puedes utilizar la imagen oficial de [Ollama en Docker Hub](https://docker.com "Ollama Docker Official").

---

## 🧠 2. Fundamentos de las Cuantizaciones (¿Cómo encajar modelos gigantes en RAM?)

Un modelo nativo se entrena en formatos de alta precisión como **FP16** (16 bits flotantes por parámetro), lo que requiere aproximadamente 2GB de memoria por cada 1.000 millones de parámetros (1B). 

La **cuantización** es una técnica matemática de compresión que reduce el peso de estos parámetros a representaciones de 4 u 8 bits. Ollama automatiza esto utilizando formatos avanzados basados en `GGML` y `GGUF`:

*   **Etiqueta estándar / por defecto (sin especificar bits):** Generalmente descarga la cuantización **Q4_K_M** (4 bits con un método de cuantización por bloques balanceado). Ofrece una pérdida de precisión casi imperceptible para el ojo humano y reduce los requisitos de memoria a la mitad.
*   **q8_0 / Q8_K:** Cuantización de 8 bits. Retiene prácticamente el 99% de la precisión del modelo original en FP16, ideal si cuentas con margen de memoria extra y requieres alta precisión lógica o de código.
*   **fp16:** El modelo intacto. Conserva la máxima fidelidad matemática, pero requiere una infraestructura de GPU empresarial muy costosa.

---

## 📊 3. Matriz de Hardware vs. Modelos Recomendados (4GB a 256GB+ RAM)

A continuación se detalla la hoja de ruta definitiva para elegir tus modelos en función de las especificaciones de tu máquina. El comando provisto se introduce directamente en tu terminal para descargar y ejecutar el modelo de forma inmediata.

### 🟩 Perfil 1: Ultra-Ligero (4GB a 8GB RAM / VRAM)
*   **Hardware común:** Laptops de oficina, MacBooks base, PCs de escritorio sin gráfica dedicada antigua.
*   **Estrategia:** Modelos ultracompactos (de 1B a 4B parámetros) con cuantización de 4 bits.
    *   **Llama 3.2 (3B):** El modelo pequeño estándar de Meta. Excelente velocidad y comprensión general.
        ```bash
        ollama run llama3.2
        ```
    *   **Qwen 2.5 (3B):** Altamente competente en múltiples idiomas, especialmente lógica matemática y programación ligera.
        ```bash
        ollama run qwen2.5:3b
        ```
    *   **Gemma 2 (2B):** Arquitectura optimizada de Google. Respuestas refinadas e ideal para tareas de extracción rápida.
        ```bash
        ollama run gemma2:2b
        ```

### 🟦 Perfil 2: Estándar Comercial (16GB RAM o GPUs de 8GB VRAM)
*   **Hardware común:** Computadoras de desarrollo estándar, portátiles gaming de entrada, GPUs dedicadas tipo RTX 3060/4060 o MacBooks con 16GB o 18GB de memoria unificada.
*   **Estrategia:** Modelos estándar de la industria (7B a 8B parámetros) en cuantizaciones de 4 o 5 bits.
    *   **Llama 3.1 (8B):** El modelo versátil por excelencia a nivel global, con ventana de contexto nativa expandida.
        ```bash
        ollama run llama3.1
        ```
    *   **Mistral (7B v0.3):** Excelente alternativa europea de código abierto, sumamente rápido y eficiente en lógica de diálogos.
        ```bash
        ollama run mistral
        ```
    *   **Qwen 2.5 (7B):** Supera a muchos modelos de su tamaño en tareas técnicas y generación de código complejo.
        ```bash
        ollama run qwen2.5:7b
        ```

### 🟪 Perfil 3: Avanzado Profesional (32GB RAM o GPUs de 12GB-16GB VRAM)
*   **Hardware común:** Estaciones de trabajo, MacBooks con 36GB de memoria unificada, PCs gaming con GPUs de gama alta (RTX 4070 Ti / 4080).
*   **Estrategia:** Modelos de rango medio-alto (14B a 35B parámetros).
    *   **Gemma 2 (27B):** Ofrece un rendimiento sorprendentemente cercano a modelos el doble de grandes gracias a su arquitectura refinada.
        ```bash
        ollama run gemma2:27b
        ```
    *   **Qwen 2.5 (14B):** El balance perfecto entre tamaño, velocidad de inferencia y alto nivel intelectual.
        ```bash
        ollama run qwen2.5:14b
        ```
    *   **Command R (35B):** Diseñado específicamente por Cohere para tareas empresariales complejas y arquitecturas RAG (Búsqueda Recuperada y Aumentada).
        ```bash
        ollama run command-r
        ```

### 🟨 Perfil 4: Entusiasta / Estación de Trabajo Pesada (64GB a 96GB RAM)
*   **Hardware común:** PCs equipadas con múltiples GPUs de consumo (ej. 2x RTX 3090/4090 de 24GB unidas mediante puente o PCIe), Mac Studio / MacBook Pro con 64GB, 96GB o 128GB de memoria unificada.
*   **Estrategia:** Modelos grandes corporativos y arquitecturas avanzadas Mixture of Experts (MoE).
    *   **Llama 3.1 (70B):** Inteligencia de grado empresarial capaz de realizar razonamientos abstractos profundos y análisis sintácticos masivos.
        ```bash
        ollama run llama3.1:70b
        ```
    *   **Mixtral 8x7B (MoE):** Una arquitectura de mezcla de expertos de Mistral AI. Aunque pesa como un modelo grande, solo activa una fracción de sus parámetros por token, logrando una velocidad fulgurante.
        ```bash
        ollama run mixtral
        ```
    *   **Qwen 2.5 (72B):** Uno de los modelos abiertos más potentes del ecosistema en su categoría cuantitativa.
        ```bash
        ollama run qwen2.5:72b
        ```

### 🟥 Perfil 5: Nivel de Datos Corporativo / Enterprise (2x DGX Spark o Servidores con 256GB+ RAM/VRAM)
*   **Hardware común:** Servidores multi-nodo basados en GPUs de arquitectura corporativa (NVIDIA H100, A100 o clústeres unificados mediante interconexiones de alta velocidad como InfiniBand o NVLink).
*   **Estrategia:** Los titanes del código abierto sin compromisos de cuantización agresiva, o ejecuciones en FP16.
    *   **Llama 3.1 (405B):** El primer modelo abierto de frontera capaz de competir directamente con soluciones cerradas como GPT-4. Ejecución estable en Q4 o Q8 con esta infraestructura.
        ```bash
        ollama run llama3.1:405b
        ```
    *   **Mixtral 8x22B (MoE):** La evolución masiva de Mistral con capacidades extraordinarias de razonamiento matemático, de programación y soporte multilingüe nativo a gran escala.
        ```bash
        ollama run mixtral:8x22b
        ```

---

## 🛠️ 4. Guía de Comandos Avanzados de la CLI de Ollama

El manejo fluido de la interfaz de comandos (CLI) permite administrar de forma óptima los recursos del equipo:

```bash
# Comprobar qué versión de Ollama está instalada actualmente
ollama --version

# Listar de forma tabular todos los modelos que tienes descargados localmente
ollama list

# Ver metadatos detallados de un modelo específico (arquitectura de capas, sistema de tokens, etc.)
ollama show llama3.1

# Forzar la descarga o actualización de un modelo del registro oficial a tu disco duro
ollama pull qwen2.5:7b

# Eliminar de manera inmediata un modelo para liberar espacio en disco duro o SSD
ollama rm mistral

# Detener o liberar de la memoria RAM/VRAM cualquier modelo que esté activo en segundo plano
# (Ollama descarga automáticamente el modelo tras un periodo de inactividad de 5 minutos por defecto)
```

---

## ⚙️ 5. Variables de Entorno del Servidor para Expertos

Para entornos de alta concurrencia, producción o configuraciones de red personalizadas, debes inyectar las siguientes variables de entorno en el demonio/servicio de Ollama (ej. modificando el servicio de systemd en Linux, las variables globales en Windows o el archivo plist de arranque en macOS):

*   `OLLAMA_HOST`: Por defecto es `127.0.0.1:11434`. Cambiándolo a `0.0.0.0:11434` permitirás que cualquier dispositivo en tu red local (LAN) consuma la API del servidor.
*   `OLLAMA_NUM_PARALLEL`: Controla el procesamiento de peticiones simultáneas. Si configuras `OLLAMA_NUM_PARALLEL=4`, el servidor procesará de forma concurrente las solicitudes de 4 usuarios dividiendo los hilos de computación sin encolar las peticiones.
*   `OLLAMA_MAX_LOADED_MODELS`: Modifica cuántos modelos diferentes pueden residir simultáneamente cargados en tu memoria RAM/VRAM de forma concurrente.
*   `OLLAMA_KEEP_ALIVE`: Define el tiempo que el modelo permanece en memoria tras recibir la última petición (por ejemplo, `30m` para mantenerlo cargado durante media hora, o `-1` para mantenerlo indefinidamente cargado en memoria).

---

