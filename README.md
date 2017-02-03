### Type inference
    
    class Animal
    class Dog extends Animal
    class Cat extends Animal

    val list = List(new Dog(), new Cat())

### Variance affects type compatibility between parameterized types

* Invariant - for any T and S, you can assign a value of type List[T] to a variable of type List[S] if and only if T = S
    
   ```
    class Container[A]

    val test1 = new Container[String]
    val test2: Container[AnyRef] = new  Container[String]
    val test3: Container[String] =  new Container[AnyRef]
   ``` 
    
* Covariant - for any T extending S, you can assign a value of type List[T] to a variable of type List[S]
                
   ```
    class Container[+A]

    val test1 = new Container[String]
    val test2: Container[AnyRef] = new  Container[String]
    val test3: Container[String] =  new Container[AnyRef]
  ```  
    
* Contravariant - for any T extending S, you can assign a value of type Predicate[S] to a variable of type Predicate[T]
    
  ```
    class Container[-A]
   
    val test1 = new Container[String]
    val test2: Container[String] =  new Container[AnyRef]
    val test3: Container[AnyRef] = new  Container[String]
  ```  
   
### java.util.List is invariant

  ```
	import java.util.{List => JList, Arrays}
	
	val list1 = Arrays.asList("string1", "string2")
	
	val list2: JList[AnyRef] = list1
  ``` 
	
### scala.List is covariant in its type parameter

### contravariance example
    
   ```
    trait Predicate[-T] {
      def apply(obj: T): Boolean
    }
    
    object Predicate {
      def apply[T](f: (T) => Boolean) = new Predicate[T] {
        def apply(obj: T) = f(obj)
      }
    }
   ```

### scala.Function is contravariant on in its input argument type and covariant on the return type parameter

    * func(a: A): B

     tyoe A - make fewer requirements on it’s input type - no possible to call a method that only exists on subtypes of A.
     type B - at least as specialised as B, since the caller of the function may be expecting all the methods on B to be available.

 
### Recap

	Depending on class responsibilities
	
	* Consumer: accepts values of generic type but does not return it - make it contravariant
	
	* Producer: if only returns values of generic type - make it covariance

    * "PECS" idiom ("Producer extends, Consumer super") because variance annotations are written extends and super in Java.