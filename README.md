Build
=====

Build is a library for defining and instantiating valid instances of objects. It is intended for use during testing. 

Example
-------

(This example uses NUnit)

```c#
public class Build
{
    private static readonly Dictionary<string, Func<object>> defaults = new Dictionary<string, Func<object>>
        {
            {TypeKey<Person>(), () => new Person("Peter", 8)},
            {"ANotValidPerson", () => new Person("Peter", -2)}
        };

    // ... some other stuff 
}

[TestFixture]
public class Tests
{
    [Test]
    public void ValidPerson() {
        var validPerson = Build.A<Person>();
        Assert.IsTrue(validator.IsValid(validPerson));
    }

    [Test]
    public void InvalidPerson() {
        var notValidPerson = Build.A<Person>("ANotValidPerson");
        Assert.IsFalse(validator.IsValid(notValidPerson));
    }

    [Test]
    public void AnotherInvalidPerson() {
        var notValidPerson = Build.A<Person>((p) => p.Age = -7);
        Assert.IsFalse(validator.IsValid(notValidPerson));
    }

    [Test]
    public void PersonWhoCanDriveInAustralia() {
        var person = Build.A<Person>((p) => p.Age = 17);
        Assert.IsTrue(person.CanDrive());
    }
}
```