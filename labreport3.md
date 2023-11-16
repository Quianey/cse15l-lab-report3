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

**find -type**
 ```
$ find technical -type d
technical
technical/biomed
technical/911report

$ find technical/911report -type f
technical/911report/preface.txt
technical/911report/chapter-13.5.txt
technical/911report/chapter-13.2.txt
technical/911report/chapter-13.4.txt
technical/911report/chapter-6.txt
technical/911report/chapter-8.txt
technical/911report/chapter-13.1.txt
technical/911report/chapter-13.3.txt
technical/911report/chapter-2.txt
technical/911report/chapter-3.txt
technical/911report/chapter-9.txt
technical/911report/chapter-12.txt
technical/911report/chapter-7.txt
technical/911report/chapter-11.txt
technical/911report/chapter-5.txt
technical/911report/chapter-1.txt
technical/911report/chapter-10.txt
```
find -type is going to display files and directories. ```-type d``` is to display directories. ```-tpye f``` is to display files. 
**reference : [https://www.geeksforgeeks.org/grep-command-in-unixlinux/](https://www.redhat.com/sysadmin/linux-find-command)**

 **find -mtime -n**
 ```
$ find technical/911report -mtime -7
technical/911report
technical/911report/preface.txt
technical/911report/chapter-13.5.txt
technical/911report/chapter-13.2.txt
technical/911report/chapter-13.4.txt
technical/911report/chapter-6.txt
technical/911report/chapter-8.txt
technical/911report/chapter-13.1.txt
technical/911report/chapter-13.3.txt
technical/911report/chapter-2.txt
technical/911report/chapter-3.txt
technical/911report/chapter-9.txt
technical/911report/chapter-12.txt
technical/911report/chapter-7.txt
technical/911report/chapter-11.txt
technical/911report/chapter-5.txt
technical/911report/chapter-1.txt
technical/911report/chapter-10.txt
```
grep -n is to find a file or directory I modified last n days. In this example, I used n = 7, which means I modified last 1 week.
**reference : [https://www.geeksforgeeks.org/grep-command-in-unixlinux/](https://opensource.com/article/21/9/linux-find-command)**

**find -iname**
```
$ find -iname "*chapter-12*"
/home/docsearch/technical/911report/CHAPTER-12.txt
/home/docsearch/technical/911report/chapter-12.txt

$ find -iname "*report*"
./technical/911report
./technical/911REPORT
```
I can broaden your search by making it case-insensitive with the -iname. When I use -iname, I can ingore upper case and lower case and find any file or directroy which match the name after -iname. 
**reference : [https://www.geeksforgeeks.org/grep-command-in-unixlinux/](https://opensource.com/article/21/9/linux-find-command)**

**find -empty**
```
find technical -empty
technical/biomed/1476-4598-2-28.txt
technical/biomed/1476-4598-2-25.txt
technical/biomed/1476-0711-2-3.txt
technical/biomed/1475-9268-1-2.txt
technical/biomed/1476-5918-1-2.txt
technical/biomed/1475-925X-2-3.txt
technical/biomed/1476-069X-2-9.txt
technical/biomed/1475-925X-2-11.txt
technical/biomed/1476-4598-2-20.txt
technical/biomed/1475-4924-1-5.txt
technical/biomed/1476-069X-2-4.txt
technical/biomed/1475-9268-1-1.txt
technical/biomed/1476-511X-1-2.txt
technical/biomed/1475-4924-1-10.txt
technical/biomed/1475-925X-2-12.txt
technical/biomed/1476-069X-1-3.txt
technical/biomed/1476-4598-2-24.txt
technical/biomed/1476-4598-1-8.txt
technical/biomed/1476-9433-1-2.txt
technical/biomed/1476-4598-2-2.txt
technical/biomed/1476-4598-1-5.txt
technical/biomed/1475-2891-1-2.txt
technical/biomed/1475-9276-1-3.txt
technical/biomed/1476-069X-2-2.txt
technical/biomed/1475-2891-2-1.txt
technical/biomed/1475-925X-2-10.txt
technical/biomed/1476-4598-2-22.txt
technical/biomed/1476-4598-1-6.txt
technical/biomed/1475-925X-2-1.txt
technical/biomed/1476-511X-2-3.txt
technical/biomed/1476-0711-2-7.txt
technical/biomed/1476-511X-2-2.txt
technical/biomed/1476-069X-2-7.txt
technical/biomed/1476-4598-2-3.txt
technical/biomed/1476-072X-2-4.txt
technical/biomed/1476-4598-2-1.txt
technical/biomed/1476-4598-1-3.txt
technical/biomed/1476-072X-2-3.txt
technical/biomed/1475-925X-2-6.txt
technical/911report/CHAPTER-12.txt
technical/911REPORT

```
find -empty finds all empty folders and files in the entered directory or sub-directories. In the example, I have many emtpy txt files and one empty folder--911REPORT. 
**reference : [https://www.geeksforgeeks.org/grep-command-in-unixlinux/](https://www.geeksforgeeks.org/find-command-in-linux-with-examples/)**

