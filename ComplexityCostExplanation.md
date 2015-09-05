# Introduction #

The Complexity Cost measures how hard your code is to test as a result of non-injectable method calls you make, by aggregating their recursive cyclomatic complexity. This estimates how much longer the tests will take to run, and the negative influence they have on your ability to unit test the code in isolation.

# Details #

A method call is **non-injectable** if there is no way to replace it in a unit test, forcing all unit tests to invoke that code. For example, these are all non-injectable:

```
class Bad {
  private Counter counter = new Counter();

  public void nonInjectables() {
    float foo = MathUtils.squareRoot(100); // static method
    int bar = counter.nextInt(); // always calls Counter#nextInt()
    
  }
}
```

This is bad because you should be able to unit test your code in isolation, and non-injectable methods are code outside of the class under test that you cannot avoid calling. This is true especially if the dependencies are complex. The cost of a non-injectable call to `MathUtils.squareRoot()` is probably low, because that is a simple method. However, the cost of calling some business logic in another part of your application will be quite high, because the cost is recursive and includes all the code branches that are executed downstream.

The better way to code this is to be sure that all references to external objects may be replaced by a unit test. Here are a few examples:

Allow constructor injection where possible, or setter injection otherwise:
```
class Good {
  private Counter counter;
  public void setCounter(Counter counter) {
    this.counter = counter;
  }
}
```
```
class Better {
  private final Counter counter;
  public Better(Counter counter) {
    this.counter = counter;
  }
}
```