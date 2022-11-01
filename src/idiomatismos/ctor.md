# Constructores

## Descripción

Rust no dispone de constructores como elemento del lenguaje. A falta de ellos, para crear un objeto se usa por convención la función asociada denominada `new`.

```rust
# #![allow(unused)]
# fn main() {
/// Tiempo en segundos.
///
/// # Ejemplo
///
/// ```
/// let s = Second::new(42);
/// assert_eq!(42, s.value());
/// ```
pub struct Second {
    value: u64
}

impl Second {
    // Construye una nueva instancia de [`Second`].
    // Hay que observar que  es una función asociada:
    // es decir, no tiene self.
    pub fn new(value: u64) -> Self {
        Self { value }
    }

    /// Devuelve el valor en segundos.
    pub fn value(&self) -> u64 {
        self.value
    }
}
# }
```

## Constructores por defecto

Rust facilita un constructor por defecto mediante el uso del rasgo `Default`:

```rust
# #![allow(unused)]
# fn main() {
/// Tiempo en segundos.
///
/// # Ejemplo
///
/// ```
/// let s = Second::default();
/// assert_eq!(0, s.value());
/// ```
pub struct Second {
    value: u64
}

impl Second {
    /// Devuelve el tiempo en segundos.
    pub fn value(&self) -> u64 {
        self.value
    }
}

impl Default for Second {
    fn default() -> Self {
        Self { value: 0 }
    }
}
# }
```

`Default` puede derivarse si todos los tipos de los campos de la estructura implementan `Default`, como sucede con el tipo `Second`. Así, es posible hacer lo siguiente:

```rust
# #![allow(unused)]
# fn main() {
/// Tiempo en segundos.
///
/// # Ejemplo
///
/// ```
/// let s = Second::default();
/// assert_eq!(0, s.value());
/// ```
#[derive(Default)]
pub struct Second {
    value: u64
}

impl Second {
    /// Devuelve el valor en segundos.
    pub fn value(&self) -> u64 {
        self.value
    }
}
# }
```

**Nota**: es habitual y se espera que los tipos que se definan implementen ambos: `Default` y un constructor `new` vacío. Como `new` es, por convención, el constructor por defecto y los usuarios esperan que exista, es razonable que no tenga argumentos en su definición. Lo normal sería que su comportamiento funcioanl coincidera con los valores asignados con `Default`.

**Pista**: la ventaja de implementar o derivar `Default` es que el tipo que lo hace puede utilizarse allí donde se requiera el rasgo `Default`, por ejemplo, en cualquiera de [las funciones `*or_default` de la librería estándar](https://doc.rust-lang.org/stable/std/?search=or_default).

## Vea también

- El [idiomatismo Default](./default.md) para una descripción en profundidad del rasgo `Default`.
- El [patrón constructor](../patterns/creational/constructor.md) para la construcción de objetos cuando se necesitan múltiples configuraciones.
- [Las recomendaciones para construir APIs/C-COMMON-TRAITS](https://rust-lang.github.io/api-guidelines/interoperability.html#types-eagerly-implement-common-traits-c-common-traits) para implementar ambos, `Default` y `new`.
