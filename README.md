# 🏦 Workshop: GitHub Copilot para Contoso Banco

## Desarrollo Asistido por IA con Python

![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-Enabled-green)
![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Flask](https://img.shields.io/badge/Flask-3.x-lightgrey)
![Swagger](https://img.shields.io/badge/Swagger-UI-orange)
![Duración](https://img.shields.io/badge/Duración-2%20horas-red)

---

## 📋 Tabla de Contenidos

- [Introducción](#-introducción)
- [Conceptos Clave de GitHub Copilot](#-conceptos-clave-de-github-copilot)
- [Pre-requisitos](#️-pre-requisitos)
- [Agenda del Workshop](#-agenda-del-workshop)
- [Ejercicio 1: API REST con Copilot](#-ejercicio-1-api-rest-con-copilot-35-40-min)
- [Ejercicio 2: Frontend e Integración](#-ejercicio-2-frontend-e-integración-25-30-min)
- [Ejercicio 3: Tests y Refactoring](#-ejercicio-3-tests-y-refactoring-20-25-min)
- [Referencia Rápida](#-referencia-rápida)
- [Recursos Adicionales](#-recursos-adicionales)

---

## 🎯 Introducción

Este workshop práctico de **2 horas** te guiará en el desarrollo de un **Sistema Bancario para Contoso Banco**, una institución financiera ficticia. Utilizarás **GitHub Copilot** como asistente de desarrollo para construir una aplicación completa con Python. Aprenderás a:

- ✅ Usar el autocompletado y sugerencias inline de Copilot
- ✅ Escribir prompts efectivos que guíen a Copilot con intención
- ✅ Crear una API REST con documentación Swagger automática
- ✅ Desarrollar un frontend que consuma la API
- ✅ Generar pruebas unitarias asistidas por IA
- ✅ Usar Copilot Chat para explicar, corregir y refactorizar código

### Estándares del Proyecto

| Aspecto | Estándar |
|---------|----------|
| Tipo | API REST |
| Tecnología | Python 3.10+ |
| Framework | Flask + Flask-RESTX |
| Documentación API | Swagger UI (integrada) |
| Idioma | Español (código, comentarios, documentación) |
| Base de datos | En memoria (diccionarios Python) |
| Frontend | HTML + JavaScript vanilla + Bootstrap 5 (sin frameworks) |
| Pruebas | pytest |

### Escenario: Contoso Banco 🏦

**Contoso Banco** es una institución financiera que necesita un sistema digital para gestionar:

- **Clientes**: datos personales y de contacto
- **Cuentas bancarias**: tipos de cuenta, saldos, estados
- **Transacciones** *(bonus)*: depósitos, retiros, transferencias

> 💡 La complejidad es **intencionalmente baja**. El objetivo NO es construir una app robusta, sino mostrar cómo **Copilot acelera cada fase del desarrollo**.

> 📝 **Sobre las transacciones:** Los ejercicios guiados cubren **Clientes** y **Cuentas** como funcionalidad principal. Las **Transacciones** se presentan como un desafío opcional al final del Ejercicio 1 para quienes avancen más rápido. No te preocupes si no llegas a completarlas — el valor del workshop está en el proceso, no en terminar todo.

### Arquitectura de la Solución

```
┌─────────────────────────────────────────────────────────────┐
│                     NAVEGADOR WEB                           │
│                                                             │
│  ┌─────────────────────┐    ┌───────────────────────────┐   │
│  │   Frontend HTML     │    │      Swagger UI            │   │
│  │                     │    │   (auto-generado por       │   │
│  │  Bootstrap 5 (CDN)  │    │    Flask-RESTX)            │   │
│  │  JavaScript vanilla │    │                           │   │
│  │  fetch() → API      │    │   /api/doc                │   │
│  │                     │    │                           │   │
│  │  /                  │    │                           │   │
│  └────────┬────────────┘    └─────────┬─────────────────┘   │
│           │                           │                     │
└───────────┼───────────────────────────┼─────────────────────┘
            │  HTTP (mismo origen)       │
            ▼                           ▼
┌─────────────────────────────────────────────────────────────┐
│                    SERVIDOR FLASK                            │
│                    (app.py)                                  │
│                                                             │
│  ┌──────────────┐   ┌──────────────────────────────────┐    │
│  │ render_       │   │        Flask-RESTX API           │    │
│  │ template()   │   │                                  │    │
│  │              │   │  /api/clientes    → CRUD         │    │
│  │ index.html   │   │  /api/cuentas     → CRUD         │    │
│  │              │   │  /api/transacciones → CRUD (*)    │    │
│  └──────────────┘   └──────────┬───────────────────────┘    │
│                                │                            │
│                                ▼                            │
│                  ┌──────────────────────────┐               │
│                  │   Modelos (Python)       │               │
│                  │                          │               │
│                  │  modelos/cliente.py      │               │
│                  │  modelos/cuenta.py       │               │
│                  │  modelos/transaccion.py  │               │
│                  │  (*) = bonus/opcional    │               │
│                  │                          │               │
│                  │  Datos en memoria        │               │
│                  │  (diccionarios Python)   │               │
│                  └──────────────────────────┘               │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  pytest  →  tests/test_cliente.py                    │   │
│  │             tests/test_api.py                        │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘

(*) Transacciones es un desafío opcional (Paso 1.7)
```

**Flujo de la aplicación:**
1. El usuario abre `http://127.0.0.1:5000/` → Flask sirve `index.html` con `render_template`
2. El JavaScript dentro del HTML usa `fetch('/api/...')` para consumir la API REST
3. Flask-RESTX enruta cada petición al endpoint correspondiente (clientes, cuentas, transacciones)
4. Los modelos Python procesan la lógica y almacenan datos en diccionarios en memoria
5. Swagger UI está disponible en `/api/doc` para explorar y probar la API

---

## 🧠 Conceptos Clave de GitHub Copilot

### ¿Qué es GitHub Copilot?

GitHub Copilot es un **asistente de programación impulsado por IA** que se integra directamente en tu editor de código. Funciona como un **par de programación** que:

- Sugiere líneas completas o bloques de código mientras escribes
- Entiende el contexto de tu proyecto (nombres de archivos, comentarios, código existente)
- Aprende de tus patrones y se adapta a tu estilo

### El Arte del Prompting en Copilot

La clave para obtener buenos resultados con Copilot está en **cómo describes lo que necesitas**. Los comentarios descriptivos e intencionales guían mejor a Copilot que instrucciones rígidas paso a paso.

**❌ Prompt débil:**
```python
# función
def f():
    pass
```

**✅ Prompt efectivo:**
```python
# Endpoint para obtener el saldo actual de una cuenta bancaria de Contoso Banco
# Recibe el número de cuenta como parámetro
# Retorna el saldo disponible y la fecha de última actualización
```

> Observa cómo el segundo comentario le da a Copilot **contexto**, **intención** y **detalles** sobre lo que necesitas.

### ¿Qué es @workspace?

El `@workspace` es un participante de chat que proporciona contexto sobre todo tu espacio de trabajo (proyecto) a GitHub Copilot.

| Uso | Ejemplo |
|-----|---------|
| Buscar código | `@workspace ¿dónde se define la clase Cliente?` |
| Entender el proyecto | `@workspace ¿qué hace este proyecto?` |
| Encontrar patrones | `@workspace ¿cómo se implementan los endpoints?` |
| Generar código contextual | `@workspace crea un nuevo endpoint similar a los existentes` |

### Modos de GitHub Copilot Chat

> ⚠️ **Nota importante:** La interfaz de los modos (íconos, ubicación del selector, nombres) puede variar según tu versión de VS Code y la extensión de GitHub Copilot. Si ves una interfaz diferente a la descrita aquí, consulta con el instructor o revisa la [documentación oficial](https://docs.github.com/en/copilot).

GitHub Copilot tiene tres modos principales de operación:

#### 1️⃣ Modo Ask (Preguntar) 💬

| Aspecto | Detalle |
|---------|---------|
| Ícono | 💬 Burbuja de mensaje |
| Función | Solo responde preguntas, **NO** modifica archivos |
| Uso ideal | Explorar, entender, planificar, aprender |

**Ejemplo:**
```
[Modo Ask]
"¿Cuál es la mejor forma de implementar una API REST con Flask y Swagger?"

→ Copilot EXPLICA las opciones pero NO crea archivos
```

#### 2️⃣ Modo Agent (Agente) 🤖

| Aspecto | Detalle |
|---------|---------|
| Ícono | 🤖 Robot o chispa |
| Función | **PUEDE** crear y modificar archivos automáticamente |
| Uso ideal | Implementar cambios, crear código, refactorizar |

**Ejemplo:**
```
[Modo Agent]
"Crea una API REST con Flask-RESTX para gestionar clientes de Contoso Banco"

→ Copilot CREA los archivos con todo el código
```

#### 3️⃣ Modo Plan (Planificar) 📋

| Aspecto | Detalle |
|---------|---------|
| Ícono | 📋 Lista o documento |
| Función | Genera un plan detallado **ANTES** de ejecutar |
| Uso ideal | Tareas complejas que involucran múltiples archivos |

**Ejemplo:**
```
[Modo Plan]
"Implementa la funcionalidad completa de transacciones bancarias 
con modelo, servicio y endpoints"

→ Copilot MUESTRA el plan:
  1. Crear transaccion.py (modelo)
  2. Crear transaccion_servicio.py (lógica de negocio)
  3. Crear transaccion_endpoints.py (API REST)
  4. Registrar endpoints en app.py

→ Tú APRUEBAS cada paso antes de que se ejecute
```

#### Comparativa de Modos

| Característica | Ask 💬 | Agent 🤖 | Plan 📋 |
|----------------|--------|----------|---------|
| Modifica archivos | ❌ No | ✅ Sí | ✅ Sí (con aprobación) |
| Velocidad | Rápido | Rápido | Más lento |
| Control | N/A | Bajo | Alto |
| Ideal para | Aprender | Implementar | Tareas complejas |
| Riesgo | Ninguno | Medio | Bajo |

### Comandos Especiales (/comando)

| Comando | Descripción | Ejemplo de uso |
|---------|-------------|----------------|
| `/tests` | Genera pruebas unitarias | Selecciona código → `/tests` |
| `/doc` | Genera documentación | Selecciona función → `/doc` |
| `/fix` | Propone corrección de errores | Selecciona código con error → `/fix` |
| `/explain` | Explica código seleccionado | Selecciona código → `/explain` |

---

## 🛠️ Pre-requisitos

### Software Necesario

```bash
# Verificar instalaciones
python3 --version   # Debe ser 3.10 o superior
code --version      # Visual Studio Code
git --version       # Git
pip --version       # Gestor de paquetes de Python
```

> 📝 **NOTA:** Este taller usa datos en memoria (diccionarios de Python) para no requerir instalación de bases de datos. Los datos se pierden al reiniciar la aplicación, pero se cargan datos de ejemplo automáticamente al iniciar.

### Extensiones de VS Code

1. **GitHub Copilot** — Extensión principal
2. **GitHub Copilot Chat** — Chat integrado
3. **Python** (Microsoft) — Soporte para Python

### Cuenta de GitHub

- Necesitas una cuenta con acceso a **GitHub Copilot**
- Puede ser licencia individual, de organización o educativa

---

## 📅 Agenda del Workshop

| Hora | Bloque | Actividad | Modo Copilot |
|------|--------|-----------|--------------|
| 0:00 - 0:15 | Bienvenida | Setup, configuración e introducción | - |
| 0:15 - 0:55 | Ejercicio 1 | API REST con Flask + Swagger | Ask → Agent |
| 0:55 - 1:00 | ☕ Break | Descanso y Q&A rápido | - |
| 1:00 - 1:30 | Ejercicio 2 | Frontend HTML + integración con API | Agent |
| 1:30 - 1:50 | Ejercicio 3 | Pruebas unitarias y refactoring | Agent + /tests |
| 1:50 - 2:00 | Cierre | Recap, tips avanzados y recursos | - |

> ⏱️ Los tiempos son aproximados. Ajusta según el ritmo del grupo, pero **nunca excedas 2 horas**.

> 🎓 **Nota para el instructor:** Si la configuración inicial (Setup) toma más de 15 minutos por problemas de instalación, compensa reduciendo el Ejercicio 3: limítalo a solo generar tests con `/tests` (Pasos 3.1–3.3) y omite el refactoring y `/doc` (Pasos 3.4–3.5). Lo más importante es que los participantes completen los Ejercicios 1 y 2.

---

## 🔬 Ejercicio 1: API REST con Copilot (35-40 min)

### Objetivos

- ✅ Configurar instrucciones de Copilot para el proyecto
- ✅ Crear la estructura del proyecto Python
- ✅ Implementar una API REST con Flask-RESTX
- ✅ Obtener documentación Swagger automática
- ✅ Experimentar con el autocompletado de Copilot

### Paso 1.1: Explorar con Modo Ask 🔍

> 💡 **IMPORTANTE:** Asegúrate de estar en **Modo Ask** (ícono de mensaje 💬). Este modo NO modifica archivos, solo responde preguntas.

📍 **Cómo activar Modo Ask:**
1. Abre Copilot Chat (`Ctrl+Shift+I` / `Cmd+Shift+I`)
2. Busca el selector de modo en la parte superior
3. Selecciona **"Ask"** o el ícono de mensaje

🤖 **PROMPT — Copia y pega en Copilot Chat:**

```
Soy desarrollador en Contoso Banco y necesito diseñar un sistema bancario simple.

Ayúdame a entender:

1. ¿Qué entidades necesitaría para un sistema que gestione:
   - Clientes (nombre, email, teléfono, dirección)
   - Cuentas bancarias (tipo: ahorro/corriente, saldo, estado)
   - Transacciones (depósitos, retiros, transferencias)

2. ¿Qué endpoints REST serían necesarios para un CRUD básico?

3. ¿Cómo organizar esto usando Flask y Flask-RESTX en Python?

4. ¿Cómo se integra Swagger automáticamente con Flask-RESTX?
```

📝 **Observa:** Copilot responde con información detallada pero **NO crea ningún archivo**. Esto es ideal para la fase de exploración y planificación.

> 🌟 **Momento wow:** Observa cómo Copilot comprende el dominio bancario y sugiere una arquitectura coherente sin que le des detalles técnicos excesivos.

---

### Paso 1.2: Crear instrucciones de Copilot para el proyecto

> 💡 **¿Por qué ahora?** El archivo `copilot-instructions.md` configura a Copilot para que siga los estándares del proyecto en **todos** los archivos que genere de aquí en adelante. Crearlo antes de escribir código asegura que los modelos, la API y el frontend se generen con las convenciones correctas desde el inicio.

> 💡 **IMPORTANTE:** Cambia a **Modo Agent** (ícono de robot 🤖). Este modo **PUEDE** crear y modificar archivos.

📍 **Cómo activar Modo Agent:**
1. En Copilot Chat, busca el selector de modo
2. Selecciona **"Agent"** o el ícono de robot/chispa

🤖 **PROMPT en Modo Agent:**

```
Crea el archivo .github/copilot-instructions.md con instrucciones para que Copilot actúe como experto en Python para Contoso Banco:

# Instrucciones para GitHub Copilot - Proyecto Contoso Banco

## Idioma
- Todo el código, comentarios y documentación debe estar en **español**
- Mensajes de error en español
- Nombres de variables y funciones en español (excepto palabras técnicas estándar como get, post, api)

## Estándares de Código
| Aspecto | Estándar |
|---------|----------|
| Tecnología | Python 3.10+ |
| Framework | Flask + Flask-RESTX |
| Estilo | PEP 8 |
| Documentación | Docstrings en español |

## Nomenclatura
- Funciones y variables: snake_case en español (obtener_clientes, crear_cuenta)
- Clases: PascalCase (ClienteModelo, CuentaServicio)
- Constantes: UPPER_SNAKE_CASE

## Contexto del Proyecto
Este es un sistema bancario para Contoso Banco que gestiona clientes, cuentas y transacciones. Usa datos en memoria (diccionarios Python) sin base de datos externa.
```

---

### Paso 1.3: Crear estructura del proyecto e instalar dependencias

🤖 **PROMPT en Modo Agent:**

```
Crea la estructura inicial del proyecto Contoso Banco con sus dependencias:

1. Estructura de carpetas:
   contoso-banco/
   ├── app.py
   ├── requirements.txt (flask, flask-restx, pytest)
   ├── modelos/
   │   └── __init__.py
   ├── static/
   ├── templates/
   └── tests/
       └── __init__.py

2. Instala las dependencias con: pip install -r requirements.txt
```

📝 **Alternativa manual** (si el agente no ejecuta):
```bash
mkdir -p contoso-banco/modelos contoso-banco/static contoso-banco/templates contoso-banco/tests
touch contoso-banco/app.py contoso-banco/requirements.txt
touch contoso-banco/modelos/__init__.py contoso-banco/tests/__init__.py

# Crear requirements.txt
echo -e "flask\nflask-restx\npytest" > contoso-banco/requirements.txt

# Instalar dependencias
cd contoso-banco
pip install -r requirements.txt
```

> 📝 **Nota:** Los archivos `__init__.py` son necesarios para que Python reconozca `modelos/` y `tests/` como paquetes importables. Sin ellos, los imports como `from modelos.cliente import ...` fallarán.

---

### Paso 1.4: Crear los modelos de datos con Copilot

Ahora vamos a ver cómo Copilot nos ayuda a escribir código a partir de **comentarios descriptivos**. En lugar de pedirle el código exacto, le daremos contexto e intención.

🤖 **PROMPT en Modo Agent:**

```
Crea el archivo contoso-banco/modelos/cliente.py con el modelo de datos para los clientes de Contoso Banco.

El modelo debe incluir:
- Un diccionario en memoria como base de datos simulada
- Datos de ejemplo precargados (3 clientes ficticios con nombres en español)
- Campos: id, nombre, email, telefono, direccion
- Funciones para: obtener todos, obtener por id, crear, actualizar, eliminar
- Cada función debe tener un comentario en español que describa su propósito
```

📝 **¿La sugerencia es útil?** Observa el código generado:
- ¿Copilot usó nombres en español? 
- ¿Los datos de ejemplo son coherentes con un banco?
- ¿Necesitas ajustar algún campo?

> 💡 **Tip:** Si Copilot genera el código en inglés, prueba agregar al prompt: *"Recuerda seguir las instrucciones de .github/copilot-instructions.md"*. Esto refuerza las convenciones que configuraste en el Paso 1.2.

---

### Paso 1.5: Crear el modelo de cuentas bancarias

Ahora usaremos Copilot para generar el modelo de cuentas, siguiendo el mismo patrón que usamos con clientes.

🤖 **PROMPT en Modo Agent:**

```
Crea el archivo contoso-banco/modelos/cuenta.py para las cuentas bancarias de Contoso Banco.

Necesito un modelo similar a modelos/cliente.py pero para cuentas bancarias con:
- Campos: id, cliente_id, numero_cuenta, tipo_cuenta (ahorro/corriente), saldo, estado (activa/inactiva/bloqueada), fecha_apertura
- Datos de ejemplo que coincidan con los clientes existentes
- Validación: el saldo no puede ser negativo

Sigue el mismo patrón y estilo que modelos/cliente.py
```

📝 **Observa:**
- ¿Copilot detectó el patrón de `cliente.py` y generó código consistente?
- ¿Los datos de ejemplo usan `cliente_id` que coinciden con los clientes existentes?
- ¿Incluyó la validación de saldo negativo que pediste?

> 🌟 **Momento wow:** Al mencionar "sigue el mismo patrón que modelos/cliente.py", Copilot analiza el archivo existente y replica su estructura. ¡Cada persona puede obtener un resultado ligeramente diferente!

---

### Paso 1.6: Crear la aplicación Flask con Swagger

Ahora le pediremos a Copilot que genere la aplicación principal. Observa cómo con un prompt conciso y orientado a la intención, Copilot puede generar una app completa.

🤖 **PROMPT en Modo Agent:**

```
Crea contoso-banco/app.py como la aplicación principal de Contoso Banco.

Configura Flask con Flask-RESTX para tener Swagger UI automático.
Título: "API de Contoso Banco", versión 1.0.
Incluye dos namespaces con endpoints CRUD completos:
  - /api/clientes (usando modelos/cliente.py)
  - /api/cuentas (usando modelos/cuenta.py)
Agrega también una ruta "/" que renderice templates/index.html para servir el frontend.
Incluye decoradores @ns.doc() y modelos de Swagger con api.model() para la documentación.
```

> 💡 **Si la sugerencia no incluye algo que necesitas** (por ejemplo, falta la documentación Swagger o la ruta del frontend), prueba un prompt de seguimiento como: *"Agrega modelos de Swagger para documentar los esquemas de request/response de cada endpoint"* o *"Agrega la ruta / para servir el frontend con render_template"*. Iterar es parte natural de trabajar con Copilot.

---

### Paso 1.7: Ejecutar y explorar Swagger

🤖 **PROMPT en Modo Agent:**

```
Ejecuta la aplicación Flask de Contoso Banco
```

📝 **Alternativa manual:**
```bash
cd contoso-banco
python app.py
```

**Abre en el navegador:** `http://127.0.0.1:5000/api/doc`

✅ **Verificar:**
- Swagger UI se muestra con el título "API de Contoso Banco"
- Los endpoints de clientes y cuentas aparecen organizados por namespace
- Puedes probar los endpoints directamente desde Swagger (botón "Try it out")
- GET `/api/clientes` retorna los clientes de ejemplo

> 🌟 **Momento wow:** ¡Con Flask-RESTX, Swagger UI se genera **automáticamente** sin necesidad de configuración adicional! Prueba hacer un POST desde Swagger para crear un nuevo cliente.

---

### Paso 1.8: Agregar endpoint de transacciones (⭐ desafío bonus)

> 📝 **Este paso es OPCIONAL.** Es un desafío para quienes terminaron los pasos anteriores antes de tiempo. Si el grupo va justo de tiempo, el instructor puede indicar que lo salten y pasen directamente al Ejercicio 2.

Este paso es un **mini-desafío**. Usa lo que aprendiste para crear la funcionalidad de transacciones con la ayuda de Copilot.

🤖 **PROMPT sugerido (adáptalo a tu estilo):**

```
@workspace Basándote en los patrones existentes del proyecto, crea la funcionalidad de transacciones bancarias:

1. Modelo en modelos/transaccion.py con:
   - Campos: id, cuenta_origen_id, cuenta_destino_id, tipo (deposito/retiro/transferencia), monto, fecha, descripcion
   - Datos de ejemplo precargados
   - Validación: el monto debe ser positivo

2. Registra los endpoints de transacciones en app.py con un namespace /api/transacciones
3. Incluye documentación Swagger para cada endpoint
```

> 💡 **Observa:** Al usar `@workspace`, Copilot analiza los archivos existentes y genera código que **sigue los mismos patrones** que ya usaste en clientes y cuentas.

---

### 🛠️ Troubleshooting Ejercicio 1

| Problema | Solución |
|----------|----------|
| `ModuleNotFoundError: flask_restx` | Ejecuta `pip install -r requirements.txt` |
| `ModuleNotFoundError` al importar modelos | Verifica que `modelos/__init__.py` exista (se creó en el Paso 1.3) |
| Swagger no aparece | Verifica que `Api()` esté inicializado en `app.py` y abre `/api/doc` |
| Puerto en uso | Cambia el puerto: `app.run(port=5001)` |
| Copilot genera código en inglés | Refuerza con "Sigue las instrucciones de .github/copilot-instructions.md" |

---

## 🔬 Ejercicio 2: Frontend e Integración (25-30 min)

> ⚠️ **PRERREQUISITO:** Este ejercicio requiere que la API del Ejercicio 1 esté funcionando.

> 📝 **ENFOQUE SIMPLE:** El frontend es **una sola página HTML** (`templates/index.html`) servida por Flask con `render_template`. Usa Bootstrap 5 vía CDN y JavaScript vanilla con `fetch()` para llamar a la API. **No se usa React, npm ni ningún framework frontend** — todo está en un solo archivo HTML.

### Objetivos

- ✅ Crear una página HTML servida por Flask que consuma la API
- ✅ Usar Copilot para generar código JavaScript vanilla con `fetch()`
- ✅ Integrar Bootstrap 5 vía CDN para una interfaz visual atractiva
- ✅ Practicar el uso de `/explain` para entender código generado

### Paso 2.1: Crear la página HTML del frontend

> 💡 **NOTA:** Todo el frontend vive en un solo archivo `templates/index.html`. Flask lo sirve con `render_template` (la ruta `/` ya debería estar configurada en `app.py` desde el Paso 1.6). El JavaScript vanilla dentro del HTML usa `fetch()` para comunicarse con la API.

🤖 **PROMPT en Modo Agent:**

```
Crea el archivo contoso-banco/templates/index.html con la página principal del sistema de Contoso Banco.

Es una sola página HTML con JavaScript vanilla embebido (sin frameworks como React o Vue).

Requisitos:
1. Usar Bootstrap 5 via CDN (no instalar localmente, no usar npm)
2. Header con el nombre "Contoso Banco" y un ícono de banco (emoji o icono Bootstrap)
3. Barra de navegación con tabs: Inicio, Clientes, Cuentas
4. Sección de Inicio con:
   - Tarjetas (cards) mostrando estadísticas: Total Clientes, Total Cuentas
   - Las estadísticas se cargan dinámicamente desde la API con fetch()
5. Sección de Clientes con:
   - Tabla con los datos de clientes (se carga desde GET /api/clientes)
   - Botón "Nuevo Cliente" que muestra un modal con formulario
   - Botones de editar y eliminar en cada fila
6. Footer con "© Contoso Banco - Sistema de Gestión Bancaria"

Todo el JavaScript va dentro de una etiqueta <script> al final del HTML.
Usa colores profesionales bancarios (azul oscuro #1a237e, blanco, gris claro).
El JavaScript debe usar fetch() para consumir la API en /api/ (misma URL base).
Incluye manejo de errores con mensajes amigables al usuario.
```

> 📝 **Nota:** Observa que la barra de navegación solo incluye **Inicio, Clientes y Cuentas**. Si completaste el desafío bonus de transacciones (Paso 1.8), puedes agregar un tab de Transacciones más adelante.

---

### Paso 2.2: Agregar la sección de Cuentas al HTML

🤖 **PROMPT en Modo Agent:**

```
@workspace Actualiza templates/index.html para agregar la sección de Cuentas bancarias.

Agrega dentro del mismo archivo HTML:
1. Tabla con las cuentas (número, tipo, saldo, estado, cliente asociado)
2. Los saldos deben mostrarse en formato de moneda ($ con separadores de miles)
3. Botón "Nueva Cuenta" con modal que incluya:
   - Selector de cliente (dropdown cargado desde la API con fetch)
   - Tipo de cuenta (ahorro/corriente)
   - Saldo inicial
4. Badge de color para el estado: activa (verde), inactiva (amarillo), bloqueada (rojo)
5. Funcionalidad de eliminar cuenta con confirmación

Usa el mismo patrón de JavaScript vanilla con fetch() que ya existe en la sección de Clientes.
```

> 💡 **Observa:** Al usar `@workspace` y mencionar "el patrón que ya existe", Copilot genera código **consistente** con lo que ya escribiste, todo dentro del mismo archivo HTML.

---

### Paso 2.3: Verificar la ruta del frontend en Flask

> 📝 **Este paso puede ser innecesario** si en el Paso 1.6 Copilot ya incluyó la ruta `"/"` con `render_template('index.html')` en `app.py`. Verifica rápidamente:

🤖 **PROMPT en Modo Ask:**

```
@workspace ¿El archivo app.py ya tiene configurada una ruta "/" que sirva templates/index.html?
```

Si la respuesta es **sí**, pasa directo al Paso 2.4. Si la respuesta es **no**:

🤖 **PROMPT en Modo Agent:**

```
@workspace Actualiza app.py para agregar una ruta "/" que renderice templates/index.html con render_template, sin modificar los endpoints de la API existentes.
```

---

### Paso 2.4: Ejecutar y probar la integración

🤖 **PROMPT en Modo Agent:**

```
Ejecuta la aplicación de Contoso Banco
```

📝 **Alternativa manual:**
```bash
cd contoso-banco
python app.py
```

**Abre en el navegador:**
- Frontend (página HTML): `http://127.0.0.1:5000/`
- Swagger UI (documentación API): `http://127.0.0.1:5000/api/doc` (o la ruta configurada en Flask-RESTX)

✅ **Verificar:**
- La página HTML de Contoso Banco carga correctamente en `/`
- Las estadísticas se muestran con datos reales de la API
- La tabla de clientes muestra los datos de ejemplo
- Se puede crear un nuevo cliente desde el formulario modal
- La sección de cuentas funciona con los datos de la API
- Swagger UI sigue accesible en `/api/doc`

---

### Paso 2.5: Usar /explain para entender código (demostración)

> 💡 **CONCEPTO:** El comando `/explain` es perfecto para entender código que Copilot generó.

📍 **Instrucciones:**
1. Selecciona el bloque de JavaScript vanilla (dentro del `<script>` en el HTML) que hace `fetch()` a la API
2. Abre Copilot Chat y escribe:

```
/explain ¿Qué hace este código paso a paso? ¿Hay algún problema potencial?
```

📝 **Observa:** Copilot explica el flujo del código y puede señalar posibles mejoras como manejo de errores, timeouts o validaciones faltantes.

---

### 🛠️ Troubleshooting Ejercicio 2

| Problema | Solución |
|----------|----------|
| Página en blanco en `/` | Verifica que `app.py` tenga la ruta `/` con `render_template('index.html')` (Paso 2.3) |
| Fetch falla con "Network Error" | Confirma que la API esté corriendo en el puerto correcto |
| Bootstrap no carga | Verifica tu conexión a internet (se usa CDN) |
| Modal no se abre | Verifica que el JS de Bootstrap CDN esté incluido en el HTML |
| Datos no se muestran en la tabla | Abre la consola del navegador (F12) para ver errores |

---

## 🔬 Ejercicio 3: Tests y Refactoring (20-25 min)

> ⚠️ **PRERREQUISITO:** Este ejercicio requiere haber completado el Ejercicio 1 con la API funcionando.

> 🎓 **Nota para el instructor:** Si el tiempo es limitado, prioriza los Pasos 3.1–3.3 (generar y ejecutar tests). Los Pasos 3.4–3.5 (refactoring y documentación) son valiosos pero pueden omitirse sin afectar la experiencia central del workshop.

### Objetivos

- ✅ Generar pruebas unitarias automáticamente con `/tests`
- ✅ Usar Copilot para escribir tests de integración de la API
- ✅ Practicar refactoring asistido con `/fix`
- ✅ Generar documentación con `/doc`

### Paso 3.1: Generar pruebas unitarias con /tests

> 💡 **COMANDO ESPECIAL:** El comando `/tests` genera automáticamente pruebas unitarias para el código seleccionado.

📍 **Cómo usar /tests:**
1. Abre el archivo `contoso-banco/modelos/cliente.py`
2. Selecciona todo el contenido del archivo (`Ctrl+A` / `Cmd+A`)
3. Abre Copilot Chat y escribe:

🤖 **PROMPT:**

```
/tests Genera pruebas unitarias completas con pytest para este módulo.

Quiero pruebas que cubran:

1. Obtener todos los clientes (verifica que retorne la lista completa)
2. Obtener un cliente por ID válido e inválido
3. Crear un cliente con datos válidos
4. Crear un cliente con datos incompletos (falta email, falta nombre)
5. Actualizar un cliente existente
6. Actualizar un cliente que no existe
7. Eliminar un cliente existente
8. Eliminar un cliente que no existe

Requisitos:
- Usar pytest con nombres descriptivos en español
- Patrón Arrange-Act-Assert con comentarios
- Cada test debe ser independiente (no depender de otros tests)
- Incluir un fixture que reinicie los datos antes de cada prueba
```

> 🌟 **Momento wow:** Copilot genera un suite completo de tests incluyendo casos positivos y negativos, ¡a partir del código que tú escribiste!

---

### Paso 3.2: Crear archivo de pruebas

🤖 **PROMPT en Modo Agent:**

```
Guarda las pruebas generadas en contoso-banco/tests/test_cliente.py

Asegúrate de que:
1. Los imports sean correctos para la estructura del proyecto
2. Incluye un fixture de pytest que reinicie los datos del diccionario en memoria antes de cada prueba
```

> 📝 **Nota:** El archivo `tests/__init__.py` ya se creó en el Paso 1.3, así que los imports deberían funcionar directamente.

---

### Paso 3.3: Generar pruebas de integración para la API

🤖 **PROMPT en Modo Agent:**

```
@workspace Crea pruebas de integración para la API de Contoso Banco en contoso-banco/tests/test_api.py

Usando el test client de Flask, verifica:

1. GET /api/clientes retorna 200 y una lista de clientes
2. GET /api/clientes/1 retorna 200 y el cliente correcto
3. GET /api/clientes/999 retorna 404
4. POST /api/clientes con datos válidos retorna 201
5. POST /api/clientes con datos faltantes retorna 400
6. PUT /api/clientes/1 actualiza correctamente
7. DELETE /api/clientes/1 retorna 200 o 204
8. GET /api/cuentas retorna 200

Usa el test client de Flask:
- app.test_client() para hacer las peticiones
- json=data para enviar datos JSON
- response.get_json() para leer la respuesta
```

---

### Paso 3.4: Ejecutar las pruebas

🤖 **PROMPT en Modo Agent:**

```
Ejecuta todas las pruebas del proyecto Contoso Banco y muéstrame los resultados
```

📝 **Alternativa manual:**
```bash
cd contoso-banco
python -m pytest tests/ -v
```

✅ **Verificar:**
- Todas las pruebas pasan (verde)
- Los tests unitarios y de integración se ejecutan correctamente
- No hay errores de importación

---

### Paso 3.5: Refactoring con /fix y /explain *(si hay tiempo)*

> 💡 **CONCEPTO:** Ahora usaremos Copilot para **mejorar** el código existente.

📍 **Ejercicio de refactoring:**

1. Abre `contoso-banco/app.py`
2. Selecciona un bloque de endpoints
3. Usa el siguiente prompt en Copilot Chat:

🤖 **PROMPT:**

```
/explain Analiza este código y dime:
1. ¿Hay código duplicado que se pueda simplificar?
2. ¿Faltan validaciones importantes?
3. ¿Hay algún problema de seguridad potencial?
4. ¿Cómo mejorarías el manejo de errores?
```

Después, si Copilot sugiere mejoras:

```
/fix Aplica las mejoras de seguridad y validación que acabas de sugerir
```

> 🌟 **Momento wow:** Copilot puede detectar **problemas de seguridad** como falta de validación de inputs, inyección potencial o manejo inadecuado de errores, y proponer correcciones específicas.

---

### Paso 3.6: Generar documentación con /doc *(si hay tiempo)*

📍 **Instrucciones:**
1. Abre `contoso-banco/modelos/cuenta.py`
2. Selecciona una función que no tenga docstring
3. Usa el comando `/doc`:

🤖 **PROMPT:**

```
/doc Genera docstrings completos en español para estas funciones.

Incluye:
- Descripción clara de qué hace cada función
- Parámetros con su tipo y descripción
- Valor de retorno
- Excepciones que puede lanzar
- Un ejemplo de uso
```

---

### 🛠️ Troubleshooting Ejercicio 3

| Problema | Solución |
|----------|----------|
| `ModuleNotFoundError` en tests | Verifica que `__init__.py` exista en `modelos/` y `tests/` (creados en Paso 1.3) |
| Tests fallan por datos compartidos | Usa un fixture que reinicie el diccionario en memoria |
| pytest no encuentra los tests | Ejecuta desde la raíz del proyecto: `python -m pytest tests/ -v` |
| `/tests` genera código incompleto | Intenta seleccionar menos código o ser más específico en el prompt |
| `/fix` no aplica cambios | Cambia a Modo Agent para que Copilot pueda modificar archivos |

---

## 📖 Referencia Rápida

### Comandos de Copilot Chat

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `@workspace` | Contexto del proyecto entero | `@workspace ¿cómo se implementa la ruta de clientes?` |
| `/tests` | Generar pruebas unitarias | Selecciona código → `/tests` |
| `/doc` | Generar documentación | Selecciona función → `/doc` |
| `/fix` | Proponer corrección | Selecciona código con error → `/fix` |
| `/explain` | Explicar código | Selecciona código → `/explain` |

### Atajos de Teclado

| Atajo | Acción |
|-------|--------|
| `Ctrl+I` / `Cmd+I` | Abrir Copilot inline en el editor |
| `Ctrl+Shift+I` / `Cmd+Shift+I` | Abrir panel de Copilot Chat |
| `Tab` | Aceptar sugerencia de Copilot |
| `Esc` | Rechazar sugerencia |
| `Alt+]` | Ver siguiente sugerencia |
| `Alt+[` | Ver sugerencia anterior |

### Modos de Copilot

| Modo | Cuándo usarlo |
|------|---------------|
| Ask 💬 | Explorar, entender, planificar (no modifica archivos) |
| Agent 🤖 | Implementar, crear archivos, hacer cambios |
| Plan 📋 | Tareas complejas multi-archivo (con aprobación) |

### Tips para Mejores Resultados con Copilot

| Tip | Ejemplo |
|-----|---------|
| **Nombres descriptivos** | `obtener_saldo_cuenta()` en vez de `get_data()` |
| **Comentarios con intención** | `# Validar que el monto sea positivo antes de procesar` |
| **Contexto del negocio** | Mencionar "Contoso Banco", "cuenta bancaria", "transacción" |
| **Iterar sobre sugerencias** | Si la primera sugerencia no es perfecta, ajusta tu comentario |
| **Usar @workspace** | Para que Copilot entienda tu estructura de proyecto |
| **Referenciar archivos** | "Sigue el patrón de cliente.py" para generar código consistente |

---

## 🆘 ¿Te quedaste atrás?

No te preocupes — el valor del workshop está en **experimentar con Copilot**, no en terminar todo el código.

| Situación | Qué hacer |
|-----------|-----------|
| No terminé el Ejercicio 1 | Pide al instructor la solución de referencia para poder continuar con el Ejercicio 2 |
| No terminé el Ejercicio 2 | El Ejercicio 3 solo necesita la API (Ejercicio 1). Puedes hacer tests sin frontend |
| No terminé el desafío de transacciones | ¡Es bonus! No afecta el resto del workshop |
| Copilot genera algo diferente a mi vecino | Eso es **normal y esperado**. Comparen resultados — es un buen ejercicio de aprendizaje |

> 🎓 **Para el instructor:** Se recomienda tener una rama `solucion` en el repositorio del workshop con el código completo de referencia. Esto permite que participantes que se queden atrás puedan alcanzar al grupo descargando los archivos necesarios.

---

## ✅ Checklist Final del Workshop

Al terminar el workshop, deberías tener:

### Configuración de Copilot
- [ ] `.github/copilot-instructions.md` — Instrucciones del proyecto

### Proyecto Backend
- [ ] `contoso-banco/app.py` — Aplicación Flask con Swagger y ruta frontend
- [ ] `contoso-banco/requirements.txt` — Dependencias del proyecto
- [ ] `contoso-banco/modelos/__init__.py` — Paquete de modelos
- [ ] `contoso-banco/modelos/cliente.py` — Modelo de clientes
- [ ] `contoso-banco/modelos/cuenta.py` — Modelo de cuentas
- [ ] `contoso-banco/modelos/transaccion.py` — Modelo de transacciones *(⭐ bonus)*

### Frontend (página HTML servida por Flask)
- [ ] `contoso-banco/templates/index.html` — Página HTML con Bootstrap 5 CDN y JavaScript vanilla
- [ ] Tabla de clientes funcional con CRUD (usando `fetch()`)
- [ ] Sección de cuentas con badges de estado
- [ ] Estadísticas dinámicas en la página de inicio

### Swagger
- [ ] Swagger UI accesible y funcional en `/api/doc`
- [ ] Endpoints documentados con modelos de request/response
- [ ] Posibilidad de probar endpoints desde Swagger ("Try it out")

### Pruebas
- [ ] `contoso-banco/tests/__init__.py` — Paquete de tests
- [ ] `contoso-banco/tests/test_cliente.py` — Pruebas unitarias
- [ ] `contoso-banco/tests/test_api.py` — Pruebas de integración
- [ ] Todas las pruebas pasan con `pytest`

---

## 📚 Recursos Adicionales

### Documentación Oficial

- [GitHub Copilot Docs](https://docs.github.com/en/copilot)
- [VS Code + Copilot](https://code.visualstudio.com/docs/copilot/overview)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [Flask-RESTX Documentation](https://flask-restx.readthedocs.io/)
- [Swagger UI](https://swagger.io/tools/swagger-ui/)
- [pytest Documentation](https://docs.pytest.org/)
- [Bootstrap 5](https://getbootstrap.com/docs/5.3/)

### Patrones y Buenas Prácticas

- [PEP 8 — Style Guide for Python](https://peps.python.org/pep-0008/)
- [Flask REST API Best Practices](https://flask.palletsprojects.com/patterns/rest/)

### Siguiente Nivel con Copilot

- **Copilot en la terminal:** Usa `Ctrl+I` en la terminal integrada de VS Code para generar comandos
- **Agentes personalizados:** Crea archivos `.github/agents/*.md` para especializar a Copilot
- **Copilot para Git:** Usa Copilot Chat para generar mensajes de commit descriptivos

---

## 🙋 Preguntas Frecuentes

### ¿Por qué usamos datos en memoria en lugar de una base de datos?

| Ventaja | Detalle |
|---------|---------|
| Sin instalación | No requiere PostgreSQL, MySQL ni ningún motor de BD |
| Rápido | Operaciones instantáneas, ideal para un workshop |
| Portable | Funciona igual en cualquier máquina |
| Simple | Permite enfocarnos en Copilot, no en configuración de BD |

**¿Cómo migrar a una base de datos real?**
1. Instalar `flask-sqlalchemy` y un driver de BD
2. Reemplazar los diccionarios por modelos SQLAlchemy
3. Copilot puede ayudarte: *"@workspace migra los modelos en memoria a SQLAlchemy con SQLite"*

### ¿Copilot genera código diferente para cada persona?

Sí, y eso es **intencional**. Copilot aprende del contexto (tu código, tus comentarios, tu estilo) y puede generar soluciones ligeramente diferentes para cada persona. Esto es una característica, no un error.

### ¿Cómo hago que Copilot genere código en español?

1. Configúralo en `.github/copilot-instructions.md` (Paso 1.2 — aplica para todo el proyecto)
2. Escribe comentarios y nombres de variables en español
3. Si aún genera en inglés, agrega *"Sigue las instrucciones de .github/copilot-instructions.md"* al prompt

### ¿Qué hago si Copilot sugiere código incorrecto?

1. **No aceptes ciegamente** — siempre revisa las sugerencias
2. Presiona `Esc` para rechazar y ajusta tu comentario/prompt
3. Usa `Alt+]` para ver sugerencias alternativas
4. Recuerda: Copilot es un **asistente**, no un reemplazo del desarrollador

### ¿Qué pasa si mi interfaz de Copilot se ve diferente?

La interfaz de GitHub Copilot (modos, íconos, selectores) se actualiza con frecuencia. Si los íconos o la ubicación de los modos no coinciden exactamente con lo descrito en este workshop, consulta la [documentación oficial de VS Code + Copilot](https://code.visualstudio.com/docs/copilot/overview) o pregunta al instructor.

---

## 👥 Créditos

**Workshop desarrollado para:** Demostración de GitHub Copilot

**Tecnologías:** GitHub Copilot, Python 3, Flask, Flask-RESTX, Swagger UI, HTML + JavaScript vanilla, Bootstrap 5 (CDN), pytest

**Duración:** 2 horas

**Escenario ficticio:** Contoso Banco (institución financiera ficticia para fines educativos)

---

> 🎉 **¡Gracias por participar!** Ahora tienes las herramientas para usar GitHub Copilot como tu par de programación en proyectos reales.
