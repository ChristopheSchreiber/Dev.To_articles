Everybody knows how to write POJO (Plain Old Java Objects). Moreover, our modern IDEs handle this repetitive and not so rewarding job by generating the boilerplate code such as accessors, constructors and often overriden methods such as toString, equals or hashCode.
By the way, which developer can tell he never had long debates with his team about how to write a satisfying equals or hashCode method, who never received a notification from his static code analysis tool about an inconsistency between the fields used in these methods' implementation ?
The Lombok project's goal is to make developer's life easier by handling this dirty work and cleaning the POJOs from all this boilerplate code that doesn't have much value and that, more importantly, loses the important information, the business code, in the middle of technical layers. How does this work in a nutshell : Lombok integrates with your favorite IDE and will generate this code automatically without showing it to you. You will only see fields definition, even though all accessors and "utility" methods will be available. This magic is performed by using one or more annotations.
We will also talk about some other repetitive and/or verbose tasks that Lombok can handle for us.

##Let's start !
Installing Lombok is easy. You can either download its self-executable jar on the official site and execute it, or install its plugin in your IDE (I personnaly use IntelliJ, so I've used this solution), then add lombok.jar to your Java project's classpath. If you are using Maven to handle your build and dependencies, add the following dependency in your pom.xml :
`<dependency>
<groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
<version>1.16.8</version>
<scope>provided</scope>
</dependency>`


Once we are ready to work, let's create a simple POJO :

![Pojo](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/image1.png)

As expected, the compiler is note very satisfied with our code... Let's just add Lombok's @Data annotation to our class definition :

![Annotation Data](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/image2.png)
Problem solved ! But what is really interesting is that our POJO now has much more to offer :

![Methods](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/image3.png)

We can quickly test the results :

![Testing POJO](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/image4.png)

We now have a pretty human readable toString method without having to write several lines of code !

##Special needs
This was the simpler use case of Lombok, but I'm sure some of you are thinking that it's a bit limited and that it won't work for your special cases. Fear not ! Lombok allows you to customise everything it's generating.
Instead of using the @Data to generate all the code, Lombok provides annotations for each of its features. For instance, if we want to expose accessors for only some fields, or with a specific visibility, we can achieve it by using the @Getter and @Setter annotations and their parameters on the desired attributes.

![Getters and Setters](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/image5.png)

Regarding the features we've seen earlier, we can annotate the class to generate the toString, equals and hashCode methods, and for each method we can specify the attributes to include or exclude for code generation :

![Custom hashcode and toString](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/image6.png)

We can also generate a private constructor and its associated factory :

![Factory](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/image7.png)

Another interesting annotation provided by Lombok is @NonNull. It allows to automatically check that a parameter of a setter or a constructor is not null, and throw an exception otherwise :

![Non null](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/image8.png)

![Exception](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/04/image9.png)

##Keeping things simple is important !
Lombok also contains more annotations, but I find them less interesting.
For instance, @CleanUp allows to automatically call the close methode of a variable defined inside a try/catch block. It can be useful for those who are still using Java 6 and can't enjoy the try with resourses introduced in JDK 7.
The @Synchronized annotation allows to define locks. I personaly prefer defining locks using the standard synchronized keyword that every Java developer knows.
The last annotation I want to talk about is the trickies : @SneakyThrows allows throwing an unchecked exception inside the body of a methode without specifying it inside the method signature. It can mask potential errors to the developer and so can be very error prone, I don't recommend using it.

##Final thoughts
As a conclusion, I think Lombok is an interesting tool to simplify the writing of the POJO. It is not a killer tool, but having less boilerplate code to maintain is a good thing to me, it helps making the business information more visible.
The technical annotations of Lombok are less interesting according to me, and they can even be quite dangerous from my point of view. Despites these few drawbacks, I'm ready to give Lombok a try inside my personal projects.

You can find the code snippets of this article on my [my Github repository](https://github.com/ChristopheSchreiber/LombokPOC)
