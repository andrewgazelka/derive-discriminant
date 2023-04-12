```rust
#[derive(Discriminant)]
#[derive(Debug, PartialEq, Eq)]
enum Abc {
    A,
    B { x: usize, y: usize },
}

fn main() {
    // try_from implementations
    let a = A::try_from(Abc::A).unwrap();
    assert_eq!(a, A);
    let not_a = A::try_from(Abc::B { x: 1, y: 2 });
    assert!(not_a.is_err());

    let b = B::try_from(Abc::B { x: 1, y: 2 }).unwrap();
    assert_eq!(b, B { x: 1, y: 2 });
    let not_b = B::try_from(Abc::A);
    assert!(not_b.is_err());

    // from implementations
    let a = Abc::from(A);
    assert_eq!(a, Abc::A);
    let b = Abc::from(B { x: 1, y: 2 });
    assert_eq!(b, Abc::B { x: 1, y: 2 });
    
    // casting
    let a = Abc::A;
    let a: Box<dyn Debug> = a.cast();
    assert_eq!(format!("{a:?}"), "A");

    let b = Abc::B { x: 1, y: 2 };
    let b: Box<dyn Debug> = b.cast();
    assert_eq!(format!("{b:?}"), "B { x: 1, y: 2 }");
    
    // is_{variant} methods
    assert!(Abc::A.is_a());
    assert!(!Abc::A.is_b());

    assert!(!Abc::B { x: 1, y: 2 }.is_a());
    assert!(Abc::B { x: 1, y: 2 }.is_b());
}
```
