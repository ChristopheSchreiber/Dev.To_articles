Writing a list partioning function is a classic programming interview exercise. I'd like to show you a solution to this problem, written in Kotlin. My solution will make use of tail recursive recursion, a feature of the language that is very interesting and that we have missed for so long in Java.
I will proceed step by step using Test Driven Development (TDD).

## Prerequisites
If your are working with IntelliJ IDEA, Kotlin is compatible out of the box since version 15 of the IDE. If you are using another IDE, please follow the instructions on how to get your environment up and running on [Kotlin's tutorial page](https://kotlinlang.org/docs/tutorials/).

## Explanation of the exercise
The problem is very simple: we want to write a method that allows to partition a list into a list of lists of a given size (called the partition size).
For instance, the list made of {1, 2, 3, 4, 5} partitioned with a size of 2 will be { {1, 2}, {3, 4}, {5} }.
Of course, list size should be strictly greater than 0, otherwise this is a non-sense call. The method should be able to handle lists of any type, like lists of strings, lists of integers...

## First step : a failing test
As always in TDD, I'm first going to write a failing test.
```kotlin
class ListPartitionerKtTest {

    @Test
    fun empty_list_should_return_an_empty_list() {
        assertEquals(emptyList<List<String>>(), emptyList<String>().myPartition())
    }
}
```
As you can see, I've chosen to implement my partitioning method as a myPartition function extension for Kotlin List type. I've chosen the name myPartition to avoid confusion with the existing List.partition method (which has not the same purpose).
Let's make this test pass:
```kotlin
fun List<String>.myPartition() = emptyList<List<String>>())
```

## Being generic
Now, let's implement one of the exercise's constraint, by handling multiple types of list:
```kotlin
@Test fun should_also_handle_integers() {
    assertEquals(emptyList<List<Int>>(), emptyList<Int>().myPartition())
```
We have to change our implementation so that it becomes generic:
```
fun <T> List<T>.myPartition() = emptyList<List<T>>()
```

## Declaring a partition size
Our tests are passing, but the signature of our myPartition function is incorrect regarding specification: we have to be able to choose the partition size. So, let's write a failing test:
```kotlin
@Test fun should_accept_different_partition_size() {
    assertEquals(emptyList<List<Int>>(), emptyList<Int>().myPartition(3))
}
```
Obviously, the code doesn't compile, so let's add a parameter to myPartition, with a default value to avoid having to modify our previous tests:
```kotlin
fun <T> List<T>.myPartition(partitionSize: Int = 2) = emptyList<List<T>>()
```

## Checking the partition size
Now that we have a partition size parameter, we have to check that it is a valid argument. Partition size must be stritly greater than 0.
```kotlin
@Test(expected = IllegalArgumentException::class) fun partition_size_should_not_be_negative() {
    emptyList<Int>().myPartition(-1)
}
@Test(expected = IllegalArgumentException::class) fun partition_size_should_be_strictly_greater_than_0() {
    emptyList<Int>().myPartition(0)
}
```
We have several things to do to fix our implementation:
* Add a control on the parameter and throw an exception
* Since we are going to write a full method body, we'll need to write an explicit type return and a return statement
```kotlin
fun <T> List<T>.myPartition(partitionSize: Int = 2) : List<T> {
    if (partitionSize < 1) throw IllegalArgumentException("Partition sizz should be strictly greater than 0, ${partitionSize} is invalid")
    return emptyList<List<T>>()
}
```
Our test is passing, so let's refactor a bit to avoid having a long line of exception initializing:
```kotlin
fun <T> List<T>.myPartition(partitionSize: Int = 2) : List<List<T>> {
    validatePartitionSize(partitionSize)
    return emptyList<List<T>>()
}

private fun validatePartitionSize(partitionSize: Int) {
    if (partitionSize < 1) throw IllegalArgumentException("Partition sizz should be strictly greater than 0, ${partitionSize} is invalid")
}
```
Everything is still green, so let's move forward.

## Partioning a list smaller than partition size
We'll write writing the actual list partitioning. First step is easy: if the list is smaller than the partition size, we'll just have to return a list containing the list itself:
```kotlin
@Test fun given_list_smaller_than_partition_size_should_return_the_a_list_containing_source_list() {
    val list = listOf(1)
    assertEquals(listOf(list), list.myPartition(2))
}
```
As always in TDD, let's write the most simple code to make this test pass:
```kotlin
fun <T> List<T>.myPartition(partitionSize: Int = 2) : List<List<T>> {
    validatePartitionSize(partitionSize)
    return listOf(this)
}
```
We can add the case of a list with exactly the partition size:
```kotlin
@Test fun given_list_size_equals_to_partition_size_should_return_the_a_list_containing_source_list() {
    val list = listOf(1, 2)
    assertEquals(listOf(list), list.myPartition(2))
}
```
No code to write since the test is passing !

## Let's (finally) partition a list!
The next test is obvious: we pass a list of 3 elements with the default partition size (2), and we expect to have a list containing two elements :
* a list containing the first 2 elements
* a list containing the third element
```kotlin    
@Test fun given_1_2_3_and_partition_size_2_should_return_1_2_and_3() {
    val list = listOf(1, 2, 3)
    assertEquals(listOf(listOf(1, 2), listOf(3)), list.myPartition(2))
}
```
Again, let's implement the minimum code to fix this test:
```kotlin
fun <T> List<T>.myPartition(partitionSize: Int = 2) : List<List<T>> {
    validatePartitionSize(partitionSize)
    if (this.size <= partitionSize)
        return listOf(this)
    else
        return listOf(this.take(partitionSize), takeLast(this.size - partitionSize))
}
```
The test passes, but this looks ugly, so let's refactor it using Kotlin's when operator:
```kotlin
fun <T> List<T>.myPartition(partitionSize: Int = 2) : List<List<T>> {
    validatePartitionSize(partitionSize)
    return when {
        this.size <= partitionSize -> listOf(this)
        else -> listOf(this.take(partitionSize), takeLast(this.size - partitionSize))
    }
}
```
We can add a test that should already pass: the case of a list of size 4, with a partition size of 2:
```kotlin
@Test fun given_1_2_3_4_and_partition_size_2_should_return_1_2_and_3_4() {
    val list = listOf(1, 2, 3, 4)
    assertEquals(listOf(listOf(1, 2), listOf(3, 4)), list.myPartition(2))
}
```
All green, so we don't change anything :)

## Partitioning a list several times
Our previous tests were involving only one partitioning, we will now complicate things a bit and move towards a real partitioning implementation:
```kotlin
@Test fun given_1_2_3_4_5_and_partition_size_2_should_return_1_2_and_3_4_and_5() {
    val list = listOf(1, 2, 3, 4, 5)
    assertEquals(listOf(listOf(1, 2), listOf(3, 4), listOf(5)), list.myPartition(2))
}
```
The simplest way to make this test pass is to add a bit of recursion to our implementation:
```kotlin
fun <T> List<T>.myPartition(partitionSize: Int = 2) : List<List<T>> {
    validatePartitionSize(partitionSize)
    return when {
        this.size <= partitionSize -> listOf(this)
        else -> listOf(this.take(partitionSize))
         + takeLast(this.size - partitionSize).myPartition(partitionSize)
    }
}
```
Aplying the myPartition recursively to the last elements of the list and adding it to the result list allows our test to pass. We now have a working list partitioning method !

## The dangers of recursion
Of course, any programmer that has played with recursive calls will have spotted the weakness of our implementation: with big lists, we will surely be facing a StackOverflowException error.
Let's add a test to test the limits of our method:
```kotlin
@Test fun should_handle_big_lists() {
    val list = getListOfSize(1000)
    list.myPartition(2)
}

private fun getListOfSize(size: Int): List<Int> {
    val range = 1..size
    var list = emptyList<Int>()
    for (i in range) list = list + i
    return list
}
```
Apparently, our big list is not big enough, our test is passing. Let's use a real big list:
```kotlin
    @Test fun should_handle_really_big_lists() {
        val list = getListOfSize(100000)
        list.myPartition(2)
    }
```
We are finally facing our stack overflow error, nice !
In order to fix this test, we are going to use the tail recursion optimization. This will allow the compiler to reuse the last stack element instead of creating a new one, and so the stack overflow error will disappear.
To implement tail recursion, our code must respect the following constraints:
* the last call of the function (the return statement) should be the recursive call
* we have to indicate the Kotlin compiler that it should use the tail recursive optimization by using the *tailrec* keyword in our function definition
Tail recursion is typically done using a private method, that will take the current parameters of our call (here, the list and partition size) and the result of all previous steps, usually called the accumulator.
Let's implement this for our partitioning method:
```kotlin
fun <T> List<T>.myPartition(partitionSize: Int = 2): List<List<T>> {
    validatePartitionSize(partitionSize)
    return myRecursivePartition(this, partitionSize, emptyList())
}

private tailrec fun <T> myRecursivePartition(list : List<T>, partitionSize: Int = 2,
     accumulator : List<List<T>>): List<List<T>> {
    return when {
        list.isEmpty() -> accumulator
        list.size <= partitionSize -> accumulator + listOf(list)
        else -> myRecursivePartition(list.takeLast(list.size - partitionSize),
                partitionSize,
                accumulator + listOf(list.take(partitionSize)))
    }
}
```
Our myPartition function is now just a call to the recursive function with the correct parameters:
* the current list
* the partition size
* the initial state of the accumulator: an empty list of lists

The recursive method has 3 distinct return conditions:
* if the current list is empty, that means we have finished the partitioning, the result is then the accumulator, which has been computed step by step
* if the current list's size is smaller or equals to the partition size, we return the accumulator with the addition of the list. This is also a terminal operation
* the last case is where the recursion occurs: we call the same method with the remaining elements, and we add the current partition to the accumulator

And our test with a very big list is now passing !
Tail recursion is a very useful tool supported by functional languages such as Scala, but many Java programmers don't use it often since Oracle's language doesn't currently support this optimization.
Thanks to Kotlin, we can now use this tool :)
I hope this article was useful to you. Don't hesitate to send me feedbacks if you think this implementation can be improved or if you need more information about this example.
You can find the code on [my Github](https://github.com/ChristopheSchreiber/KotlinListPartitioner).