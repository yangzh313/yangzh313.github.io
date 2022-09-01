title:: Chapter-14_Larger_Testing

## What Are Larger Tests? 什么是大型测试？
- 较大的测试有许多是小型测试所不具备的内容。它们受的约束不同；因此，它们可以表现出以下特征：
- 它们可能很慢。我们的大型测试的默认时长时间为15分钟或1小时，但我们也有运行数小时甚至数天的测试。
- 它们可能是不封闭的。大型测试可能与其他测试和流量共享资源。
- 它们可能是不确定的。如果大型测试是非密封的，则几乎不可能保证确定性：其他测试或用户状态可能会干扰它。
## Larger Tests at Google 谷歌的大型测试
- ## Conclusion
- 一个全面的测试套件需要大型测试，既要确保测试与被测系统的仿真度相匹配，又要解决单元测试不能充分覆盖的问题。因为这样的测试必然更复杂，运行速度更慢，所以必须注意确保这样的大型测试时正确的，良好的维护，并在必要时运行（例如在部署到生产之前）。总的来说，这种大型测试仍然必须尽可能的小（同时仍然保留仿真度），以避免开发人员的阻力。一个全面的测试策略，确定系统的风险，以解决这些风险的大型测试，对大多数软件项目来说是必要的。
## TL;DRs 内容提要
- Larger tests cover things unit tests cannot.
- Large tests are composed of a System Under Test, Data, Action, and Verification.
- A good design includes a test strategy that identifies risks and larger tests that mitigate them.
- Extra effort must be made with larger tests to keep them from creating friction in the developer workflow.
- 大型测试涵盖了单元测试不能涵盖的内容。
- 大型测试是由被测系统、数据、操作和验证组成。
- 良好的设计包括识别风险的测试策略和缓解风险的大型测试。
- 必须对大型测试做出额外的努力，以防止它们在开发者的工作流程中产生阻力。