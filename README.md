# Unit-Test

[I. Overview](#overview)

[II. Unit test](#unittest)

[III. Types of unit test](#types)

<a name="overview"></a>
## I. Overview

Guide and example to write basic Unit test based on Google Test framework
Google has provided a very detail framework but we should focus more about some most used cases.

<a name="unittest"></a>
## II. Unit test

### What is Unit test

- A framework that helps us to simulate the input and verify the output according to the test case.
- Done by developers.
- Isolate all logic and decoupled from the outside.
- White box testing.
- Automated by the framework.

### Purpose of Unit test
- Ensure that a section of the application (known as "unit") meets it's design and behaves as intended.

<a name="types"></a>
## III. Types of Unit test

| Type | Usage |
|------|-------|
| Dummy | Objects are passed around but never be actually used.<br>Usually they are just used to fill the parameters list. |
| Stub | Return a fake value when they're called |
| Spy | Based on stub but also records some information based on how it's called |
| Fake | A shortcut of the actual implementation that suits the test but not the production.<br>Normally used when you're using external services/APIs. |
| Mock | A mock does not have a working implementation. Instead we register predefined behaviors that we want the mock to behave and assert if ther're expected.<br>Mock provides the ability to set the expectations and assert them. |

### Some examples of the types:
#### 1. Dummy:

<details>
  <summary>Dummy.cpp</summary>
  
 ```C++
  State Example::doSomething(const uint32_t value)
  {
      State ret = STATE_ERROR;
      if (STATE_ERROR == m_state)
          {
              Foo::doSomeOthers(value));
              ret = STATE_SUCCESS;
          }
      return ret;
  }
 ``` 
</details>

<details>
  <summary>DummyTest.cpp</summary>
  
 ```C++
  TEST_F(ExampleTest, doSomeThing_01)
  {
      //precondition set state != STATE_READY
      testObject->setState(STATE_ERROR);
      //run the test
      auto ret = testObject->doSomething(0);
      EXPECT_EQ(ret, STATE_ERROR);
  }
 ```
</details>

