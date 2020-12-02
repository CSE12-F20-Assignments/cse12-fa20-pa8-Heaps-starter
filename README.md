
---

# PA8: Heaps and Buffers (open)
---

**This assignment is open to collaboration.**

This assignment will teach you how to implement a Heap and how to apply it to file data.

### *Extra Credit Oppourtunity*
For this PA, if you submit early you will gain 2 extra points to your total score.
The early submission extra credit deadline is ** **Tuesday, December 8 at 11:59pm** **  
This needs to be your final submission, if you submit again after this deadline you can not receieve extra credit.   
This is a way to encourage everyone to start early and finish early! 

This PA is due on ** **Thursday, December 10 at 11:59pm** **  
The autograder for the PA will be released **Saturday, December 5**

Link to FAQ: https://piazza.com/class/kffnb3soo432qi?cid=1223  
Link to PA Video: https://canvas.ucsd.edu/courses/18854/external_tools/82  


## Getting the Code
The starter code is in the Github repository that you are currently looking at. If you are not familiar with Github, here are two easy ways to get your code.

1. Download as a ZIP folder 

    If you scroll to the top of Github repository, you should see a green button that says *Code*. Click on that button. Then click on *Download ZIP*. This should download all the files as a ZIP folder. You can then unzip/extract the zip bundle and move it to wherever you would like to work. The code that you will be changing is in the folder called *pa4-starter*.

2. Using git clone (requires terminal/command line)

    If you scroll to the top of the Github repository, you should see a green button that says *Code*. Click on that button. You should see something that says *Clone with HTTPS*. Copy the link that is in that section. In terminal/command line, navigate to whatever folder/directory you would like to work. Type the command `git clone _` where the `_` is replaced with the link you copied. This should clone the repository on your computer and you can then edit the files on whatever IDE you see fit.
    
If you are unsure or have questions about how to get the starter code, feel free to make a Piazza post or ask a tutor for help.

## Part 1: An Implementation of `Heap` (21 points)

You will create a file named `Heap.java` where you will implment the `Heap`, `maxHeapComparator` and `minHeapComparator` classes. You will implement all the methods defined in the given interface `PriorityQueue.java` in the `Heap` class as well as additional methods described below. 

### Import Statements
For `Heap.java`, you are allowed to use the following Java packages: 
```
import java.util.List;
import java.util.NoSuchElementException;
import java.util.ArrayList;
import java.util.Comparator;
```

*Do **NOT** import any additional packages!* 

### `Entry` Class
Copy the following code into your `Heap.java` file. This class represents the entries for the heap. Note, the key represents the priority. 

```
class Entry<K, V> {
    K key; // aka the _priority_
    V value;
    public Entry(K k, V v) { this.key = k; this.value = v; }
    public String toString() {
        return key + ": " + value;
    }
}
```


### `maxHeapComparator` and `minHeapComparator` Classes
In `Heap.java`, create the following classes that implement the `Comparator<Entry<String, Integer>>` interface. 

`class maxHeapComparator`: This comparator class will assume that Entries with a larger frequency have greater priority than Entries with a smaller frequency. If Entries have the same frequency, this class will use the keys of the entries i.e. the names, to assign priority. Names with a smaller alphabetical value will have higher priority. For example "abc" has high priority than "xyz". Similary "ab" has high priority than "abc"

`class minHeapComparator`: This comparator class will assume that Entries with a smaller frequency have greater priority than Entries with a larger frequency. If Entries have the same frequency, assign priority similary to how maxHeapComparator does when Entries have the same frequency.

The built-in Java `Comparator` interface has one method: 

`public int compare(Entry<String, Integer>  a, Entry<String, Integer>  b)`: takes two entries and returns an integer value that represent whether Entry `a`, takes priority over Entry `b`. A return value of 0 signifies that the priorities of `a` and `b` are equal. A negative return value will signify that `a` has less priority than `b` and a positive return value will assume the opposite. 



