- Go语言中接口与泛型区别
	- 在 Go 语言中，接口和泛型是两个不同的概念，它们用于不同的目的。
	- 接口是一种类型，它定义了一组方法，但不提供实现。通过实现接口中定义的方法，类型可以满足接口的要求。通过接口可以实现多态，即不同类型的值可以以相同的方式处理，这对于编写可扩展的代码非常有用。
	- 泛型是一种通用的编程技术，它允许使用一种通用的类型或算法来处理多种不同的类型。泛型可以提高代码的复用性和可读性，同时减少错误和重复代码。Go 语言在 2021 年发布的版本中加入了泛型支持，这使得开发人员可以使用泛型来编写更灵活的代码。
	- 尽管接口和泛型都可以用于实现多态，但它们的实现方式和目的不同。接口是通过定义一组方法来实现多态，而泛型是通过参数化类型或算法来实现多态。接口用于定义行为，泛型用于定义数据结构和算法。
- 分别用代码举例Go语言中接口与泛型区别
	- 下面是使用 Go 语言中接口和泛型的示例代码，以突出它们之间的区别。
	- 使用接口实现多态：
	- ```
	  type Animal interface {
	      Speak() string
	  }
	  
	  type Cat struct{}
	  
	  func (c Cat) Speak() string {
	      return "Meow!"
	  }
	  
	  type Dog struct{}
	  
	  func (d Dog) Speak() string {
	      return "Woof!"
	  }
	  
	  func main() {
	      animals := []Animal{Cat{}, Dog{}}
	      for _, animal := range animals {
	          fmt.Println(animal.Speak())
	      }
	  }
	  
	  ```
	- 在这个例子中，我们定义了一个 `Animal` 接口，该接口包含一个 `Speak` 方法。然后，我们定义了两个类型 `Cat` 和 `Dog`，它们都实现了 `Animal` 接口中的 `Speak` 方法。在 `main` 函数中，我们创建了一个包含 `Cat` 和 `Dog` 实例的切片，并通过循环遍历每个动物并调用 `Speak` 方法来实现多态。
	- 使用泛型实现通用算法：
	- ```
	  func Sum[T Numeric](numbers []T) T {
	      var sum T
	      for _, number := range numbers {
	          sum += number
	      }
	      return sum
	  }
	  
	  func main() {
	      integers := []int{1, 2, 3, 4, 5}
	      fmt.Println(Sum(integers))
	  
	      floats := []float64{1.1, 2.2, 3.3, 4.4, 5.5}
	      fmt.Println(Sum(floats))
	  }
	  
	  ```
	- 在这个例子中，我们定义了一个泛型函数 `Sum`，该函数接受一个数字类型的切片并返回它们的总和。我们使用类型参数 `T` 来指定通用类型，然后使用类型约束 `Numeric` 来限制只能传递数字类型。在 `main` 函数中，我们分别传递一个包含整数和浮点数的切片，并打印出它们的总和。这里我们可以看到，通过泛型，我们可以在不必写重复代码的情况下处理不同类型的数据。