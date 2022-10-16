# Unit-Test

[I. Overview](#overview)

[II. Unit test](#unittest)

[III. Types of unit test](#types)

[IV. UT Project Structure](#structure)

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

#### 2. Stub:

<details>
  <summary>StubExample.cpp</summary>
  
  ```C++
  class Service
  {
      public:
          string doSomething(UserModeInterface user)
          {
              /*Do something*/
              return user.getUuid();
          }
  };
  ```
</details>

<details>
  <summary>StubExampleTest.cpp</summary>
  
  ```C++
  class UserModelStub : public UserModeInterface
  {
      public:
          string getUuid()
          {
              return 0000-0000-0000-0001;
          }
  };

  TEST_F(ServiceTest, doSomething_01)
  {
      //run the test
      auto uuid = service.doSomething(new UserModelStub());
      EXPECT_THAT(uuid, StrEq("0000-0000-0000-0001");
  }
  ```
  
</details>

#### 3. Spy (Need to verify the example):

<details>
  <summary>SpyExample.cpp</summary>
  
  ```C++
  class Logger
  {
      public:
          void log(string message) = 0;
  };
  
  class UserNotifier
  {
      public:
          UserNotifier(Logger logger): m_logger(logger) {}
  
          void registerUser(UserModeInterface user)
          {
              m_logger.log("Notifying the user: " + user.name());
          }
      private:
          Logger m_logger;
  };
  ```

</details>

<details>
  <summary>SpyExampleTest.cpp</summary>
  
  ```C++
  class LoggerSpy : public Logger
  {
      puclic:
          vector<string> messages{};
          void log(string message): void
          {
              messages.push_back(message);
          }
  };
  
  TEST_F(UserNotifierTest, registerUser_01)
  {
      LoggerSpy logger = new LoggerSpy();
      UserNotifier notifier = new UserNotifier(logger);
  
      User user = new User(name = "Hello");
      notifier.registerUser(user);
      EXPECT_THAT(logger.messages.back(), StrEq("Notifying the user: Hello));
  }
  ```
  
</details>
  
#### 4. Fake (Need to verify the example):
<details>
  <summary>FakeExample.cpp</summary>
  
  ```C++
  class FakeExample
  {
      public:
          virtual void doSomething(int& value, int& error)
          {
              value = External::API::getValue();
              error = External::API::getError();
          }
  };
  ```

</details>

<details>
  <summary>FakeExampleTest.cpp</summary>
  
  ```C++
  class FakeExampleTest : public FakeExample
  {
      public:
          void doSomething(int& value, int& error) override
          {
              value = m_value;
              error = m_error;
          }
          int m_value;
          int m_error;
  };
  ```
  
</details>
  
<a name="structure"></a>
## IV. UT Project Structure
### An example of UT folder structure:
  >Insert image Later
    
### Parts of a test:
  >Insert image later
    
#### Class that being tested is called: Class under test.

<details>
  <summary>A.hpp</summary>
  
  ```C++
  class A
  {
      public:
          A(B* b): m_b(b) {}
          virtual ~A(){}
          virtual void doThis();
  
      private:
          void doRecovery();
          B* m_b;
  };
  ```

</details>
  
<details>
  <summary>A.cpp</summary>
  
  ```C++
  void A::doThis()
  {
      int error;
      if (m_b->doIncremental(error) == 0)
      {
          if (m_b->doIncremantal(error) == 1)
          {
              if(error != 0)
              {
                  doRecovery();
              }
          }
      }
      else
      {
          if (error != 0)
          {
              doRecovery();
          }
      }
  }
  
  void A::doRecovery()
  {
      if (C::doRecovery())
      {
          //doThings();
      }
  }
  ```
  
</details>

_Other outside classes are dependencies classes. We will try to mock all the dependencies to make the test independently_

<details>
  <summary>B.hpp</summary>
  
  ```C++
  class B
  {
      public:
          virtual int doIncremental(int& error);
      private:
          int doInternal(int& error);
          int m_state;
  };
  ```
  
</details>
  
<details>
  <summary>B.cpp</summary>
  
  ```C++
  int B::doIncremental(int& error)
  {
      return doInternal(error);
  }
  int B::doInternal(int& error)
  {
      error = 0;
      return m_state++;
  }
  ```
  
</details>
  
<details>
  <summary>C.hpp</summary>
  
  ```C++
  class C
  {
      public:
          static bool doRecovery();
  };
  ```
  
</details>
  
<details>
  <summary>C.cpp</summary>
  
  ```C++
  bool C::doRecovery()
  {
      return true;
  }
  ```
  
</details>
  
_Each class has it's own mock class. Normally we just need to mock public methods only_
<details>
  <summary>MockA.hpp</summary>
  
  ```C++
  class MockA : public A
  {
      public:
          MOCK_METHOD(void, doThis, ());
  };
  ```
  
</details>
  
<details>
  <summary>MockB.hpp</summary>
  
  ```C++
  class MockB : public B
  {
      public: 
          MOCK_METHOD(int, doIncremental, (int& error));
  };
  ```
  
</details>
  
_The function being called of C class has a static funtion. Gmock doesn't support mock non-virtual functions so we need another solution for it._
_Here we use the combination of mock and fake_
  * The mock provides expectation.
  
<details>
  <summary>MockC.hpp</summary>

  ```C++
  class MockC
  {
      public:
          MOCK_METHOD(bool, doRecovery());
  };
  Mock* M_c = nullptr;
  ```

</details>
  
  * And the fake help to link the call from the class under test to the mock
<details>
  <summary>FakeC.cpp</summary>

  ```C++
  #include "C.hpp"
  #include "MockC.hpp"

  //We can directly put it in MockC.hpp or in fake/C.cpp because it define the body of methods for C class
  bool C::doRecovery()
  {
      return M_c->doRecovery();
  }
  ```

</details>
