<h2>16.4 Tic Tac Win</h2>
<p>
     Design an algorithm to figure out if someone has won a game of tic-tac-toe. 
</p>

<h3>Solution:</h3>
<p>
    At first glance, this problem seems really straightforward. We're just checking a tic-tac-toe board; how hard could it be? It turns out that the problem is a bit more complex, and there is no single "perfect" answer. The optimal solution depends on your preferences. 
</p>
<p>
    There are a few major design decisions to consider:
</p>
<ol>
    <li>
        Will hasWon be called just once or many times (for instance, as part of a tic-tac-toe website)? If the latter is the case, we may want to add pre-processing time to optimize the runtime of hasWon. 
    </li>
    <li>
         Do we know the last move that was made? 
    </li>
    <li>
        Tic-tac-toe is usually on a 3x3 board. Do we want to design for just that. or do we want to implement it as an NxN solution?
    </li>
    <li>
         In general, how much do we prioritize compactness of code versus speed of execution vs. clarity of code? Remember: The most efficient code may not always be the best. Your ability to understand and maintain the code matters,too. 
    </li>
</ol>


<h3>
    Solution #1: If hasWon is called many times 
</h3>
<p>
    There are only 39, or about 20,000, tic-tac-toe boards (assuming a 3x3 board). Therefore, we can represent our tic-tac-toe board as an int, with each digit representing a piece (0 means Empty, 1 means Red, 2 means Blue). We set up a hash table or array in advance with all possible boards as keys and the value indicating who has won. Our function then is simply this: 
</p>

```java
    Piece hasWon(int board) { 
        return winnerHashtable[board];
    } 
```
<p>
    To convert a board (represented by a char array) to an int, we can use what is essentially a "base 3" representation. Each board is represented as 3°V0 + 31v 1 + 32v 2 + .•• + 38v 8' where vi is a 0 if the space is empty, a 1 if it's a "blue spot" and a 2 if it's a "red spot'.
</p>

```java
enum Piece { Empty, Red, Blue }; 

int convertBoardTolnt(Piece[][] board) {
    int sum = e;
    for (int i = e; i < board.length; i++) { 
        for (int j = e; j < board[i].length; j++) {
            /* Each value in enum has an integer associated with it. We 8 * can just use that. */
            int value = board[i][j].ordinal(); 
            sum = sum * 3 + value;
        } 
    }
    return sum; 
} 
```
<p>
    Now looking up the winner of a board is just a matter of looking it up in a hash table. 
</p>
<p>
    Of course, if we need to convert a board into this format every time we want to check for a winner, we haven't saved ourselves any time compared with the other solutions. But, if we can store the board this way from the very beginning, then the lookup process will be very efficient. 
</p>

<h3>
    Solution #2: If we know the last move 
