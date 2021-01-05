#Bear Map

##Porject2AB
#You are going to building a system very similar to Google Maps.

#This system will have three major parts:

#Project 2AB: Building 2 useful data structures (ExtrinsicPQ and KdTree) & Building an AI for solving problems (AStarSolver).
#Project 2C: Combining all the pieces into a web-browser mapping application called BearMaps.

##Proj2A - Extrinsic MinPQ

#For this part of the project, you will build an implementation of the ExtrinsicMinPQ interface. Ultimately, this will be useful for implementing the AStarSolver class that will be described in the HW4 spec.

#The ExtrinsicMinPQ interface operations are described below:

#public boolean contains(T item): Returns true if the PQ contains the given item.
#public void add(T item, double priority): Adds an item of type T with the given priority. Edit (3/13/19): If the item already exists, throw an IllegalArgumentException. You may assume that item is never null.
#public T getSmallest(): Returns the item with smallest priority. If no items exist, throw a NoSuchElementException.
#public T removeSmallest(): Removes and returns the item with smallest priority. If no items exist, throw a NoSuchElementException.
#public int size(): Returns the number of items.
#public void changePriority(T item, double priority): Sets the priority of the given item to the given value. If the item does not exist, throw a NoSuchElementException.
#There are four key differences between this abstract data structure and the MinPQ ADT.

#The priority is extrinsic to the object. That is, rather than relying on some sort of comparison function to decide which item is less than another, we simply assign a priority value using the add or changePriority functions.
#There is an additional changePriority function that allows us to set the extrinsic priority of an item after it has been added.
#There is a contains method that returns true if the given item exists in the PQ.
#There may only be one copy of a given item in the priority queue at any time. Edit (3/13/19): To be more precise, if you try to add an item that is equal to another according to .equals, the PQ should throw an exception.
#If there are 2 items with the same priority, you may break ties arbitrarily.

##ArrayHeapMinPQ
#Your job for this part of the project is to create the ArrayHeapMinPQ class, which must implement the ExtrinsicMinPQ interface.

#Your ArrayHeapMinPQ is subject to the following rules:

#One of your instance variables must be a min heap, and this min heap must be represented as either an array or a java.util.ArrayList, as described in lecture 20 (tree representation 3 or 3b). It is OK to have additional private variables.
#You may only import from the java.util and org.junit libraries. You’re not required to import anything. Edit (3/16/19): You may also import the Stopwatch and StdRandom classes from the Princeton libraries.
#Your getSmallest, contains, size and changePriority methods must run in O(log(n)) time. Your add and removeSmallest must run in O(log(n)) average time, i.e. they should be logarithmic, except for the rare resize operation. For reference, when making 1000 queries on a heap of size 1,000,000, our solution is about 300x faster than the naive solution. Iterating over your entire min heap array (or ArrayList) for any of these methods will be linear time and thus too slow!
#If using an array representation for your min heap, make sure to increase the array size by a multiplicative factor when resizing, otherwise add will be linear time on average, not log time. If using a heap ordered ArrayList, you don’t need to worry about this requirement as ArrayLists automatically resize their underlying array.
#If using an array representation for your min heap, it should never be more than (edit: 3/13/19) approximately 3/4s empty. If using a heap ordered ArrayList, you don’t need to worry about this requirement as ArrayLists automatically resize their underlying array.
#You may not add additional public methods. Private methods or “package private” methods are fine. See the testing section below for a description of what “package private” means.
#Note: We have not discussed how you should implement the changePriority method invent this yourself. 

##Testing Proj2A

#You should write your tests in a file called ArrayHeapMinPQTest.java. This file should be part of the BearMaps package.

#We encourage you to primarily write tests that evaluate correctness based on the outputs of methods that provide output (e.g. getSmallest and removeSmallest). This is in contrast to tests that try to directly test the states of instance variables (see tip #3 below). For example, to test changePriority, you might use a sequence of add operations, a changePriority call, and then finally check the output of a removeSmallest call. This is (usually) a better idea than iterating over your private variables to see if changePriority is setting some specific instance variable.

#Write tests for functions in the order that you write them. You might even find it helpful to write the tests first, which can help elucidate the task you’re trying to solve. Since each function may call previously written functions, this helps ensure that you are building a solid foundation.


##Proj2B - K-d Tree
#For this part of the project, your primary goal will be to implement the KDTree class. This class implements a k-d tree.

#This will ultimately be useful in your BearMaps application. Specifically, a user will click on a point on the map, and your k-d tree will be used to find the nearest intersection to that click.

#Before you create your efficient KDTree class, you will first create a naive linear-time solution to solve the problem of finding the closest point to a given coordinate. The goal of creating this class is that you will have a alternative, albeit slower, solution that you can use to easily verify if the result of your k-d tree’s nearest is correct. Create a class called NaivePointSet The declaration on your class should be:

    public class NaivePointSet implements PointSet {
        ...
    }
#Your NaivePointSet should have the following constructor and method:

#public NaivePointSet(List<Point> points): You can assume points has at least size 1.
#public Point nearest(double x, double y): Returns the closest point to the inputted coordinates. This should take \(\theta(N)\) time where \(N\) is the number of points.
#Note that your NaivePointSet class should be immutable, meaning that you cannot add or or remove points from it. You do not need to do anything special to guarantee this.
  
