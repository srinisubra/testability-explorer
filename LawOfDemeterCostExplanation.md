# Introduction #

The Law of Demeter, also known as the Principle of Least Knowledge, says that your code should only talk to its immediate collaborators. So, if you call a method on a dependency, and then use the resulting object my calling its methods, chances are good that your dependency should be calling those methods instead.

# Details #

The most common pattern that violates the Law of Demeter is this:

```
interface Foo {
  public ComplexObject getTheObject();
}
class MyClass {
  private Foo myDependency;
  public void method() {
    myDependency.getTheObject().doThingWithIt();
  }
}
```

This is a bad thing to do, because it makes testing MyClass harder. If we want to set a Mock, Stub, or Fake object into myDependency, we will be forced to implement doThingWithIt() on that object, in a way that keeps MyClass happy. The more MyClass knows how traverse the object graph of its collaborators, the more complex the test fixture for MyClass becomes.

If you control the Foo class, you should prefer to ask it to perform the work on its own collaborators:

```
interface Foo {
  public void doThing();
}
class MyClass {
  private Foo myDependency;
  public void method() {
    myDependency.doThing();
  }
}
```