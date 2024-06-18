# top-knights

Your task is to build a function knightMoves that shows the shortest possible way to get from one square to another by outputting all squares the knight will stop on along the way.

You can think of the board as having 2-dimensional coordinates. Your function would therefore look like:

`knightMoves([0,0],[1,2]) == [[0,0],[1,2]]`

Sometimes there is more than one fastest path. Examples of this are shown below. Any answer is correct as long as it follows the rules and gives the shortest possible path.
`knightMoves([0,0],[3,3]) == [[0,0],[2,1],[3,3]]` or `knightMoves([0,0],[3,3]) == [[0,0],[1,2],[3,3]]`
`knightMoves([3,3],[0,0]) == [[3,3],[2,1],[0,0]]` or `knightMoves([3,3],[0,0]) == [[3,3],[1,2],[0,0]]`
`knightMoves([0,0],[7,7]) == [[0,0],[2,1],[4,2],[6,3],[4,4],[6,5],[7,7]]` or `knightMoves([0,0],[7,7]) == [[0,0],[2,1],[4,2],[6,3],[7,5],[5,6],[7,7]]`


1. Think about the rules of the board and knight, and make sure to follow them.
2. For every square there is a number of possible moves, choose a data structure that will allow you to work with them. Don’t allow any moves to go off the board.
3. Decide which search algorithm is best to use for this case. Hint: one of them could be a potentially infinite series.
5. Use the chosen search algorithm to find the shortest path between the starting square (or node) and the ending square. Output what that full path looks like, e.g.:

## Final thoughts

I was able to find a picture in one of the TOP discord channels that depicted how to traverse the possible moves from a given position. It was helpful to know that it doesn't matter from which position you take to get to the next position, because the weight of each move is 1. This means that A -> B -> C or A -> D -> C is the same distance to get to C. This works out because we can make those previous squares visited and we won't run into a cyclic situation.

I also add a mark to all the next positions with the current position so I can backtrack from the end position later on.

Once I reach the end position, I break out of the BFS `while` loop and I can backtrack from the end position!

## Initial Thoughts

### Use nodes?
- Does it make sense to make each square a node?
  - I'm thinking along these lines because a node that was visited should not be visited again
    - I can store a `visited` property to a node
    - But what will I do when multiple nodes have the same neighbor?
  - How would I assign nodes to be a node's neighbors?

### Use the nodes in a tree?
- Do I need to use a tree to find nodes and to attach them as neighbors or possible moves to another node?
- I can create a tree just for the purposes of finding nodes that are possible moves for a specific node
- I can either do this for every single node beforehand, or do it as needed?
  - If I do it beforehand, then it may be quicker since I wouldn't have to find all the nodes when I do my BFS to find the end position
  - If I do it as I go, then it
```javascript
currentPosition = chessTree.find([1,2]);

possibleMoves = [[3,1], [2, 4], ..., [0, 0]]

for (let move in possibleMoves) {
  let knightMove = chessTree.find(move)
  currentPosition.moves.push(knightMove);
}
```

### The knights

- From a knight's possible positions, if two nodes are two spaces from each other, they share at least one neighbor, so it doesn't matter who gets there first because they would've gone down the same tree. This means that it's okay for a node to be visited before another has had the chance to visit that node. This makes the "visited" property helpful so we don't end up in a cyclic loop.

### Examples

- `knightMoves([0,0],[3,3]) == [[0,0],[2,1],[3,3]]` or `knightMoves([0,0],[3,3]) == [[0,0],[1,2],[3,3]]`
- `knightMoves([3,3],[0,0]) == [[3,3],[2,1],[0,0]]` or `knightMoves([3,3],[0,0]) == [[3,3],[1,2],[0,0]]`
  - How do we get to this answer? 
  - Starting from 3,3, we can check the possible routes:
    - 1,2 check out this route
    - 1,4
    - 2,1
    - 4,1
    - 2,5
    - 5,2
    - 5,4
    - 1,2
  - We go to 1,2, we can check the possible routes:
    - 0,0 correct answer; 2 moves
    - 3,3 eliminated because we've been here
    - 3,1 check out this route
    - 1,3
    - 0,4
  - We go to 3,1 to check the following:
    - 1,3 check this
    - etc.
- `knightMoves([0,0],[7,7]) == [[0,0],[2,1],[4,2],[6,3],[4,4],[6,5],[7,7]]` or `knightMoves([0,0],[7,7]) == [[0,0],[2,1],[4,2],[6,3],[7,5],[5,6],[7,7]]`
