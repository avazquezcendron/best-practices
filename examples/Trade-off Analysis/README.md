## Trade-off Analysis

Software engineering is a vast and complex topic, so usually almost every problem has several approaches that you take to solve it. For example, there are at least [5 ways to clone an object](https://levelup.gitconnected.com/5-ways-to-clone-an-object-in-c-d1374ec28efa) or [5 ways to implement a repository pattern in C#](https://levelup.gitconnected.com/5-ways-to-implement-repository-pattern-in-c-e12565e4d4a2).

While each of the few available approaches can solve a coding problem, you should not just pick one at random or the most fancy one. You should perform a trade-off analysis because it is the only reliable way to find the best solution for a given problem.

Let’s see how this can work in practice.

Imagine, that you created two simple classes with some logic inside:

``` c#
public class ReportGenerator
{
    public void Generate()
    {
        //Generate report
        //Save the report somewhere
    }
}

public class ReportSender
{
    public void Send()
    {
        //Load report
        //Send report to email
    }
}

```

Now you are faced with the next task — to make the two classes work together, because the report should be sent immediately after its creation. This is where you should collect different approaches to do that task and perform trade-off analysis before starting to write the code.

Trade-off analysis is the process of writing down all approaches to a problem and indicating the pros and cons of each.

This is what the trade-off analysis might look:

1. Create an instance of the ```ReportSenderclass``` at the end of the ```GenerateReport``` method using the new keyword and call the Sendmethod.
   
    **Pros**: The simplest approach, requiring a minimum of time to implement.

    **Cons**: Two classes will be tightly coupled because new keyword is a glue. The class ```GenerateReport``` cannot be reused separately from ```ReportSender```.Also, ```GenerateReport``` cannot be unit tested in isolation.    
2. Use dependency injection technique to inject the interface of ```ReportSenderclass``` into the constructor of ```GenerateReportclass``` and call ```Send``` method at the end of ```Generatemethod```.   
   
    **Pros**: Two classes are loosely coupled. Both classes can be tested in isolation.

    **Cons**: What if the application has dozens of classes besides ```ReportSender ```class, which need to do something with the report after it has been created? The logic of ```GenerateReport``` will be polluted with a bunch of responsibilities to notify all interested parties.
3. Use **Observer** design pattern. The ```GenerateReport``` class should define an event ```ReportGenerationCompleted``` to which each interested objects can subscribe and react appropriately.
   
    **Pros**: The list of subscribers can be changed at runtime. Also, the two classes are loosely coupled because ```GenerateReport``` even doesn’t need to know about the existence of ```ReportSender``` class or its interface.
    **Cons**: Need to remember to unsubscribe in time to avoid memory leaks.
1. Use **Mediator** design pattern…
2. Use **Event Aggregator** design pattern…


After all the approaches have been identified with pros and cons, it will be much easier to choose the only one without hesitation.

Regular trade-off analysis is what will make you a very confident and experienced software engineer, because this approach will require you to look at the problem from different angles.

</br>

[Back to Index 👆](./../../README.md#index "Go to Index")