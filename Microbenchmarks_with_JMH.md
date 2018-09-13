Today I'd like to talk about a subject that cares for me : performance. I'm going to give a quick overview of an aspect that it not often treated : micro-benchmarking. We will see how we can implement micro-benchmarking using the JMH tool.

##Why using micro-benchmarking and why JMH ?
When we are coding, we often want to have a quick estimatin of the performance of an algorithm, a utility method or any component we are writing or using. What we often do is writing some simple code in a very basic main class, calling the test we want to inspect and measuring elasped time of the call using system clock. Unfortunately, this is a very unprecise approach, and the only case when it can be a sufficient indicator is when it clearly shows very poor performance. This kind of performance testing is too different from the context in which we want to execute our production code:
*  such single measure is very error prone and can't supply reliable statistics
*  simple tests with just a few executions can't enjoy the benefits of JVM optimization, such as the JIT (Just in Time) compiler that compiles frequently executed bytecode into native code to optimize execution durations. Such optimizations can only be applied after numerous calls, so that the JIT compiler can detect possible gains.
*  it is difficult to reproduce a production environment, with multithreading concurrent access and all the impact it can have on performance
In order to adress these issues, the OpenJDK project has developed a tool dedicated to benchmarks for languages running on the JVM, called JMH. This framework aimes at executing automatically repeated executions of our code, simulating production context and collecting statistics about code performance.Of course, JMH can be used with the official JDK from Oracle, not only with an OpenJDK installation.

##Project setup
A JMH Maven archetype is available, making it very easy to setup a project using the command line:

`mvn archetype:generate
-DinteractiveMode=false
-DarchetypeGroupId=org.openjdk.jmh
-DarchetypeArtifactId=jmh-java-benchmark-archetype
-DgroupId=com.cs
-DartifactId=poc-jmh
-Dversion=1.0`

The module will contain all necessary dependencies and Maven plugins, and a default MyBenchmark class will also be created. This class will contain en empty annotated method.
Like many other Java frameworks, JMH is using annotations. Benchmark classes are similar to the unit tests classes we are writing everyday (I hope you do ! ;-) )
Executing the benchmarks (meaning executing classes annotated with @Benchmark) requires building our Maven project with the well-known command:

`mvn clean install`
Then we just have to execute the generated jar file:
`java -jar target/benchmarks.jar`

This will launch a series of iterations (continuous execution of our code during a predefined duration) and will generate a report at the end.

##Usage example
Let's consider a simple usecase everyone knows : strings concatenation inside a loop. We will compare the naive implementation using the + operator on String objects and the StringBuilder usage:
![Code](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/code.png)

![Results](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/resultats.png)
This very simple benchmark indicates the average operation numbers (execution of our benchmark code) for each method during the duration, and also other intesting statistics like min/max/average, standard deviation and the 99th percentile...

##Handling more complex cases
After implementing a very simple JMH benchmark using default execution parameters, we will see how we can customize our benchmarks. As explained in the beginning of this article, JMH aims at providing executions close to the production context. This is achieved with some parameters tuning, like:
*  the number of iterations
*  the duration of each iteration
*  the benchmark type (we can for instance measure the number of executions during a time frame or the average execution time)
*  the number of thread to be used to simulate concurrent access
*  the number of JVM instances on which we want to execute our benchmarks, and their parameters
*  the number of warmup iterations to be executed in each scenario. These iterations don't appear in the computed statistics, but this is a very useful feature when we want to have an idea of the behaviour of our code once the JIT compiler will have optimized it
This is not an exhaustive list of JMH features. If you want to go deeper, I invite you to have a look at the numerous example of its [official documentation][official documentation](http://hg.openjdk.java.net/code-tools/jmh/file/tip/jmh-samples/src/main/java/org/openjdk/jmh/samples).

##Conclusion
JMH is very useful and powerful tool to build, execute and monitor our code by using micro-benchmarks. This is a very interesting approach that completes the more traditional performance tests like load testing. It can give us very valuable data about some precise parts of our code.
Like unit tests, the more decoupled the code to be tested is, the simpler is the writing of the benchmarks, and results will be more accurate too !
You can find the sources of this article on [my Github repository](https://github.com/ChristopheSchreiber/poc-jmh)

