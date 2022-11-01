# Concatenar String con format!

## Descripción

Es posible construir cadenas utilizando los métodos `push` y `push_str` sobre una variable de tipo `mut String` (de tipo cadena de caracteres modificable). También se puede usar su operador `+`. Sin embargo, puede ser más conveniente usar la macro `format!`, especialmente cuando se trata de crear la cadena de caracteres con una mezcla de cadenas de caracteres literales con otras no literales.

## Ejemplo

```rust
#![allow(unused)]
fn main() {
fn say_hello(name: &str) -> String {
    // Podríamos construir la cadena de caracteres
    // resultante de forma manual 
    // let mut result = "Hello ".to_owned();
    // result.push_str(name);
    // result.push('!');
    // result

    // Pero el uso de format! es mejor.
    format!("Hello {}!", name)
}
}
```

## Ventajas

El uso de `format!` suele ser la forma más sucinta y legible de combinar cadenas de caracteres.

## Desventajas

No es la forma más eficiente de combinar cadenas de caracteres. La forma más eficiente de combinar cadenas es usar una `mut String` sobre la que se van realizando operaciones `push` (especialmente, si se ha prerreservado el espacio necesario para la cedena de caracteres).
