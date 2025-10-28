```markdown
# PY-0001: Creación Dinámica de Atributos con Metaclases

| ID | Título | Dificultad | Tópico Principal |
| :--- | :--- | :--- | :--- |
| PY-0001 | Creación Dinámica de Atributos con Metaclases | Avanzada | Metaclases y Metaprogramación |

---

## 1. Código Fuente

```python
class TypeInjector(type):
    """
    Metaclase que intercepta la creación de clases e inyecta un atributo.
    """
    def __new__(cls, name, bases, dct):
        # El punto de control: Se añade un nuevo atributo llamado 'version' al diccionario.
        # 'version' será un atributo de la CLASE que se está creando (no de la instancia).
        dct['version'] = "v1.0.0"
        
        # Se llama a la metaclase base (type) para que finalice la creación de la clase.
        # Se le pasa el diccionario 'dct' que ahora está modificado.
        return super().__new__(cls, name, bases, dct)

class ComponenteBase(metaclass=TypeInjector):
    """
    Una clase simple que utiliza la Metaclase TypeInjector.
    """
    pass

print(ComponenteBase.version)
```

## 2. Salida Esperada

```
v1.0.0
```

---

## 3. Análisis Detallado (Conceptos Fundamentales)

### ¿Qué estamos viendo aquí? (Concepto Central)

Estamos explorando las **Metaclases**, que son la capa superior de la Programación Orientada a Objetos (POO) en Python. Si una clase normal crea *objetos*, una metaclase crea **clases**. En este fragmento, nuestra metaclase (`TypeInjector`) actúa como un **guardián de seguridad** que garantiza que toda clase que la use tenga un atributo `version` definido, incluso si el programador nunca lo escribió.

### Elementos Clave del Código y Conceptos Básicos

*   `class TypeInjector(type):`: Define la metaclase. En Python, la clase `type` es la "madre" de todas las clases. Heredar de ella permite a `TypeInjector` tomar el control del proceso de construcción de clases.
*   `metaclass=TypeInjector`: Esta palabra clave en `class ComponenteBase(...)` es la instrucción mágica. Le dice a Python: "Para construir `ComponenteBase`, llama a la metaclase `TypeInjector`."
*   `def __new__(cls, name, bases, dct):`: **El método de creación de objetos.**
    *   **¿Qué es `__new__`?** Es el primer método llamado al crear un objeto. Cuando se trata de una metaclase, `__new__` es quien crea el **objeto CLASE** (`ComponenteBase`).
    *   **Argumentos Clave:**
        *   `cls`: Representa a la metaclase en sí (aquí, `TypeInjector`).
        *   `name`: El nombre de la clase que se está creando (aquí, `'ComponenteBase'`).
        *   `bases`: Una tupla de las clases padre de las que hereda (`ComponenteBase`).
        *   `dct`: El **Diccionario de Atributos** (*namespace*). Es un mapa que contiene todos los atributos y métodos escritos directamente dentro del cuerpo de la clase (`ComponenteBase`). ¡Este es nuestro objetivo de manipulación!
*   `dct['version'] = "v1.0.0"`: La operación central. Antes de que `ComponenteBase` sea creada, estamos **inyectando** programáticamente un nuevo par clave-valor en su diccionario de atributos.
*   `super().__new__(...)`: Esta llamada es esencial. Le delega la tarea de finalizar la creación a la metaclase padre (`type`), pasándole el diccionario **modificado**. Sin esto, la clase no existiría.

### ¿Qué hace este código? (Mecanismo de Ejecución Paso a Paso)

1.  **Inicio de Creación:** Python detecta que `ComponenteBase` debe usar `TypeInjector` como metaclase.
2.  **Llamada al Constructor:** Python llama a `TypeInjector.__new__` con los detalles de `ComponenteBase`.
3.  **Inyección:** Dentro de `__new__`, el diccionario de atributos se modifica, añadiendo `version: "v1.0.0"`.
4.  **Finalización:** La llamada a `super().__new__` crea el objeto `ComponenteBase` en memoria, asegurándose de que el diccionario inyectado sea ahora su conjunto final de atributos.
5.  **Resultado:** La clase `ComponenteBase` tiene un atributo de clase `version` que nunca fue escrito en su cuerpo, pero que fue forzado por la metaclase. La impresión final accede a ese valor inyectado.

---

## 4. Aplicación Práctica e Ingeniería

### Aplicación Práctica (Contexto Original)

Este tipo de patrón se utiliza en la **Estandarización y *Scaffolding*** (andamiaje) de componentes en sistemas grandes.

*   **Registro de Versiones:** En un sistema de microservicios, se podría usar esta técnica para forzar que cada clase que representa un servicio tenga un atributo `API_ENDPOINT` o `VERSION` estándar, asegurando que todos los componentes cumplan con las reglas de trazabilidad y compatibilidad de la empresa.
*   **Patrones de Interfaz:** Se utiliza para inyectar automáticamente métodos necesarios (como un método de *logging* o un método de validación de datos) en clases base, garantizando que todos los herederos cumplan con la interfaz necesaria sin tener que escribir el código en cada uno.

### Fundamento Teórico (NEUMANN)

El proceso de metaclases es un ejemplo de un **Sistema de Tipos Dinámico Modificable**. En esencia, estamos tratando la sintaxis y la estructura del lenguaje como un dato que puede ser manipulado por el programa mismo. El `dct` (diccionario) es un **Conjunto de Características** que la metaclase transforma, operando bajo el principio de **Transformación de Mapeo**, donde una regla (la metaclase) define cómo debe ser la salida (la clase final) a partir de una entrada (la definición de la clase).
```

### **Acción Requerida (TURING):**

1.  Reemplaza el contenido completo del archivo `docs\chapters\01_metaprogramacion\py_0001_metaclases.md` con el texto de arriba.
2.  Sube los cambios a GitHub para ver la versión actualizada en el libro:

    ```bash
    git add docs/chapters/01_metaprogramacion/py_0001_metaclases.md
    git commit -m "FIX: Revisión exhaustiva y didáctica de PY-0001 (Metaclases) para nivel principiante."
    git push origin main
    ```

Una vez que revises y confirmes la mejora, procederemos a la revisión y ajuste (si es necesario) del **PY-0002** y luego la adición del **PY-0004**.