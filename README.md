# LINQ to A\*

[![Build Status](https://travis-ci.org/rvhuang/linq-to-astar.svg?branch=master)](https://travis-ci.org/rvhuang/linq-to-astar)

**LINQ to A\*** is an experimental library aimed to incorporate LINQ expressions into [A\*](https://en.wikipedia.org/wiki/A*_search_algorithm) as well as other heuristic search algorithms. The goals are:

* Better human-readability and maintainability of algorithm using.
* A structural, object-oriented, unified programming model for various heuristic algorithms.

By taking advantage of the power of LINQ, the library is not only about re-implementing in C#, but also giving new ability and flexibility to the algorithms.

**All feedbacks are greatly appreciated** as the experiment is growing rapidly.

## Example

Snippet below shows the LINQ expression used to find shortest path between two positions.

```csharp
// The path to be found between two positions.
var start = new Vector2(5, 5);
var goal = new Vector2(35, 35);
var unit = 1;

// The initialization of A* algorithm
// with the factory that gets possible steps from current step.
var queryable = HeuristicSearch.AStar(start, goal, (step, lv) => step.GetFourDirections(unit));

// See description below.
var solution = from step in queryable.Except(GetObstacles())                      // 1.
               where step.X >= 0 && step.Y >= 0 && step.X <= 40 && step.Y <= 40   // 2.
               orderby step.GetManhattanDistance(goal)                            // 3.
               select step;

// Each step of the shortest path found by A* algorithm.
foreach (var step in solution)
{
    Console.WriteLine(step);
}
```

The LINQ expression consists of following clauses:

1. The `Except()` eliminates invalid steps during the process.
2. The `where` clause sets up the boundary but can also be used for checking invalid steps.
3. The `orderby` clause serves as *h(n)* (aka [Heuristic](https://en.wikipedia.org/wiki/Heuristic)) that estimates the cost of the cheapest path from *n* (current step) to the goal.

If path is found, the enumeration returns each step in deferred execution. Otherwise, no step is returned.

### Executable Examples

* [Path Finding](src/LinqToAStar.Example.PathFinding/) 
* [Solving 8-Puzzle](src/LinqToAStar.Example.EightPuzzle/)

## Supported Algorithms

|Algorithm|Factory Method|
|----------|----------|
|[A\*](https://en.wikipedia.org/wiki/A*_search_algorithm)|`AStar<TStep>()`|
|[Best-first Search](https://en.wikipedia.org/wiki/Best-first_search)|`BestFirstSearch<TStep>()`|
|[Recursive Best-first Search](http://cs.gettysburg.edu/~tneller/papers/talks/RBFS_Example.htm)|`RecursiveBestFirstSearch<TStep>()`|
|[Iterative Deepening A\*](https://en.wikipedia.org/wiki/Iterative_deepening_A*)|`IterativeDeepeningAStar<TStep>()`|

## Supported LINQ Clauses

|Clause|Remarks|
|----------|----------|
|`Select()`||
|`SelectMany()`||
|`Where()`||
|`OrderBy()`||
|`OrderByDescending()`||
|`ThenBy()`||
|`ThenByDescending()`||
|`Except()`||
|`Contains()`||
|`Reverse()`|To be implemented|

## Roadmap

|Milestone|Release Date (NuGet)|
|----------|----------|
|1.0.0 Preview|Q2 2018|
|1.0.0|Q3 2018|

## Platform

The library targets [.NET Standard 2.0](https://docs.microsoft.com/en-us/dotnet/api/?view=netstandard-2.0) currently.

## Dependencies

* [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/) (>= 4.4.0)
* [System.Drawing.Primitives](https://www.nuget.org/packages/System.Drawing.Primitives/) (>= 4.3.0)

## License

The library is licensed under MIT. Feel free to copy and use in your algorithm homework (grades not guaranteed).