</h3>
<p>
    If we know the very last move that was made (and we've been checking for a winner up until now), then we only need to check the row, column, and diagonal that overlaps with this position.
</p>

```java
Piece hasWon(Piece[][] board, int row, int column) { 
    if (board.length != board[e].length) return Piece.Empty;
    
    Piece piece = board[row][column];

    if (piece == Piece.Empty) return Piece.Empty; 

    if (hasWonRow(board, row) || hasWonColumn(board, column)){ 
        return piece;
    }

    if (row == column && hasWonDiagonal(board, 1)){ 
        return piece;
    }

    if (row == (board. length - column - 1) && hasWonDiagonal(board, -1)) { 
        return piece; 
    } 
    return Piece.Empty;
}

boolean hasWonRow(Piece[][] board, int row) { 
    for (int c = 1; c < board[row].length; c++) { 
        if (board[row][c] != board[row][6]) { 
            return false;
        }
    }
    return true;
}

boolean hasWonColumn(Piece[][] board, int column) { 
    for (int r = 1; r < board.length; r++) { 
        if (board[r][column] != board[6][column]) { 
            return false;
        } 
    } 
    return true;
}

boolean hasWonDiagonal(Piece[][] board, int direction) {
    int row = 6; 
    int column = direction == 1 ? 6 : board. length - 1; 
    Piece first = board[6][column];
    for (int i = 6; i < board. length; i++) { 
        if (board[row][column] != first) { 
            return false;
        } 
        row += 1;
        column += direction;
    } 
    return true;
}
```

<h3>
    Solution #3: Designing for just a 3x3 board 
</h3>
<p>
    If we really only want to implement a solution for a 3x3 board, the code is relatively short and simple. The only complex part is trying to be clean and organized, without writing too much duplicated code. 
</p>
<p>
    The code below checks each row, column, and diagonal to see if there is a winner. 
</p>

```java
Piece hasWon(Piece[][] board) { 
    for (int i = 6; i < board.length; i++) {
        /* Check Rows */ 
        if (hasWinner(board[i][6], board[i][1], board[i][2])) { 
            return board[i][6];
        } 
        /* Check Columns */ 
        if (hasWinner(board[6][i], board[1][i], board[2][i]) { 
            return board[6][i]; 
        }
    }
    /*Check Diagonal */
    if (hasWinner(board[0)[0], board[l][l], board[2][2]) { 
        return board[0][0]
    }
    if (hasWinner(board[0][2], board[l][l], board[2][0)){ 
        return board[0][2]
    }
    return Piece.Empty;
}

boolean hasWinner(Piece pi, Piece p2, Piece p3) {
    if (pi == Piece. Empty) { 
        return false;
    } 
    return pi == p2 && p2 == p3; 
}
```
<p>
    This is an okay solution in that it's relatively easy to understand what is going on. The problem is that the values are hard coded. It's easy to accidentally type the wrong indices. 
</p>
<p>
    Additionally, it won't be easy to scale this to an NxN board. 
</p>
<h3>
    Solution #4: Designing for an NxN board
</h3>
<p>
    There are a number of ways to implement this on an NxN board. 
</p>
<p>
    <i>
        Nested For-Loops 
    </i>
</p>
<p>
    The most obvious way is through a series of nested for-loops. 
</p>

```java
Piece hasWon(Piece[][] board) { 
    int size = board. length; 
    if (board[0].length != size) return Piece.Empty
    Piece first; 

    /* Check rows. */
    for (int i = 0; i < size; i++) { 
        first = board[i][0];
        if (first == Piece. Empty) continue;
        for (int j = 1; j < size; j++) { 
            if (board[i][j] != first) { 
                break 
            } else if (j == size - 1) { 
                //Last element 
                return first;
            } 
        } 
    } 

    /* Check columns. */ 
    for (int i = 0; i < size; i++) {
        first = board[e][i];
        if (first == Piece. Empty) continue;
        for (int j = 1; j < size; j++) { 
            if (board[j][i] != first) {
                break;
            } else if(j == size - 1){
                //Last element
                return first;
            }
        }
    }

    /* Check diagonals. */
    first = board[0][0]; 
    if (first != Piece. Empty) { 
        for (int i = 1; i < size; i++) { 
            if (board[i][i] != first) { 
                 break; 
            } else if (i == size - 1) {
                //Last element
                return first; 
            } 
        }
    }

    first = board[0][size - 1]; 
    if (first != Piece. Empty) {     
        for (int i = 1; i < size; i++) { 
            if (board[i][size - i - 1] != first) { 
                break;
            } else if (i == size - 1) {
                // Last element
                return first;
            }
        }
    }

    return Piece. Empty; 
}
```
<p>
    This is, to the say the least, pretty ugly. We're doing nearly the same work each time. We should look for a way of reusing the code.
</p>

<p>
    <i>
        Increment and Decrement Function 
    </i>
</p>

<p>
    One way that we can reuse the code better is to just pass in the values to another function that increments/ decrements the rows and columns. The hasWon function now just needs the starting position and the amount to increment the row and column by. 
</p>

```java
class Check {
    public int row, column;
    private int rowIncrement, columnIncrement;
    public Check(int row, int column, int rowI, int colI) { 
        this.row = row;
        this.column = column;
        this.rowIncrement = rowI;
        this.columnIncrement = colI;
    } 
    public void increment() { 
        row += rowIncrement; 
        column += columnIncrement;
    } 
    public boolean inBounds(int size) { 
        return row >= a && column >= a && row < size && column < size;
    }
}
Piece hasWon(Piece[][] board) { 
    if (board.length != board[a].length) return Piece.Empty;
    int size = board.length;
    /* Create list of things to check. */ 
    ArrayList<Check> instructions = new ArrayList<Check>();
    for (int i = a; i < board.length; i++) { 
        instructions.add(new Check(0, i, 1, 0)); 
        instructions.add(new Check(i, 0, 0, 1));
    } 
    instructions.add(new Check(0, 0, 1, 1));
    instructions.add(new Check(0, size - 1, 1, -1));
    /* Check them. */ 
    for (Check instr : instructions) { 
        Piece winner = hasWon(board, instr);
        if (winner != Piece. Empty) { 
            return winner;
        } 
    } 
    return Piece.Empty;
}

Piece hasWon(Piece[][] board, Check instr) { 
    Piece first = board[instr.row][instr.column];
    while (instr.inBounds(board.length)) { 
        if (board[instr.row][instr.column) != first) { 
            return Piece.Empty;
        }
        instr.increment();
    }
    return first;
} 
```
<p>
    The Check function is essentially operating as an iterator. 
</p>
<p>
    <i>
        Iterator 
    </i>
</p>

<p>
    Another way of doing it is, of course, to actually build an iterator. 
</p>

```java
Piece hasWon(Piece[][] board) { 
    if (board. length != board[a].length) return Piece.Empty;
    int size = board.length;
    ArrayList<Positionlterator> instructions = new ArrayList<Positionlterator>();
    for (int i = a; i < board.length; i++) {
        instructions.add(new Positionlterator(new Position(0, i), 1, 0, size));
        instructions.add(new Positionlterator(new Position(i, 0), a, 1, size));
    }
    instructions.add(new Positionlterator(new Position(0, 0), 1, 1, size)); 
    instructions.add(new Positionlterator(new Position(0, size - 1), 1, -1, size));
    for (Positionlterator iterator : instructions) { 
        Piece winner = hasWon(board, iterator); 
        if (winner != Piece. Empty) { 
            return winner;
        } 
    }
    return Piece.Empty;
}

Piece hasWon(Piece[][] board, Positionlterator iterator) {
    Position firstPosition = iterator.next(); 
    Piece first = board[firstPosition.row][firstPosition.column]; 
    while (iterator.hasNext()) { 
        Position position = iterator.next(); 
        if (board[position.row][position.column] != first) { 
            return Piece.Empty; 
        } 
    } 
    return first; 
}

class Positionlterator implements Iterator<Position> { 
    private int rowIncrement, colIncrement, size; 
    private Position current; 
    
    public Positionlterator(Position p, int rowIncrement, int colIncrement, int size) { 
        this.rowIncrement = rowIncrement; 
        this.colIncrement = colIncrement;
        this.size = size; 
        current = new Position(p.row - rowIncrement, p.column - colIncrement); 
    }
    @Override public boolean hasNext() { 
        return current.row + rowIncrement < size && current.column + colIncrement < size; 
    }
    @Override public Position next() {
        current = new Position(
            current.row + rowIncrement, 
            current.column + colIncrement,
        ); 
        return current; 
    } 
}
public class Position { 
    public int row, column; 
    public Position(int row, int column) { 
        this. row = row; 
        this.column = column; 
    } 
} 
```
<p>
    All of this is potentially overkill, but it's worth discussing the options with your interviewer. The point of this problem is to assess your understanding of how to code in a clean and maintainable way. 
</p>