Working on legacy software is difficult. We've all faced this issue in our career, and that's why there are so many books about working with legacy code, improving an existing code base using unit tests and clean code principles...

The lack of tests implies that code is hard to understand, hard to test, long to deliver and costly to support because of bugs that will inevitably happen in production.

For too many teams, testing legacy code leads to the same 5 phases as those of grief and loss:
- denial: "we don't need automated tests, we can still maintain our product, we know how it works"
- anger: "there was still an incident after our delivery! this product is s**t, we should rewrite it from scratch"
- bargaining: "ok, we could write some tests for new features. Let's try this for the next release"
- depression: "our code is not easily testable, the release is late because it takes too much time to write tests, and moreover they are fragile, there is nothing we can do"
- acceptation: "let's hire more people for support, this will help us since we can't add automated tests. We'll also release less often so that we can plan non regression testing phase"

Sure, testing a legacy monolith is not an easy task, even for experienced and cautious developers. In fact, testing spaghetti code is almost guaranteed to lead to poor tests, very coupled with implementation details, and every change will break them.

That's why it is important to improve the code step by step, with small refactorings that are unlikely to break existing behaviors, starting from very small steps and going on with more complex ones as the code improves:
- rename variables, methods or classes so that their purpose is clearly expressed and easier to understand
- remove unused or dead code
- testing a long method or class is difficult, so break them into smaller pieces. By simply extracting them using your IDE's refactoring capabilities, you shouldn't break anything. The code will become easier to tests, since the dependencies and the responsibility of each block will be reduced.
- once you start to have a comfortable tests harness, you can start performing more deeper refactorings (using abstractions instead of concrete implementations to favor testability and evolutiveness, using design patterns...)

Don't forget the importance of team work during these difficult times. If you can't solve a problem alone, pair programming or even mob programming will help you and your pairs. This will ensure that every developer of the team will know how to face similar problems when they'll want to improve another piece of code.

During the challenging tasks, clean code principles, SOLID principles and all the usual craftsmanship toolkit will help you, so be sure to understand them and to be able to apply them when necessary !