### `Heap` Class
#### Instance Variables
- `public List<Entry<K, V>> entries`: Instead of creating a tree of entries, we will use a list to maintain the heap structure.
- `public Comparator<Entry<K,V>> comparator`: This comparator determines whether this will be a min or max heap.


#### Constructor
Your heap implementation will have one constructor that takes a comparator as its argument. 
` public Heap(Comparator<Entry<K, V>> comparator)`: initializes the instance variables

#### Required Method Descriptions

| Method Name | Description |
|-------------|----------------------|
|`void add(K k, V v)`| Insert a new entry with the given key and value to the end of the heap. Then, `bubbleUp` so that the heap properties are not violated | 
|`Entry<K, V> poll()`| Remove and return the root element in the heap. Set the last entry in the heap to the root. Use `bubbleDown` to fix the heap after the removal. If the size is zero, throw `NoSuchElementException()`|
|`Entry<K, V> peek()`| Return the root element of the heap. If the size is zero, throw `NoSuchElementException()`  |
|`List<Entry<K,V>> toArray()`| Return the list of entries. |


 
#### Additional Method Descriptions

| Method Name | Description |
|-------------|----------------------|
|`public boolean isEmpty()`| If the List of entries is empty, return true. Otherwise, return false. | 
|`public int parent(int index)`| Return the parent index. |
|`public int left(int index)`| Return the left child index. |
|`public int right(int index)`| Return the right child index. |
|`public void swap(int i1, int i2)`| Takes the index of two entries and swaps them. |
|`public void bubbleUp(int index)`| A recursive method that moves the entry at the specified `index` to a smaller index (up the tree) while maintaining the heap structure. In the case where the element is equal to the parent, you should **not** swap. |
|`public void bubbleDown(int index)`| A recursive method that moves the entry at the specified `index` to a larger index (down the tree) while maintaining the heap structure. Swap with the smaller child. If both chilren are equal and swapping is needed, **swap with the left child**. In the case where the element is equal to the smaller child, you should **not** swap. |
|`public boolean existsAndGreater(int index1, int index2)`| Returns true if the entry at index1 is greater than that at index2 (Note: Both entries at the specified indicies must exists for this to be true). Return false otherwise.  |
|`public int size()`| Returns the number of elements in `entries`. |
|`public String toString()`| Returns a string representation of the elements in `entries` (this method is helpful for debugging) |


You may find the following useful:
- [`Comparator`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Comparator.html)

Your implementation of `Heap` will be graded automatically by tests that we
provide. We’ll provide a subset of the tests in the grader.

## Part 2: Implemention of `Buffer` (9 points)

For this part, you will implement a simple buffer that reads from a csv file 10 lines at a time. The buffer will read from the csv file and hold at most 10 lines of text at any time for the program to read from. This allows the program to load a subsection of names and frequencies into the heap periodically rather than all at once after reading in the entire file.

To do so, you will implement all the required methods defined below. 

### Import Statements
For `Buffer.java`, you are allowed to use the following Java packages: 
```
import java.io.File;
import java.io.IOException;
import java.util.Scanner;
```

### Instance Variables
- `String[] buffer`: An array of lines currently read from the csv file.
- `Scanner sc`: The scanner obejct that reads the csv file.
- `int currPointer`: This number determines the current line of text in the buffer than has not yet been read (like an index).


#### Required Method Descriptions

| Method Name | Description |
|-------------|----------------------|
|`public Buffer(String csv_file)`| Initialize all instance variables. Skip the first line of the csv file and begin to fill the buffer. | 
|`public void fillBuffer()`| Fill the buffer until the end of the file has been reached or the buffer is full.|
|`public String getNext()`| Return the next line of text that has not been read from the buffer. Do not forget to re-fill the buffer if all names in the buffer have been previously read. The indices in the buffer array that have been "read" should be set to null afterwards. |

Your implementation of `Buffer` will be graded automatically by tests that we provide. We’ll provide a subset of the tests in the grader.

