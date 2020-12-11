## The Pursuit of Perfect Pipeline: Part #1 TDD

> “Give me six hours to chop down a tree and I will spend the first four sharpening the axe.” — Abraham Lincoln

In the upcoming posts I would like to cover an important part of project development, the software life-cycle. The life-cycle of a software starts from the very first day you create the project (sometimes even earlier) till the endless delivery to production. It includes deciding what kind of requirements your code has to pass before it can be deployed/merged, what is the deployment strategy for your product, how the team members collaborate and work together and a lot more. I will do my best to share our insights from the last few years and how we, at The Elegant Monkeys, managed to find our software pipeline.

**The goal of these posts is to help you build the perfect trust-worthy pipeline for your project.**

This part is going to focus on automatic testing and specifically TDD (test driven development) and why testing is so important.

### Why should you care?

As a startup, we started with no tests at all and after a very short time in production, we realized that we made a huge mistake. The bugs kept coming, servers went down in the middle of the night, hours of maintenance and QA just to keep our product sane. We have always chased the next feature without spending enough time on the foundation. So we started writing tests, at the beginning just a tiny bit, and slowly added more and more till we fully transitioned to test-driven-development.

If you really want to sleep better at night, reduce maintenance and QA time, deliver a stable and agile product, or simply just build a better technology, you should consider doing TDD or at least writing enough tests to trust your code.

The most popular excuse for not doing enough tests is that takes a lot of time to write them. And indeed, it takes more time to develop a fully tested feature (roughly 20%-30% more time), than a non-tested feature but when looking at the bigger picture, you will realize how many effort and time you saved by doing so.

### OK, I get it now but how should I do it?

TDD is based on a very simple concept which may sound weird at first but it makes more sense as you get used to it.

There are three phases in TDD development cycle:

**_Red —_**_Create a test and make it fail._

1.  Imagine how the new code should be called and write the test as if the code already existed.
2.  Create the new code stub. Write just enough code so that it compiles.
3.  Run the test. It should fail. This is a calibration measure to ensure that your test is calling the correct code and that the code is not working by accident. This is a meaningful failure, and you expect it to fail.

**_Green —_**_Make the test pass by any means necessary._

1.  Write the code to make the test pass. Keep it simple.
2.  Sometime it is best even to hard-code the expected return value to verify that the test correctly detects success.
3.  If you’ve written the code so that the test passes as intended, you are finished. You do not have to write more code. If new functionality is still needed, then another test is needed. Make this one test pass and continue.
4.  When the test passes, you might want to run all tests up to this point to build confidence that everything else is still working.

**_Refactor —_**_Change the code to remove duplication in your project and to improve the design while ensuring that all tests still pass._

1.  Remove duplication caused by the addition of the new functionality.
2.  Make design changes to improve the overall solution.
3.  After each refactoring, rerun all the tests to ensure that they all still pass.

Repeat the cycle, each cycle should be very short. You shouldn’t write code without writing its test before and you also shouldn’t write tests that you know that will pass before writing any new code. Thanks to this process you will develop the functionality you really need and not what you think you need.

### The top three test types

There are many types of tests and each and every one has a whole philosophy of how to write this type of test and what are the best practices. I want to focus on the core and most popular types of test:

*   **Unit tests —**Ensure that individual components of the code work as expected. Most of the time you will mock dependencies and ignore side-effects. Assertions test the component output according to a given input.
*   **Integration tests —**Ensure that component collaboration work as expected. Assertions may test component API, UI, or side-effects (such as database I/O, logging, etc…)
*   **Functional tests —**Ensure that the application works as expected from the user’s perspective. Assertions primarily test the user interface.

TDD is mostly around unit tests, but we modified it a bit to include also integration tests. For example, let’s say I am building an API server for my application and in this server I have my model class which communicates with the database. This model class shouldn’t be unit tested because it is all about side-effects and nothing else, no business logic at all. So you follow the TDD cycle but this time you write an integration test which truly connects to the database and not a unit test which mocks it.

Some also adapt TDD to functional tests. They start the development by writing one functional test, then the relevant integration and unit tests and only then the code itself. It is really more of a personal choice, take your time to adapt TDD as you please.

### I am working on a legacy, this ain’t for me

You have read this far and you got really excited about TDD and automatic testing but then you remembered that you are working on a legacy and it will take too much time to test it from zero. Don’t worry, you don’t have to start immediately with a full blown test suite. Start small, if you are working on a new feature be sure to test it, even not with TDD. Fixed a bug maybe? add the relevant test to check that it is not coming back again. After a while, your test suite will get bigger and bigger and you will be able to follow the TDD principals easily.

TDD needs a lot of motivation and effort to get it going but then you can’t go back to regular programming. It makes your product more stable and you really can trust your code. Be sure to give it a shot [The next post](/posts/pursuit-of-perfect-pipeline-part-2/) is about choosing the right source control branching model for your project, a very fundamental block in the life-cycle. Stay tuned!
