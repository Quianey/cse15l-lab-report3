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

**before**
```
  // Averages the numbers in the array (takes the mean), but leaves out the
  // lowest number when calculating. Returns 0 if there are no elements or just
  // 1 element in the array
  static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    for(double num: arr) {
      if (num != lowest) { sum+= num; }
    }
    return sum / (arr.length - 1);
  }


}
```
***after***
```
  // Averages the numbers in the array (takes the mean), but leaves out the
  // lowest number when calculating. Returns 0 if there are no elements or just
  // 1 element in the array
  static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    for(double num: arr) {
      sum += sum; 
    }
    sum -= lowest; 
    return sum / (arr.length - 1);
  }


}
```

**why the fix addresses the issue**

In the buggy code, we assumed that there is only one lowest number. If we have multiple lowest number, we would fail to add the remaining lowest numbers. For example, in my tester, I include two 2's in the input. With the buggy method, we will ignore both 2's. But we should add a 2. 
I fixed the code by adding every number in the list up and subtracting the smallest number, then calculating the average. This would make sure that I include more than one lowest numbers. 

## Part 2 - Researching Commands

**grep -c**
 directory:
 ```
$ grep -c "good" technical
grep: technical: Is a directory
0

$ grep -c "good" technical/biomed/1468-6708-3-1.txt
6
```
grep -c is going to find the number of lines that matches the given string/pattern. For directory, it's not useful. For a file,"1468-6708-3-1.txt" in  technical/biomed/ for example, it has 6 lines of "good". 
**reference : https://www.geeksforgeeks.org/grep-command-in-unixlinux/**

 **grep -n**
 ```
$ grep -n "good" technical
grep: technical: Is a directory

$ grep -n "good" technical/biomed/1468-6708-3-1.txt
83:          self-rated health (is your health excellent, very good,
84:          good, fair, or poor?) (EVGFP) which was collected every 6
96:          good, or good health (were 'healthy'). YHL ranges from 0
97:          (for persons who were never in excellent, very good, or
98:          good health) to 7 years (for persons who were healthy
425:        EVGFP Is your health excellent, very good, good, fair or
```
grep -n is to show the line number of file with the line matched. For a directory, we can't use this command. For a file, "1468-6708-3-1.txt" in  technical/biomed/ for example, it lists 6 lines that include "good".
**reference : https://www.geeksforgeeks.org/grep-command-in-unixlinux/**

**grep - o**
```
$ grep -o "good" technical
grep: technical: Is a directory
 
$ grep -o "good" technical/biomed/1468-6708-3-1.txt
good
good
good
good
good
good
good
good
```
grep -o displays only the matched string by using the -o option. The example above shows that there are 6 lines that include good. Now, we are lising good in these lines. Similarly, this command doesn't work for directory. 
**reference : https://www.geeksforgeeks.org/grep-command-in-unixlinux/**

**grep -l**
```
$ grep -l "good" technical
grep: technical: Is a directory

     
$ grep -l "good to" technical/biomed/*
technical/biomed/1471-2105-2-8.txt
technical/biomed/1471-2407-3-18.txt

```
grep -l display the files that contains the given string/pattern. This command also doesn't work for directory. For files, we are examine all txt files in  technical/biomed/ whether they contain "good to". And we find that technical/biomed/1471-2105-2-8.txt and technical/biomed/1471-2407-3-18.txt have "good to". Thus, we have these two files listed in the terminal. 
**reference : https://www.geeksforgeeks.org/grep-command-in-unixlinux/**

