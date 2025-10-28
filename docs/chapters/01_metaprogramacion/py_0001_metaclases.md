```markdown
# PY-0001: Inyección de Atributos con Metaclases

| ID | Título | Dificultad | Tópico Principal |
| :--- | :--- | :--- | :--- |
| PY-0001 | Inyección de Atributos con Metaclases | Avanzada | Metaclases y la Construcción de Clases |

---

## 1. Código Fuente

```python
class Meta(type):
    def __new__(cls, name, bases, dct):
        # Modificación del diccionario de la clase antes de su creación
        dct['id'] = 100
        # Llamada al constructor de la metaclase base (type) para crear la clase A
        return super().__new__(cls, name, bases, dct)

class A(metaclass=Meta):
    pass

print(A.id)
```

## 2. Salida Esperada

```
100
```

---

## 3. Análisis Detallado

### ¿Qué estamos viendo aquí? (Concepto Central)

Este fragmento demuestra el concepto de **Metaclases**, que son las "fábricas" de las clases en Python. Al usar `metaclass=Meta`, indicamos que la clase `Meta` debe intervenir y manipular el proceso de creación de la clase `A`. El tema central a aprender es cómo la metaprogramación permite modificar el comportamiento estructural de las clases en tiempo de definición.

### Elementos Clave del Código

*   `class Meta(type):`: Define la metaclase al heredar de `type`, la metaclase por defecto de Python.
*   `metaclass=Meta`: Argumento que enlaza la clase `A` con su metaclase constructora, `Meta`.
*   `__new__(cls, name, bases, dct)`: El método de la metaclase que es llamado antes de que la clase (objeto) sea finalizada.
    *   `dct`: Es el diccionario que recoge todos los atributos y métodos definidos en el cuerpo de la clase `A`.
*   `dct['id'] = 100`: **El mecanismo de inyección**. Esta línea añade un atributo de clase llamado `id` con el valor `100` directamente al diccionario de la clase, volviéndose un atributo de `A`.

### ¿Qué hace este código? (Mecanismo de Ejecución)

1.  Python detecta el argumento `metaclass=Meta` al iniciar la definición de la clase `A`.
2.  En lugar de crear `A` directamente, llama a `Meta.__new__`, pasándole el nombre de la clase, las bases y el diccionario de atributos (que inicialmente está vacío para este ejemplo).
3.  `Meta.__new__` modifica este diccionario (`dct`) agregando el par `'id': 100`.
4.  Luego, llama a la versión superior (`super().__new__`) para que `type` complete la creación de la clase `A`, utilizando el diccionario ya modificado.
5.  Como resultado, la clase `A` es creada con el atributo `id=100`, el cual se imprime en la última línea.

---

## 4. Aplicación Práctica e Ingeniería

### Aplicación Práctica (Uso Común)

Las metaclases son herramientas esenciales en el desarrollo de *frameworks* de alto nivel, permitiendo la creación de **APIs declarativas** y la estandarización de código sin la intervención del desarrollador final.

*   **Sistemas ORM (Object-Relational Mapping):** Se utilizan para leer la definición de un modelo de datos (ej. `class Usuario: nombre = Field()`) e inyectar automáticamente métodos de persistencia (`.save()`, `.delete()`) y atributos de mapeo de base de datos.
*   **Patrones de Diseño (Singleton, *Plugins*):** Permiten forzar patrones a nivel de la estructura de la clase, como asegurar que solo se pueda crear una instancia (Singleton) o registrar automáticamente clases en un sistema de *plugins* al momento de su definición.

### Fundamento Teórico

Desde una perspectiva formal, este mecanismo representa la **autorreferencia estructural**. La metaclase opera en un nivel superior, tratando a la definición de la clase como su propio dato (similar a la Aritmetización de la Sintaxis en la lógica matemática). Esto se relaciona con los conceptos de **Generación Dinámica de Código** y **Abstracción de Nivel Superior**.
```git 