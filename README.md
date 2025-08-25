# ft_printf (42) — Implementación personalizada de printf en C
**Custom printf implementation in C supporting characters, strings, integers, unsigned, hexadecimals, pointers, and the percent sign.**

---

## 📌 Resumen
* **Lenguaje:** C (C99)
* **Funcionalidad:** Imprime en la salida estándar formateando distintos tipos de datos.
* **Objetivo:** Aprender manejo de **varargs (`stdarg.h`)**, funciones recursivas, conversión de números a cadenas y gestión de memoria dinámica.
* **Restricciones:** No se pueden usar las funciones estándar de `printf` ni `sprintf`; todo se hace mediante **write** y funciones propias.

---

## 🧠 ¿Por qué este proyecto?
Este proyecto de 42 enseña a:
* Trabajar con **argumentos variables** usando `va_list`.
* Implementar impresión de distintos **tipos de datos**: caracteres, strings, enteros, unsigned, hexadecimales y punteros.
* Gestionar memoria dinámica para conversión de enteros a cadenas.
* Desarrollar funciones auxiliares reutilizables (`ft_putnbr_fd`, `ft_putptr`, `ft_putnbr_hex`, etc.).

👉 Es fundamental para entender cómo funcionan funciones de formateo y manipulación de strings de bajo nivel en C.

---

## ⚙️ Funcionamiento de ft_printf

1. Recorre la cadena de formato.
2. Cada vez que encuentra un `%`, identifica el **tipo de dato** (`c`, `s`, `d`, `i`, `u`, `x`, `X`, `p`, `%`).
3. Llama a funciones auxiliares para imprimir el valor correspondiente:
   * `ft_putchar_fd` → imprime un carácter.
   * `ft_putstr_fd` → imprime un string.
   * `ft_itoa` → convierte enteros en cadenas.
   * `ft_putnbr_unsigned` → imprime enteros sin signo.
   * `ft_putnbr_hex` → imprime números en hexadecimal.
   * `ft_putptr` → imprime punteros con el prefijo `0x`.
4. Acumula el **número total de caracteres impresos** y lo devuelve al final.

📌 **Decisión clave:** Separar el manejo de caracteres/strings y números/punteros en funciones auxiliares permite un código limpio y modular.

---

## 🚀 Uso

### Compilación
```bash
make
```

### Ejemplo
```c
#include "ft_printf.h"

int main(void)
{
    int a = 42;
    char *str = "Hola, mundo!";
    void *ptr = &a;

    ft_printf("Entero: %d\n", a);
    ft_printf("String: %s\n", str);
    ft_printf("Carácter: %c\n", 'X');
    ft_printf("Unsigned: %u\n", 3000);
    ft_printf("Hex minúscula: %x\n", 255);
    ft_printf("Hex mayúscula: %X\n", 255);
    ft_printf("Pointer: %p\n", ptr);
    ft_printf("Porcentaje: %%\n");

    return 0;
}
```

**Salida esperada:**
```
Entero: 42
String: Hola, mundo!
Carácter: X
Unsigned: 3000
Hex minúscula: ff
Hex mayúscula: FF
Pointer: 0x7ffee3b2b5ac
Porcentaje: %
```

---

## 🗂️ Estructura del proyecto
```text
ft_printf/
├─ ft_printf.c              # Función principal y recorrido del formato
├─ aux.c                    # Manejo de argumentos variables
├─ ft_itoa.c                # Conversión de enteros a string
├─ put_functions.c          # Funciones de impresión (char, string, hex, ptr, unsigned)
├─ ft_printf.h              # Prototipos y cabeceras necesarias
├─ libft/                   # Librería personal con utilidades básicas
├─ Makefile                 # Reglas estándar (all, clean, fclean, re)
└─ README.md
```

---

## 🗂️ Explicación del código

### 📌 Manejo de argumentos
* `va_list args` → permite acceder a un número variable de argumentos.
* `aux()` → identifica el tipo de dato y llama a la función correspondiente.
* Separación clara en:
  * `handle_character_and_string()`
  * `handle_numbers_and_pointers()`

### 📌 Conversión y impresión
* `ft_itoa` → convierte enteros negativos y positivos a strings usando recursión.
* `ft_putnbr_hex` → imprime números en hexadecimal, minúscula o mayúscula.
* `ft_putptr` → imprime punteros con formato `0x` o `(nil)` si es NULL.
* `ft_putnbr_unsigned` → imprime enteros sin signo.
* `ft_putchar_fd` y `ft_putstr_fd` → funciones base para imprimir caracteres y strings.

### 📌 Contador de caracteres
* Cada función auxiliar incrementa un puntero `count` para devolver la longitud total impresa.

---

### 📌 `ft_printf.h`
* Contiene todos los **prototipos**, cabeceras necesarias (`stdarg.h`, `stdlib.h`, `unistd.h`) y funciones auxiliares.

---

### 📌 `Makefile`
* Reglas estándar:
  * `all` → compila el proyecto.
  * `clean` → elimina objetos `.o`.
  * `fclean` → elimina objetos y binarios.
  * `re` → recompila desde cero.

---

## 🧵 Posibles mejoras
* Manejar flags, ancho, precisión y padding como el printf real (`%05d`, `%.3s`, etc.).
* Optimizar funciones recursivas y reducir llamadas a `malloc` (por ejemplo en `ft_itoa`).
* Agregar soporte para floats y long long.