##Part 2: K-d Tree
#Now, create the class KDTree class. Your KDTree should have the following constructor and method:

#public KDTree(List<Point> points): You can assume points has at least size 1.
#public Point nearest(double x, double y): Returns the closest point to the inputted coordinates. This should take \(O(\log N)\) time on average, where \(N\) is #the number of points.
#As with NaivePointSet, your KDTree class should be immutable. Also note that while k-d trees can theoretically work for any number of dimensions, your implementation only has to work for the 2-dimensional case, i.e. when our points have only x and y coordinates.

##Testing NaivePointSet and KDTree
#There are a number of different ways that you can construct tests for this part of the project, but we will go over our recommended approach.

#Randomized Testing

#Creating test cases that span all of the different edge cases of proj2B will be more difficult than in proj2A due to the complexity of the problem we are solving. To avoid thinking about all possible strange edge cases, we can turn towards techniques other than creating specific, deterministic tests to cover all of the possible errors.

#Our suggestion is to use randomized testing which will allow you to test your code on a large sample of points which should encompass most, if not all, of the edge cases you might run into. By using randomized tests you can generate a large number of random points to be in your tree, as well as a large number of points to be query points to the nearest function. To verify the correctness of the results you should be able to compare the results of your KDTree’s nearest function to the results to the NaivePointSet’s nearest function. If we test a large number of queries without error we can build confidence in the correctness of our data structure and algorithm.

#An issue is that randomized tests are not deterministic. This mean if we run the tests again and again, different points will be generated each time which will make debugging nearly impossible because we do not have any ability to replicate the failure cases. However, randomness in computers is almost never true randomness and is instead generated by pseudorandom number generators (PRNGs). Tests can be made deterministic by seeding the PRNG, where we are essentially initializing the PRNG to start in a specific state such that the random numbers generated will be the same each time. We suggest you use the class Random which will allow you to generate random coordinate values as well as provide the seed for the PRNG. More can be found about the specifications of this class in the online documentation.


##A* solver
#In this assignment, we’ll be building an artificial intelligence that can solve arbitrary state space traversal problems. Specifically, given a graph of possible states, your AI will find the optimal route from the start state to a goal state.

#This AI will be able to solve a wide variety of problems, including finding driving directions, solving a 15 puzzle, and finding word ladders.

#Your AI will use the technique from lecture called A*.
#AStarGraph


##Project 2C: Bear Maps, version 4.0
#Introduction
#This project was originally created by former TA Alan Yao (now at AirBNB). It is a web mapping application inspired by Alan’s time on the Google Maps team and the OpenStreetMap project, from which the tile images and map feature data was downloaded.

#In this project, you’ll build the “smart” pieces of a web-browser based Google Maps clone. You’ve actually already done much of this work in proj2ab and hw4.

#Unlike prior 61B projects, you’ll start with a gigantic code base that somebody else wrote. This is typical in real world programming, where you don’t have the luxury and freedom that comes with starting from totally blank files. As a result, you’ll need to spend some time getting to know the provided code, so that you can complete the parts that need completing.

#By the end of this project, with some extra work, you can even host your application as a publicly available web app. More details will be provided at the end of this spec.

##Overview
##Application Structure
#Your job for this project is to implement the back end of a web server. To use your program, a user will open an html file in their web browser that displays a map of the city of Berkeley, and the interface will support scrolling, zooming, and route finding (similar to Google Maps). We’ve provided all of the “front end” code. Your code will be the “back end” which does all the hard work of figuring out what data to display in the web browser.

#At its heart, the project works like this: The user’s web browser provides a URL to your Java program, and your Java program will take this URL and generate the appropriate output, which will displayed in the browser. 
#The job of the back end server (i.e. your code) is take this URL and generate an image corresponding to a map of the region specified inside the URL (more on how regions are specified later). This image will be passed back to the front end for display in the web browser.

#You’ll notice the web address that we saw in the setup instructions is strange, starting with localhost:63365 or similar instead of something like www.eecs.berkeley.edu. localhost means that the website is on your own computer, and the “63365” refers to a port number, something that you’ll learn about in a future class.

##Parts of this Project
#There are 5 parts of this project. The first two are required, the third is gold points, and the last two are completely optional. The parts are as follows:

#Part I, Map Rastering: Given coordinates of a rectangular region of the world and the size of the web browser window, provide images of the appropriate resolution that cover that region.
#Part II, Routing: Given a start and destination latitude and longitude, provide street directions between those points.
#Part III, Autocomplete: Given a string, find all locations that match that string. Useful for finding, e.g. all the Top Dogs in Berkeley.
#Part IV, Written Directions: Augment the routing from Part II to include written driving directions.
#Part V, Above and Beyond: Do anything else that seems fun.
Of the two required parts, Part I is much longer that Part II. It is not as difficult as other programming projects you’ve done in the past, but it may take you several hours to understand what to do and how to get to get started. Once you understand what you’re supposed to do (see next section), as a rough estimate, I’d guess the coding is somewhat easier than proj2b.

##project description link: https://sp19.datastructur.es/materials/proj/proj2c/proj2c
