---
layout: post
title: GIVEN my article WHEN a reader reads it THEN the reader writes better tests
tags: testing, documentation, teamwork
---

## GIVEN a developer who hates writing tests
I'm not one for testing, that's evidenced by my most used side project having around 5% test coverage. I just can't bring myself to slog through and write every test case of the code I'm writing particularly when I don't have much time to work on side projects anymore.

## WHEN placed in a software house that likes testing
However, in my current employment I'm expected to have 80% code coverage as part of the minimum to calling something 'stable' - hence I actually spend most of my working time writing unit, integration, and system tests.

## AND they must write lots of tests in their job
Testing is something my company are very good at. We have hundreds of thousands of lines of code spread across hundreds of different repositories across tens of different projects and on any given day I could be working in any part of it.

The fact of the matter is that the tests save us countless headaches and make the insane amount of context switching significantly easier for us. Because of the high test coverage I know that I can hack and slash at code, and even perform massive refactors if I do so desire, and at the end of my rampage I can run the tests and know within minutes if there has been any changes to the code's behaviour.

I just don't need to worry about breaking code while I edit it.

## THEN they learn some neat tricks in testing
This is the point where I tell you about the wonderous world of behaviour driven tests.

We have a convention to write every test with the GIVEN-WHEN-THEN structure. Here's an example from my side project:
```csharp
[TestMethod]
public void TestReadStringDeserializesTheDataCorrectly()
{
    // GIVEN a buffer of serialized data
    mockMessageBuffer.Setup(m => m.Buffer).Returns(new byte[] { 0, 0, 0, 6, 65, 0, 66, 0, 67, 0 });
    mockMessageBuffer.Setup(m => m.Offset).Returns(0);
    mockMessageBuffer.Setup(m => m.Count).Returns(10);

    // WHEN I read a string from the reader
    string result = reader.ReadString();

    // THEN the value is as expected
    Assert.AreEqual("ABC", result);
}
```
I see this test and immediately know where to look for everything I need:

- In the GIVEN section we setup any mocks we need, load in any test data from disk if we require it, and do any preparation we have to.

- In the WHEN section we run the code being tested (and as best as we possibly can, *only* the code being tested) and obtain a result.

- In the THEN section we assert that the behaviour of the code was as we expect.

Having this simple structure makes understanding what a test does trivial. I can look through hundreds of tests in development and code review and instantly grasp what a test is doing in a matter of seconds because every test in our company has this same, behaviour driven format.

We also find this style creeps out into other parts of our tests as well:
```csharp
[TestInitialize]
public void Initialize()
{
    // GIVEN the object cache is disabled
    ObjectCache.Initialize(ObjectCacheSettings.DontUseCache);

    // AND a DarkRiftReader under test
    reader = DarkRiftReader.Create(mockMessageBuffer.Object);
}
```

You can start to see how we build more complex tests as well here. It's encouraged that if you do multiple things in any section that you document them with an AND section:
```csharp
[TestMethod]
public void TestExtraLargeMemoryBlocksArePooledCorrectly()
{
    // GIVEN a memory pool with no previously pooled memory

    // WHEN I return an extra large memory block
    byte[] oldBlock = new byte[5000];
    memoryPool.ReturnInstance(oldBlock);

    // AND I request a new extra large memory block
    byte[] newBlock = memoryPool.GetInstance(4000);

    // THEN my memory block is the same
    Assert.AreSame(oldBlock, newBlock);
}
```
(I take a shortcut here, I should probably assign `oldBlock` in a GIVEN, but then this is really just as understandable)

## AND can apply them anywhere
It also helps when writing tests!

Thinking about behaviour is a lot easier in English - that's why things like pseudo code and rubber duck debugging exist - so why not write tests in your native language and then use that as a template?

```csharp
[TestMethod]
public void TestGetInstanceUsesPoolWhenHasInstances()
{
    // GIVEN a pool with a single element

    // WHEN I get an instance from the pool

    // THEN a new instance is not generated

    // AND the returned instance is the pooled object
}
```

```csharp
[TestMethod]
public void TestGetInstanceUsesPoolWhenHasInstances()
{
    // GIVEN a pool with a single element
    object pooledObject = new object();
    objectPool.ReturnInstance(pooledObject);

    // WHEN I get an instance from the pool
    object result = objectPool.GetInstance();

    // THEN a new instance is not generated
    Assert.IsNull(newInstanceCaptor);

    // AND the returned instance is the pooled object
    Assert.IsNotNull(result);
    Assert.AreSame(pooledObject, result);
}
```
The exact order of the GIVEN-WHEN-THEN also doesn't matter, we often find that in the most complex of tests we need to assert behaviour in multiple places (THEN), or perform some step on the class under test (WHEN) before we can continue the setup (GIVEN):

```cucumber
@free
Scenario: I can disconnect from a server
	Given I have a running server
	And 1 client connected
	Then all clients should be connected
	And the server should have 1 client
	When I disconnect client 0
	Then 0 clients should be connected
	And 1 client should be disconnected
	And the server should have 0 clients
	And there should be no recycling warnings
```
## Conclusion
You have no idea what my side project does, I never mentioned it, but you were hopefully still able to understand what these tests did, despite them covering some of the most complex and wacky optimisations I've had to add to it!

Adopting this behaviour, for me, makes testing significantly less cumbersome. For the developers I work with, it makes understanding my tests faster and easier. And in the merge review we can review the test behaviour using the comments and then check the code against the comments to make reviews of large quantities of tests a little more manageable.

I hope this article has made you think, and maybe you'll write a quick GIVEN-WHEN-THEN before you write your next test 🙂
