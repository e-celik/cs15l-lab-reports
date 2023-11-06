# Lab Report 3: Bugs and Commands
By Ekin Celik
## Part 1: Bugs in reverseInPlace()

A failure-inducing input for the buggy method: reverseInPlace()

```
@Test
  public void test2reverseInPlace() {
    int[] input1 = {1, 2, 3};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[] {3, 2, 1}, input1);
  }
```

A non-failure-inducing input for method: reverseInPlace()
```
@Test 
	public void testReverseInPlace() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);
	}
```
