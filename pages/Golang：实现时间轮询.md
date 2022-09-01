- ```
  func main() {
  	t := time.NewTicker(time.Second * 2)
      
      func() {
      	for {
          	select {
              case <-t.C:
              	fmt.Println(1)
              }
          }
      }{}
  }
  ```