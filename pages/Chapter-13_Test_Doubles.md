title:: Chapter-13_Test_Doubles

## 测试替代对软件开发的影响
- 可测试性
- 适用性
- 仿真度
## 谷歌的测试替代
## 基本概念
## 测试替代的使用技巧
### Faking 伪造
- ### Stubbing 打桩
  
  打桩是指将行为赋予一个函数的过程，如果该函数本身没有行为，则你可以为该函数指定要返回的值（即打桩返回值）。
- ### Interaction Testing 交互测试
  
  交互测试是一种在不实际调用函数实现的情况下验证函数调用方式的方法。如果函数没有正确调用，测试应该失败。例如，如果函数根本没有被调用，调用次数太多，或者调用参数错误。
### Prefer Realism Over Isolation 倾向于现实主义而不是孤立主义
- ## Faking 伪造测试
  
  如果在测试中使用真实实现是不可行的，那么最好的选择通常是使用伪造实现。与其他测试替代技术相比，伪造测试技术更受欢迎，因为它的行为类似于真实实现：被测试的系统甚至不能判断它是与真实实现交互还是与伪造实现交互
### Fakes Should Be Tested 伪造测试应当被测试
- ### When Is Stubbing Appropriate? 什么情况下才适合使用打桩测试？
  
  当你需要一个函数返回一个特定的值以使被测系统进入某种状态时，打桩方式就很合适，而不是真实实现的万能替代品，例如例13-12要求被测系统返回一个非空的事务列表。因为一个函数的行为是在测试中内联定义的，所以打桩可以模拟各种各样的返回值或错误，而这些返回值或错误可能无法从真实实现或伪造测试中触发。
  
  为了确保其目的明确，每个打桩函数应该与测试的断言直接相关。因此，一个测试通常应该打桩少量的函数，因为打桩太多会导致函数不够清晰。一个需要打桩许多函数的测试是一个迹象，表明打桩被过度使用，或者被测系统过于复杂，应该被重构。
## Interaction Testing 交互测试
- 在某些情况下，交互测试是有必要的：
- 你不能进行状态测试，因为你无法使用真实实现或伪造实现（例如，如果真实实现太慢，而且没有伪造测试存在）。作为备用方案，你可以进行交互测试以验证某些函数被调用。虽然不是很理想，但这确实提供了一些基本的功能，即被测系统正在按照预期工作。
- 对一个函数的调用数量或顺序的不同会导致不在预期内的行为。交互测试是有用的，因为用状态测试可能很难验证这种行为。例如，如果你期望一个缓存功能能减少对数据库的调用次数，你可以验证数据库对象的访问次数没有超过预期。使用Mockito，代码可能看起来类似于这样
## Conclusion 总结

We’ve learned that test doubles are crucial to engineering velocity because they can help comprehensively test your code and ensure that your tests run fast. On the other hand, misusing them can be a major drain on productivity because they can lead to tests that are unclear, brittle, and less effective. This is why it’s important for engineers to understand the best practices for how to effectively apply test doubles.

我们已经了解到，测试替代对工程速度至关重要，因为它们可以帮助全面测试代码并确保测试快速运行。另一方面，误用它们可能是生产率的主要消耗，因为它们可能导致测试不清楚、不可靠、效率较低。这就是为什么工程师了解如何有效应用测试替代的最佳实践非常重要。

There is often no exact answer regarding whether to use a real implementation or a test double, or which test double technique to use. An engineer might need to make some trade-offs when deciding the proper approach for their use case.

关于是使用真实实现还是测试替代，或者使用哪种测试替代技术，通常没有确切的答案。工程师在为他们的用例决定合适的方法时可能需要做出一些权衡。

Although test doubles are great for working around dependencies that are difficult to use in tests, if you want to maximize confidence in your code, at some point you still want to exercise these dependencies in tests. The next chapter will cover larger-scope testing, for which these dependencies are used regardless of their suitability for unit tests; for example, even if they are slow or nondeterministic.

尽管测试替代对于处理测试中难以使用的依赖项非常有用，但如果你想最大限度地提高代码的可信度，在某些时候你仍然希望在测试中使用这些依赖项。下一章将介绍更大范围的测试，对于这些测试，不管它们是否适合单元测试，都将使用这些依赖关系；例如，即使它们很慢或不确定。
## TL;DRs 内容提要
- A real implementation should be preferred over a test double.
- A fake is often the ideal solution if a real implementation can’t be used in a test.
- Overuse of stubbing leads to tests that are unclear and brittle.
- Interaction testing should be avoided when possible: it leads to tests that are brittle because it exposes implementation details of the system under test.
- 真实实现应优先于测试替代。
- 如果在测试中不能使用真实实现，那么伪造实现通常是理想的解决方案。
- 过度使用打桩会导致测试不明确和变脆。
- 在可能的情况下，应避免交互测试：因为交互测试会暴露被测系统的实现细节，所以会导致测试不连贯。