## Part 3 - NameFinder.java (9 points)
For part 3 you will need to fill out the main method, which will have the following functionality:
1. Use the buffer from part 2 to read in a file of names and frequency
2. Declare a heap from part 1 and add each of these names into the heap given that its gender matches the inputted gender from the command line argument. 
3. Either output the contents of the heap class into a file or print the first K names with highest priority to the console. **Note:** K is passed in from the command line arguments and if K is greater than the size of the heap, print out all the names in the heap in order of priority.

The command line arguments are detailed below:

1. The filename of the csv data file that you will be reading in.
2. "true" for outputting the contents of the heap class or "false" for printing the first K names with highest priority to the console.
3. "male", "female", or "both" to represent what type of name you will be storing in the heap.
4. "max" or "min" to represent which frequency you choose to prioritize.
5. Either a file name if the second argument was "true" or an integer representing the number of names you want printed if the second argument is "false".

Here are two examples of the command needed to run this program:  
- `java NameFinder 2000_baby_names.csv true female max outputFile.txt`
- `java NameFinder 2000_baby_names.csv false both min 25`


**Format for the file output and console printing:**

For printing the names to the console, you must print the first K names with the following format for each line: "name: frequency". We've included an example below for what this should look like.

*Erics-MacBook-Pro-7:src*$ java NameFinder small_names_subset.csv false female max 100
Yamilet: 2234
Jailene: 860
Fatoumata: 826
Adrina: 388
Addalyn: 257
Landrie: 249
Charla: 213
Trinnity: 116
*Erics-MacBook-Pro-7:src*$

For outputting to a file, the first line should be the size of the heap. The following lines should represent the structure of the heap i.e. you should output the contents of the list holding the entries in the order that they are in the list. The format should also be "name: frequency" for each of these lines. For reference we've included refOutput.txt which is an example of how the output file should be formatted. refOutput.txt was generated with the following command:
* java NameFinder small_names_subset.csv true female max refOutput.txt

Notice that in this file, the entries are not necessarily sorted in ascending or descending order; they just represent a max heap. This is what we will be mainly checking for in your output file i.e. these line should either represent the structure of a min or max heap based on what command line argument you passed in to generate this file. 

**Note:** You must follow the format exactly for printing the names to the console and for ouputting the contents of the class to the file to recieve full credit.

### Adding Arguments to Eclipse

Eclipse gives you the option to add command line arguments when you run your program. Navigate to the 'Run' tab and select 'Run Configurations...' as seen in the image below.   
![](https://i.imgur.com/5PrjR5M.png)  

This will open a window where you can edit the runtime configurations. Make sure that `NameFinder` is selected on the left menu. Next, click on the 'Arguments' tab. The text box 'Program arguments:' is where you will add any arguments you would like to pass at runtime. Make sure to include a space between each argument you are passing.  
![](https://i.imgur.com/xKrZnse.png)  
 
In your main method for `NameFinder`, you will notice that it has one parameter `String[] args`. The arguments you passed will be stored in this array. 

## Testing
### Heap - HeapTest.java
In the starter code, we provide you with HeapTest which has an example of how to unit test your implementaion. **Note**: For this PA, your unit tests will be graded for completion only, however, we **strongly** encourage you to thoroughly test every public method in your class. You are required to have at least one unit test written by yourself. 



## Submitting

#### Part 1 & 2
On the Gradescope assignment **Programming Assignment 8 - code** please submit the following file structure:

* Buffer.java
* NameFinder.java
* Heap.java
* HeapTest.java

The easiest way to submit your files is to drag them individually into the submit box and upload that to Gradescope. You may submit as many times as you like till the deadline. 
 


## Scoring (40 points total)

- 21 points: implementation of `Heap` [automatically graded]
- 9 points: Buffer [automatically graded]
- 9 points: NameFinder [automatically graded]
- 1 point: HeapTest graded on completition [manually graded]





