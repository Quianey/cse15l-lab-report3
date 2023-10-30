## Part 1 - Bugs

ArrayExamples:

**1. A failure-inducing input for the buggy program, as a JUnit test and any associated code**
```
  @Test
  public void testAverageMultipleElement() {
    double[] input1 = { 2, 7, 8, 8, 5, 2, 6, 7, 9};
    assertEquals((7+8+8+5+2+6+7+9)/8.0, ArrayExamples.averageWithoutLowest(input1), 0.0001);
  }
```

**2. An input that doesn’t induce a failure, as a JUnit test and any associated code**
```
  @Test
  public void testAverageFourElement() {
    double[] input1 = { 8, 8, 5, 9};
    assertEquals((8+8+9)/3.0, ArrayExamples.averageWithoutLowest(input1), 0.0001);
  }

}
```

**3. The symptom, as the output of running the tests**
![image](https://github.com/Quianey/cse15l-lab-report3/assets/147276821/48edd11e-dd90-4f77-9e80-300ad1fdcb56)


**4. The bug, as the before-and-after code change required to fix it** 
![image](https://github.com/Quianey/cse15l-lab-report3/assets/147276821/7a39e9a0-2729-4218-9e0a-c648343ce1a0)

**why the fix addresses the issue**
In the buggy code, we assumed that there is only one lowest number. If we have multiple lowest number, we would fail to add the remaining lowest numbers. For example, in my tester, I include two 2's in the input. With the buggy method, we will ignore both 2's. But we should add a 2. 
I fixed the code by adding every number in the list up and subtracting the smallest number, then calculating the average. This would make sure that I include more than one lowest numbers. 

## Part 2 - Researching Commands
