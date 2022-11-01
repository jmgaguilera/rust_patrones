# El uso de tipos prestados como argumentos

## Descripción

Cuando necesitamos decidir el tipo del argumento de una función, el uso de un tipo que implemente `Deref` (que permite el uso implícito de la conversión por derreferenciación) aumenta la flexibilidad del código. Gracias a esto, la función aceptará un mayor número de tipos de entrada.

Esto no se limita a los tipos *punteros gruesos* (fat pointers) o *fragmentables* (slice-able). De hecho, siempre se debería preferir el uso del **tipo prestado** (borrowed type) sobre **tomar prestado el tipo propio** (owned type). Por ejemplo, se prefiere `&str` sobre `&String`, `&[T]` sobre `&Vec<T>` o `&T` sobre `&Box<T>`.

Mediante el uso de tipos prestados se evitan capas de indirección para aquellas instancias en las que el tipo propio ya proporciona dicha indirección. Por ejemplo, un `String` ya dispone de una capa de indirección, por lo que `&String` se compone de dos capas de indirección. En lugar de esto, podemos evitarlo mediante el uso de `&str` como tipo del argumento de la función y dejar que `&String` se convierta a `&str` cuando se llame a la función.

## Ejemplo

En el siguiente ejemplo se ilustrarán algunas diferencias entre el uso de `&String` como tipo para argumento de una función y el uso de `&str` en su lugar. En cualquier caso, estas ideas se aplican también al uso de `&Vec<T>` en lugar de `&[T]`, o al uso de `&Box<T>` en lugar de `&T`.

Consideremos que deseamos determinar si una palabra contiene tres vocales consecutivas. No necesitamos la propiedad de la cadena de caracteres para determinar esto, por lo que solamente es necesaria una referencia.

El código podría ser como sigue:

```rust
fn three_vowels(word: &String) -> bool {
    let mut vowel_count = 0;
    for c in word.chars() {
        match c {
            'a' | 'e' | 'i' | 'o' | 'u' => {
                vowel_count += 1;
                if vowel_count >= 3 {
                    return true
                }
            }
            _ => vowel_count = 0
        }
    }
    false
}

fn main() {
    let ferris = "Ferris".to_string();
    let curious = "Curious".to_string();
    println!("{}: {}", ferris, three_vowels(&ferris));
    println!("{}: {}", curious, three_vowels(&curious));

    // Esto funciona bien, pero las siguientes dos líneas no compilarían:
    // println!("Ferris: {}", three_vowels("Ferris"));
    // println!("Curious: {}", three_vowels("Curious"));

}
```

El código anterior funciona bien porque pasamos un `&String` como parámetro. Si se quitan los comentarios de las últimas dos líneas, el ejemplo no compila. Esto se debe a que `&str` no puede convertirse de forma automática al tipo `&String`. Lo podemos resolver con solo cambiar el tipo del argumento de la función.

Si la declaración de la función se modifica a:

```rust
fn three_vowels(word: &str) -> bool {
```

Ambas líneas de código compilarán sin problemas e imprimirán la misma salida:

```rust
Ferris: false
Curious: true
```

Pero eso no es todo. Hay más que contar. Es posible que te hayas dicho a ti mismo: esto no me importa, nunca voy a usar un `&'static str` como parámetro de nada (como hemos hecho cuando usamos "Ferris" en la penúltima línea). Incluso, si ignoráramos este ejemplo concreto, puedes encontrar casos en los que el uso como argumento de `&str` te da más flexibilidad que el de `&String`.

Vamos a ver otro ejemplo en el que alguien nos pasa una frase y queremos determinar si alguna de las palabras de la frase contiene tres vocales consecutivas. Deberíamos usar la función que ya hemos definido y simplemente alimentarla de cada una de las palabras de la frase a analizar.

El código podría ser como el que sigue:

```rust
fn three_vowels(word: &str) -> bool {
    let mut vowel_count = 0;
    for c in word.chars() {
        match c {
            'a' | 'e' | 'i' | 'o' | 'u' => {
                vowel_count += 1;
                if vowel_count >= 3 {
                    return true
                }
            }
            _ => vowel_count = 0
        }
    }
    false
}

fn main() {
    let sentence_string =
        "Once upon a time, there was a friendly curious crab named Ferris".to_string();
    for word in sentence_string.split(' ') {
        if three_vowels(word) {
            println!("{} has three consecutive vowels!", word);
        }
    }
}
```

Al ejecutar este ejemplo con nuestra función con un argumento de tipo `&str` se imprime:

```text
curious has three consecutive vowels!
```

Sin embargo, si se cambia la función para que su argumento sea de tipo `&String`, este ejemplo no funcionará. Esto se debe a que los fragmentos de una cadena de caracteres son de tipo `&str` y no `&String`: esta conversión requeriría una asignación de memoria costosa que no se hace implícita), mientras que la conversión de `&String` a `&str` es muy económica e implícita.

## Vea también

- [La Referencia del lenguaje Rust: sobre conversiones de tipos](https://doc.rust-lang.org/reference/type-coercions.html).
- Para más información sobre cómo trabajar con `String` y `&str` se puede leer esta [serie de blogs(2015)](https://web.archive.org/web/20201112023149/https://hermanradtke.com/2015/05/03/string-vs-str-in-rust-functions.html) de Herman J. Radtke III.
