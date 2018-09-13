Builder pattern is awesome. When I discovered it, I felt that many difficulties I had with my previous codes would have vanished by using this elegant pattern, especially when dealing with complex objects that have like 20 fields. Builder pattern allows you to create such complex objects with a code that is far more expressive that calling an ugly constructor, that don't indicate much about the business represented by the object you're construction. Moreover, the created object is immutable !
What I want to show with this post is a special kind of builder that I've discovered some time ago, while I was trying to define a mix of a builder and a factory: I wanted to be able to create two types of an object (with each type inheriting from the same base class), but I wanted to have a declarative API to avoid setting useless parameters when building a specific object.

##Introducing my business
Let's consider an example that I'll use during this whole article : let's say we want to build fast food orders. My fast food is selling salads and sandwiches. Both have some attributes in common, but some are specific to a type of meal. For instance, a sandwich needs a type of bread, but this is irrelevant for a salad. So, I defined one base class FastFoodOrder and 2 subclasses Sandwich and Salad:

![Base class](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/model-2.png)
![Sandwich](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/sandwich.png)
![Salad](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/salad.png)
Attributes are simple enums:
![Attributes](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/model2-1.png)

##The classic Builder pattern
First, let's define a classic builder. For those who don't know about this pattern, it allows to create objects by using a builder class containing setters for each attribute to set, each setter returning the builder itself so that setters can be chained elegantly. When all the desired attributes have been set, the build method is called and it returns an instance of the class we want to construct.
In our case, we would have something like this class :

![Classic builder](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/builder-3.png)

Using this builder is simple :

![Using classic builder](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/using-builder.png)
The results of this call will be :
![Results Builder](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/results-builder-1.png)

As you can see, this not very satisfying: we have defined a salad with no recipe and we tried to choose bread...
A simple fix would be to validate the data inside the buildOrder method. But the validation rules will then be inside the builder implementation, and the caller will have no idea how to construct a valid order without looking in the code (or the probably outdated documentation). This issue is addressed by the checked builder pattern.

##Introducing the Checked Builder Pattern
The base idea of the checked builder pattern is to define an API inside the builder class, so that the mandatory fields and special cases are exposed directly to the caller. This allows to check the validity directly at the compilation, and to benefit from nice features like auto-completion in the IDE.
So, how does this work ? For every step of the building process, we define a simple interface containing a setter method returning the interface type of the next step. This allows to declare what step we are currently working on and define the flow of our builder.
For instance, in our example, we want to define this flow :
 * Choose between take away or eat on site
 * Choose between a salad and a sandwich
 * If we chose a sandwich, we have to choose a type of bread, if we chose a salad we don't need this step
 * Choose a base recipe (main ingredient)
 * Optionally, choose a sauce
 * Build the FastFoodOrder instance

Our first step will represent an empty order so let's create an interface to allow choosing between take away or eat on site :

![Step1](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/step1.png)
Now, we have an order with an associated order type, so we have to choose between the meal types our system can handle, ie sandwich and salad:

![Step2](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/step2-1.png)

In case of a sandwich, we have to choose its bread type, then we arrive at the same stage:

![Step3](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/sandwich-order.png)
![Step4](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/choose-meal.png)
Finally, we arrive at the last stage, where we can choose the optional ingredients (in my example, sauce is the only optional ingredient) and build our object:
![Step5](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/finalizer.png)

Now, we just have to implement all these interfaces in our builder:
![Checked Builder](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/checked-builder.png)

We also have to change the instanciation of our builder in FastFoodOrder.newOrder() so that the instance retrieved is of the type of the first step (EmptyOrder), otherwise we would be able to access all the methods of the builder, bypass the expected workflow and construct incoherent orders (although we can't stop a malicious user to cast the builder to have access to an undesired state):

![Init checked builder](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/checked_builder_init-1.png)

With this builder, we can only set the attribute relevant to the object we are creating, and we can't construct an instance without specifying all the mandatory ingredients :
![Using checked builder](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/using-checked-builder.png)

![Results checked builder](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/07/results-checked-builder.png)

##Conclusion
This special implementation of the builder pattern has many advantages, since it allows to check the consistency of the objects directly when writing code, not at runtime. This is very useful in a public API (this can be seen as some living documentation ;-) )
On the other hand, its main drawback is that it introduce a lot of interfaces to model the workflow of the construction of the object, and thus it may be hard to read or modify. So, like always, this is not a silver bullet that should be use every time you want to implement a builder, especially when handling complex business objects with many forks in their workflows, or when the workflow can change very often.
This is just another tool, that can be useful in some situations :-)
You can find the code from my example on [my Github page](https://github.com/ChristopheSchreiber/CheckedBuilderDemo)