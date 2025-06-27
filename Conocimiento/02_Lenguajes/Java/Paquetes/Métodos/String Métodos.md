#java #lenguajes 

La clase `String` en Java proporciona numerosos métodos para manipular y trabajar con cadenas de texto. A continuación, se describen los métodos principales.

---
## 1. `charAt(int index)`
Devuelve el carácter en la posición especificada.

```java
String texto = "Hola";
char letra = texto.charAt(1); // Devuelve 'o'
```

---

## 2. `codePointAt(int index)`
Devuelve el valor Unicode del carácter en la posición especificada.

```java
String texto = "Hola";
int codigo = texto.codePointAt(1); // Devuelve 111 (Unicode de 'o')
```

---

## 3. `compareTo(String str)`
Compara dos cadenas lexicográficamente.

```java
String a = "Hola";
String b = "Mundo";
int resultado = a.compareTo(b); // Devuelve un número negativo (a < b)
```

---

## 4. `compareToIgnoreCase(String str)`
Compara dos cadenas ignorando mayúsculas y minúsculas.

```java
String a = "Hola";
String b = "hola";
int resultado = a.compareToIgnoreCase(b); // Devuelve 0 (son iguales)
```

---

## 5. `concat(String str)`
Concatena (une) dos cadenas.

```java
String a = "Hola";
String resultado = a.concat(" Mundo"); // Devuelve "Hola Mundo"
```

---

## 6. `contains(CharSequence s)`
Comprueba si la cadena contiene la secuencia de caracteres especificada.

```java
String texto = "Hola Mundo";
boolean contiene = texto.contains("Mundo"); // Devuelve true
```

---

## 7. `endsWith(String suffix)`
Comprueba si la cadena termina con la secuencia de caracteres especificada.

```java
String texto = "Hola Mundo";
boolean termina = texto.endsWith("Mundo"); // Devuelve true
```

---

## 8. `equals(Object anObject)`
Comprueba si dos cadenas son iguales.

```java
String a = "Hola";
String b = "Hola";
boolean iguales = a.equals(b); // Devuelve true
```

---

## 9. `equalsIgnoreCase(String anotherString)`
Comprueba si dos cadenas son iguales, ignorando mayúsculas y minúsculas.

```java
String a = "Hola";
String b = "hola";
boolean iguales = a.equalsIgnoreCase(b); // Devuelve true
```

---

## 10. `indexOf(String str)`
Devuelve la posición de la primera aparición de la cadena especificada. lastindexOf()

```java
String texto = "Hola Mundo";
int posicion = texto.indexOf("Mundo"); // Devuelve 5
```

---

## 11. `isEmpty()`
Comprueba si la cadena está vacía.

```java
String texto = "";
boolean vacia = texto.isEmpty(); // Devuelve true
```

---

## 12. `length()`
Devuelve la longitud de la cadena.

```java
String texto = "Hola";
int longitud = texto.length(); // Devuelve 4
```

---

## 13. `replace(char oldChar, char newChar)`
Reemplaza todas las apariciones de un carácter por otro.

```java
String texto = "Hola";
String reemplazado = texto.replace('a', 'o'); // Devuelve "Holo"
```

---

## 14. `startsWith(String prefix)`
Comprueba si la cadena empieza con la secuencia de caracteres especificada.

```java
String texto = "Hola Mundo";
boolean empieza = texto.startsWith("Hola"); // Devuelve true
```

---

## 15. `substring(int beginIndex, int endIndex)`
Devuelve una subcadena desde el índice de inicio hasta el índice de fin (excluyente).
También sirve para eliminar valores

```java
String texto = "Hola Mundo";
String subcadena = texto.substring(0, 4); // Devuelve "Hola"
```

```java
String texto = "Maria"
String texto = texto.substring(0); // "aria"
```

---

## 16. `toLowerCase()`
Convierte todos los caracteres de la cadena a minúsculas.

```java
String texto = "HOLA";
String minusculas = texto.toLowerCase(); // Devuelve "hola"
```

---

## 17. `toUpperCase()`
Convierte todos los caracteres de la cadena a mayúsculas.

```java
String texto = "hola";
String mayusculas = texto.toUpperCase(); // Devuelve "HOLA"
```

---

## 18. `trim()`
Elimina los espacios en blanco al inicio y al final de la cadena.

```java
String texto = "  Hola  ";
String sinEspacios = texto.trim(); // Devuelve "Hola"
```

---

## 19. `valueOff()`
Convierte otros tipos de datos a `String`.

```java
int numero = 10;
String texto = String.valueOff(numero); // Devuelve "10"
```

--- 
