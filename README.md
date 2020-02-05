# Tips for Testing Code

## Basic concept of testing

Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you didn't break anything. Deciding on what constitutes an adequate amount is up to your team, but having 100% coverage (all statements and branches) is how you achieve very high confidence and developer peace of mind. This means that in addition to having a great testing framework, you also need to use a [good coverage tool](https://docs.microsoft.com/en-us/visualstudio/test/using-code-coverage-to-determine-how-much-code-is-being-tested).

There's no excuse to not write tests. There's [plenty of good .NET test frameworks](https://github.com/thangchung/awesome-dotnet-core#testing), so find one that your team prefers. When you find one that works for your team, then aim to always write tests for every new feature/module you introduce. If your preferred method is Test Driven Development (TDD), that is great, but the main point is to just make sure you are reaching your coverage goals before launching any feature, or refactoring an existing one.



## Single concept per test

Ensures that your tests are laser focused and not testing miscellaenous (non-related) things, forces [AAA patern](http://wiki.c2.com/?ArrangeActAssert) used to make your codes more clean and readable.

**Bad:**

```csharp

public class MakeDotNetGreatAgainTests
{
    [Fact]
    public void HandleDateBoundaries()
    {
        var date = new MyDateTime("1/1/2015");
        date.AddDays(30);
        Assert.Equal("1/31/2015", date);

        date = new MyDateTime("2/1/2016");
        date.AddDays(28);
        Assert.Equal("02/29/2016", date);

        date = new MyDateTime("2/1/2015");
        date.AddDays(28);
        Assert.Equal("03/01/2015", date);
    }
}

```

**Good:**

```csharp

public class MakeDotNetGreatAgainTests
{
    [Fact]
    public void Handle30DayMonths()
    {
        // Arrange
        var date = new MyDateTime("1/1/2015");

        // Act
        date.AddDays(30);

        // Assert
        Assert.Equal("1/31/2015", date);
    }

    [Fact]
    public void HandleLeapYear()
    {
        // Arrange
        var date = new MyDateTime("2/1/2016");

        // Act
        date.AddDays(28);

        // Assert
        Assert.Equal("02/29/2016", date);
    }

    [Fact]
    public void HandleNonLeapYear()
    {
        // Arrange
        var date = new MyDateTime("2/1/2015");

        // Act
        date.AddDays(28);

        // Assert
        Assert.Equal("03/01/2015", date);
    }
}

```

## Three laws of TDD
    1. You may not write production code until you have written a failing unit test
    2. You may not write more of a unit test than is sufficient to fail, and compiling is failing
    3. You may not write more production code than is sufficient to pass the currently failing test
    
## Keeping Tests Clean
Problem with this approach – test code could outgrow your prod code and become unmanageable

Is dirty test code better than no test code?
* Tests must change as the production code changes
* The dirtier the tests, the harder they are to change
* Tests can become a liability due to technical debt of the dirtiness

Abandoning test code has the following consequences
* Production code defects rise
* Fear of changing code increases
* Cleaning / refactoring code descreases due to lack of confidence (Code rot!!!)

Test code is as important as production code – it should be treated as a first class citizen and should be as clean as production code.

## Tests Enable the -ilities
* Tests are what keep your production code flexible, maintainable and reusable.
* Tests allow you to make changes to code without fear.
* Tests enable change.
* Tests enable improving architecture.

Without tests, your code base rots.

## FIRST
* **(F)ast** – tests should be fast / run quickly
* **(I)ndependent** – tests should NOT depend on each other – tests should be able to be run in any order they like
* **(R)epeatable** – they should be repeatable in ANY environment without need for any specific infrastructure
* **(S)elf-validating** – they should have a boolean output – pass or fail, nothing else
* **(T)imely** – they need to be written in a timely fashion – just before the production code is written – ensures they are easy to code against

## Importance
Unit Tests are as important if not moreso than the production code they’re for becasue they allow you to make changes confidently and without fear. They allow you to mold your code over time to improve flexibility / maintainability.
