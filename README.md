## About
* Pacman's behavior implementation using BFS, DFS, UCS, A* Search on AI class

## Before start
* The game used here was created as a course material at UC Berkeley and many AI classes are using this material.
* In response to the creator's request not to disclose the full answer source, I will only explain the source I implemented.

## Names are different, but they are almost the same
+ Most search algorithm is based on this search algorithm.
+ Before going deeper, we need to understand this basic concept.
+ The only changes throguth out the other algorithm is that how to pick next node to expand.

## General search algorithm
+ Step 1. Start with a tree that contains only the start state
+ Step 2. Pick an unexpanded fringe node n
+ Step 3. If fringe node n represents a goal state, then stop
+ Step 4. Expand fringe node n
+ Step 5. Go to 2

## What about other algorithm
+ In the general algorithm step 2, there is no priority what to pick up.
+ But those algorithm below have some criteria what to pick up next.
+ g(n) is the cost between the node
+ h(n) is the heuristic value from the node to the goal node

1. BFS(BREADTH-FIRST SEARCH) : pick with the smallest depth
2. DFS(DEPTH-FIRST SEARCH) : pick with the largest depth
3. UCS(UNIFORM-COST SEARCH) : pick with the smallest g(n)
4. A Star search : pick with the smallest g(n) + h(n)

## Pseudo code
+ As I built the code, I found out that all the search algorithms could be in a single function.
+ There are two different things in the function. One is which data structure will be used and the other is whether the structure can accept the cost or not.
+ Comparing pseudo code below with the general search algorithm.
```
def unifiedSearch(problem, d, type, heuristic):

    # Step 0. waitList is a data structure to be used.
    waitList = None
    if type == 'dfs':
        waitList = util.Stack()
    elif type == 'bfs':
        waitList = util.Queue()
    elif type == 'ucs' or type == 'astar':
        waitList = util.PriorityQueue()
    else:
        quit()


    # Step 1. Start with a tree that contains only the start state
    # push a Node class with priority
    if type == 'dfs' or type == 'bfs':
        waitList.push(startNode)
    elif type == 'ucs' or type == 'astar':
        waitList.push(startNode, startNode.getSumF())


    while waitList.isEmpty() == False:

        # Step 2. Pick an unexpanded fringe node n
        # Which node will be popped out is determined according to the data structure
        popNode = waitList.pop()
        popState = popNode.getState()

        # Step 3. If fringe node n represents a goal state, then stop
        if problem.isGoalState(popState):
            if debug: 
                print '!!!!!!!!!! GOAL FOUNDED !!!!!!!!!!'
                print 'result path', popNode.getPath()
        
            return popNode.getPath()

        elif popState not in visited: # not visited
            if debug: print '\tvisit check: has NOT visited.'


            # Step 4. Expand fringe node n
            # getSuccessors return a tupel of 'State'
            for x in problem.getSuccessors(popState):
                xState = x[0]
                xDir = x[1]
                if xState not in visited:
                    xSumG = x[2] + popNode.getSumG()
                    xSumH = heuristic(x[0], problem)
                    xSumF = xSumG + xSumH

                    path = popNode.getPath() + [xDir]
                    newNode = Node(x, xSumF, xSumG, xSumH, path)
                    
                    if type == 'dfs' or type == 'bfs':
                        waitList.push(newNode)
                    elif type == 'ucs' or type == 'astar':
                        waitList.push(newNode, newNode.getSumF())

                    if debug: print '\t\t<< PUSH node >>', newNode 
                else:
                    if debug: print '\t\t<< DONT PUSH node >>', xState, 'already visited'

            visited.append(popState)
        else:
            if debug: print '\tvisit check: already visited.'
        

```
