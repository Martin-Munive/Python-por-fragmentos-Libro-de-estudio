
```markdown
# PY-0001: La Diferencia Fundamental: Clases vs. Instancias

### 📋 Metadatos
**ID:** PY-0001
**Tópico Principal:** POO - Clases y Objetos
**Dificultad:** Básico

---

## 1. Código Fuente

```python
class Usuario:
    """
    La Clase: Actúa como una PLANTILLA o FÁBRICA para crear objetos.
    """
    # Atributo de CLASE: Compartido por todas las instancias.
    rol = "Visitante" 
    
    def __init__(self, nombre_param):
        """
        Método constructor que se llama al crear una INSTANCIA.
        """
        # Atributo de INSTANCIA: Único para cada objeto creado.
        self.nombre = nombre_param

# --- Ejecución de prueba ---

# 1. Creación de dos INSTANCIAS (Objetos)
usuario1 = Usuario("Alice")
usuario2 = Usuario("Bob")
usuario2.rol = "Administrador" # Modifica el atributo ROL solo para usuario2

# 2. Imprimir Atributos de INSTANCIA (Únicos)
print(f"Nombre de Alice: {usuario1.nombre}")
print(f"Nombre de Bob: {usuario2.nombre}")
print("-" * 20)

# 3. Imprimir Atributos de CLASE (Compartidos y Modificados)
print(f"Rol de Alice (sin modificar): {usuario1.rol}")
print(f"Rol de Bob (modificado): {usuario2.rol}")
print(f"Rol en la CLASE (Plantilla): {Usuario.rol}")
```

## 2. Salida Esperada

```console
Nombre de Alice: Alice
Nombre de Bob: Bob
--------------------
Rol de Alice (sin modificar): Visitante
Rol de Bob (modificado): Administrador
Rol en la CLASE (Plantilla): Visitante
```

---

## 3. Análisis Detallado (Conceptos Fundamentales)

### ¿Qué estamos viendo aquí? (Concepto Central)

Este fragmento establece el pilar de la Programación Orientada a Objetos (POO): la distinción entre una **Clase** y una **Instancia**. La **Clase (`Usuario`)** es la definición abstracta, como el plano de una casa. La **Instancia (`usuario1`, `usuario2`)** es el objeto concreto creado a partir de ese plano, como la casa física.

### Elementos Clave del Código y Conceptos Básicos

*   `class Usuario:`: Define la **Clase**. Es la plantilla que define la estructura.
*   `rol = "Visitante"`: Este es un **Atributo de Clase**. Su valor pertenece a la plantilla `Usuario` y es compartido por todas las instancias *a menos que una instancia lo sobreescriba*.
*   `def __init__(self, nombre_param):`: El **Constructor**. Este método se ejecuta *automáticamente* cada vez que se crea un nuevo objeto (`Instancia`) de la clase.
    *   `self`: Es una referencia a la **Instancia** que se está creando. En Python, debe ser el primer parámetro.
*   `self.nombre = nombre_param`: Este es un **Atributo de Instancia**. El valor (`"Alice"`, `"Bob"`) es único para cada objeto.

### ¿Qué hace este código? (Mecanismo de Ejecución Paso a Paso)

1.  **Línea 18 (`usuario1 = Usuario("Alice")`):** Se crea el objeto `usuario1`. El constructor `__init__` se llama, y el atributo `nombre` se establece en `"Alice"` solo para este objeto.
2.  **Línea 19 (`usuario2 = Usuario("Bob")`):** Se crea el objeto `usuario2`. Su atributo `nombre` es `"Bob"`.
3.  **Línea 20 (`usuario2.rol = "Administrador"`):** **Punto Clave.** Estamos creando un *nuevo* atributo llamado `rol` directamente en la **instancia** `usuario2`. Esto **no** modifica el `rol` de la plantilla (`Usuario.rol`) ni el de `usuario1`.
4.  **Impresiones Finales:**
    *   `usuario1.rol` muestra `"Visitante"` (el valor de la clase, que no fue modificado para `usuario1`).
    *   `usuario2.rol` muestra `"Administrador"` (el valor que se le asignó directamente a la instancia `usuario2`).
    *   `Usuario.rol` muestra `"Visitante"` (prueba que la plantilla, la clase, nunca se alteró).

---

## 4. Aplicación Práctica e Ingeniería

### Aplicación Práctica (Uso Común)

*   **Modelado de Datos (Web/APIs):** Toda aplicación de *backend* (como Django o Flask) usa clases para modelar usuarios, productos u órdenes. El atributo de clase puede ser un valor por defecto (`estatus = 'Activo'`), y los atributos de instancia son los datos únicos de la base de datos (nombre, ID, fecha de compra).
*   **Diseño de Componentes:** Permite crear una estructura base con valores predefinidos que pueden ser sobrescritos por la configuración específica de cada componente (ej. `Componente.velocidad_default = 10` y luego `mi_componente.velocidad = 5`).

### Fundamento Teórico

El concepto se relaciona con la **Teoría de Conjuntos**. La **Clase** es el conjunto que define las propiedades posibles. La **Instancia** es un elemento específico de ese conjunto. Un atributo de clase es un **parámetro** de todo el conjunto, mientras que un atributo de instancia es un **valor** único para ese elemento individual, lo que permite la herencia y la individualización de datos de manera eficiente en la memoria.

```
