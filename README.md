## 1. ¿Qué hace exactamente el bloque `let...in` en lenguaje M? ¿Por qué cada paso puede referenciar al anterior?

El bloque `let...in` es la estructura principal del lenguaje M. En `let` se escriben todos los pasos de la transformación de datos y en `in` se indica cuál será el resultado final.

Cada paso puede usar el resultado del paso anterior porque se guarda con un nombre. De esta manera, las transformaciones se realizan una después de otra hasta obtener la tabla final.

---

## 2. ¿Por qué M es Case Sensitive y qué consecuencia práctica tiene? Da un ejemplo de un error que esto puede causar.

Lenguaje M distingue entre mayúsculas y minúsculas, por eso se dice que es **Case Sensitive**.

Si una función o un nombre está escrito con una letra diferente, Power BI mostrará un error.

Por ejemplo:

```powerquery id="xqks6m"
Text.Trim
```

es correcto, pero

```powerquery id="g0o0so"
text.trim
```

es incorrecto y genera un error porque la función debe comenzar con mayúscula.

También ocurre con los nombres de los pasos. Si el paso se llama `LimpiarEspacios` y luego escribimos `limpiarespacios`, Power BI no lo reconocerá.

---

## 3. ¿Cuál es la diferencia entre usar `Text.Trim` y `Text.Clean` en M?

`Text.Trim` elimina los espacios que están al inicio y al final de un texto.

Ejemplo:

```
" Laptop "
```

queda como:

```
"Laptop"
```

En cambio, `Text.Clean` elimina caracteres que no se pueden ver, como saltos de línea o tabulaciones.

En este ejercicio se utilizó `Text.Trim` porque el problema eran los espacios al inicio y al final de los nombres de los productos.

---

## 4. ¿Por qué filtraste los registros "PRUEBA" después de estandarizar la categoría y no antes?

Primero estandaricé la columna con `Text.Proper` para que todas las categorías tuvieran el mismo formato.

Así, valores como `"PRUEBA"`, `"prueba"` o `"Prueba"` pasan a escribirse igual: `"Prueba"`.

Después fue más fácil eliminar los registros de prueba con un solo filtro:

```powerquery id="fjb81c"
each [categoria] <> "Prueba"
```

Si hubiera filtrado antes, algunas variaciones en mayúsculas o minúsculas podrían no haberse eliminado debido a que el lenguaje M distingue entre mayúsculas y minúsculas.
