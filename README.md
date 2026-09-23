# GETAI: Repositorio de Getronics para la IA

Bienvenido al repositorio oficial de Inteligencia Artificial local de Getronics (**GETAI**). Esta documentación está diseñada para guiarte paso a paso —desde un nivel básico ("tostadora") hasta un nivel experto con arquitecturas avanzadas— en la ejecución de modelos de lenguaje grande (LLMs) de manera totalmente local en tu equipo.

---

## 📋 Tabla de Contenidos
1. [Nivel 1: Básico ("La Tostadora") - Hardware Limitado](#nivel-1-básico-la-tostadora---hardware-limitado)
2. [Nivel 2: Intermedio - Gama Media / 32 GB RAM](#nivel-2-intermedio---gama-media--32-gb-ram)
3. [Nivel 3: Avanzado - Gama Alta / 64 GB RAM e iGPU](#nivel-3-avanzado---gama-alta--64-gb-ram-e-igpu)
4. [Nivel 4: Experto - Modelos MoE (Mixture of Experts) y Optimización Pro](#nivel-4-experto---modelos-moe-mixture-of-experts-y-optimización-pro)
5. [Guía de Configuración Rápida con LM Studio](#guía-de-configuración-rápida-con-lm-studio)

---

## Nivel 1: Básico ("La Tostadora") - Hardware Limitado
Si tu ordenador cuenta con recursos limitados (por ejemplo, 8 GB o 16 GB de RAM sin una GPU dedicada potente), aún puedes ejecutar modelos optimizados de tamaño reducido (bajo conteo de parámetros) mediante cuantización GGUF.

* **Herramienta recomendada:** [LM Studio](https://lmstudio.ai/)
* **Modelo recomendado:** `empero-ai/Qwen3.8-4B-Distill-GGUF`
* **Enlace directo al modelo:** [Hugging Face - Qwen3.8-4B-Distill](https://huggingface.co/empero-ai/Qwen3.8-4B-Distill-GGUF)

> **Consejo Pro:** Asegúrate de descargar cuantizaciones ligeras (como `Q4_K_M` o `Q5_K_M`) para asegurar que el modelo quepa enteramente en la memoria disponible sin saturar el sistema operativo.

---

## Nivel 2: Intermedio - Gama Media / 32 GB RAM
Si dispones de un equipo más capaz con **32 GB de RAM**, puedes permitirte saltar a modelos de mayor tamaño que ofrecen capacidades de razonamiento significativamente superiores y menor tasa de alucinaciones.

* **Modelo recomendado:** `empero-ai/Qwen3.8-9B-Distill-GGUF`
* **Enlace directo al modelo:** [Hugging Face - Qwen3.8-9B-Distill](https://huggingface.co/empero-ai/Qwen3.8-9B-Distill-GGUF)

---

## Nivel 3: Avanzado - Gama Alta / 64 GB RAM e iGPU
Para estaciones de trabajo o portátiles de alta gama con **64 GB de RAM**, la limitación principal deja de ser el almacenamiento en memoria RAM y pasa a ser el ancho de banda y la capacidad de procesamiento de la tarjeta gráfica integrada (**iGPU**) o dedicada.

* **Modelo recomendado:** `unsloth/Qwen3.8-27B-GGUF`
* **Enlace directo al modelo:** [Hugging Face - Qwen3.8-27B](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF)

> ⚠️ **Nota técnica:** Al utilizar modelos de 27B con arquitecturas de memoria unificada o iGPU, configura adecuadamente la offloadización de capas (`GPU Offload`) en LM Studio para equilibrar la carga entre la CPU y la GPU integrada y evitar cuellos de botella.

---

## Nivel 4: Experto - Modelos MoE (Mixture of Experts) y Optimización Pro
Cuando se requiere una **agilidad superior en las respuestas** (baja latencia y alta tasa de tokens por segundo) sin sacrificar la capacidad de comprensión general del modelo, los modelos tradicionales densos pueden volverse lentos en hardware local.

### ¿Por qué elegir Modelos MoE?
* **Arquitectura de Expertos:** Solo un subconjunto de redes neuronales (expertos) se activa por cada token procesado.
* **Eficiencia:** Ofrecen el rendimiento de un modelo grande con la velocidad de ejecución de un modelo mucho más pequeño.
* **Casos de uso:** Ideal para flujos de trabajo en tiempo real, agentes autónomos locales y desarrollo de software asistido.

---

## Guía de Configuración Rápida con LM Studio

1. **Instalación:** Descarga e instala [LM Studio](https://lmstudio.ai/) en tu sistema operativo (Windows, macOS o Linux).
2. **Búsqueda del Modelo:** Copia el identificador de Hugging Face del modelo adecuado para tu hardware (por ejemplo, `empero-ai/Qwen3.8-4B-Distill-GGUF`) y pégalo en la barra de búsqueda interna de LM Studio.
3. **Descarga:** Selecciona la variante de cuantización GGUF óptima para tu memoria disponible y pulsa **Download**.
4. **Carga y Chat:** Dirígete a la pestaña de chat (ícono de bocadillo), selecciona el modelo descargado en la parte superior y ajusta los parámetros de contexto (`Context Length`) y temperatura según tus necesidades.
