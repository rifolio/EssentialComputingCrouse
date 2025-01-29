# Random Pits Path Finding Game

<img src="https://img.shields.io/badge/Language-Java-orange.svg">
<img src="https://img.shields.io/badge/Algorithm-Breadth_First_Search_BFS-purple.svg">

<p align="center">
  <figure>
    <img src="Figures/logo.png" alt="Game Logo" width="50%">
    <figcaption style="text-align: center;">
      <em><a href="https://www.growingwiththeweb.com/2012/06/a-pathfinding-algorithm.html">Growing with the Web: A* pathfinding algorithm</a></em>
    </figcaption>
  </figure>
</p>

## Mini-project (Autumn Semester 2023)

**Author:** Vladyslav Horbatenko  
**Email:** vladyslav@ruc.dk  
**Institution:** Roskilde University  
**Date:** November 2023

---

## Overview

This project is a single-player game implemented in Java, where the player must navigate a **20x20 grid** using the **W, A, S, D** keys to reach the goal while avoiding **randomly appearing pits**. The challenge is that pits appear **randomly on every turn**, making pathfinding an essential part of the game. The program also implements the **Breadth-First Search (BFS) algorithm** to check if the goal is reachable.

---

## Rules

1. The game is played on a **20 × 20** board (grid).
2. Initially, all cells are **free**, and the player starts at **(0,0)**.
3. The player can move **up, down, left, or right**.
4. At each turn, a **random number of pits (1-5)** appear in unoccupied cells.
5. The game is **won** if the player reaches the **goal (19,19)**.
6. The game is **lost** if the player lands on a **pit**.
7. The game is **also lost** if the player becomes **trapped** and can no longer reach the goal.

---

## Solution Overview

This project includes the following components:

### 1. Initial Variables and Constants

Key variables and constants used in the game:

```java
private static final int BOARD_SIZE = 20;
private static final char FREE_CELL = '.';
private static final char PLAYER = 'X';
private static final char PIT = 'o';
private static final char GOAL = 'G';
private char[][] board;
private boolean fellIntoPit = false;
```

### 2. Board Initialization

The board is implemented as a **2D array**, and the initialization process sets up the **free spaces, player, and goal**:

```java
public PitGame() {
    board = new char[BOARD_SIZE][BOARD_SIZE];
    initializeBoard();
    placePlayer();
    placeGoal();
}
```

### 3. Placing Entities

The game includes three main entities:

- **Player** (X)
- **Goal** (G)
- **Pits** (o)

Entities are placed on the board using methods like:

```java
private void placePlayer() {
    playerRow = 0;
    playerCol = 0;
    board[playerRow][playerCol] = PLAYER;
}
```

Pits are placed at random locations **each round**:

```java
private void placePits() {
    int numPits = random.nextInt(5) + 1;
    for (int i = 0; i < numPits; i++) {
        int row, col;
        do {
            row = random.nextInt(BOARD_SIZE);
            col = random.nextInt(BOARD_SIZE);
        } while (board[row][col] != FREE_CELL);
        board[row][col] = PIT;
    }
}
```

### 4. Visualization

The game board is displayed in the console:

```java
private void printBoard() {
    for (int i = 0; i < BOARD_SIZE; i++) {
        for (int j = 0; j < BOARD_SIZE; j++) {
            System.out.print(board[i][j] + " ");
        }
        System.out.println();
    }
}
```

### 5. Move Validation

Ensuring moves are within bounds and checking win/loss conditions:

```java
private boolean isValidMove(int newRow, int newCol) {
    return newRow >= 0 && newRow < BOARD_SIZE && newCol >= 0 && newCol < BOARD_SIZE;
}

private boolean isGameWon() {
    return playerRow == goalRow && playerCol == goalCol;
}
```

### 6. BFS Algorithm for Pathfinding

**Breadth-First Search (BFS)** is used to check if the **goal is reachable** at each turn. If the algorithm determines that the goal is blocked, the game is lost.

#### BFS Initialization:

```java
private boolean isGoalReachable() {
    boolean[][] visited = new boolean[BOARD_SIZE][BOARD_SIZE];
    visited[playerRow][playerCol] = true;
    java.util.Queue<int[]> queue = new java.util.LinkedList<>();
    queue.offer(new int[]{playerRow, playerCol});
```

#### BFS Loop:

```java
while (!queue.isEmpty()) {
    int[] current = queue.poll();
    int row = current[0];
    int col = current[1];
    for (int i = 0; i < 4; i++) {
        int newRow = row + dr[i];
        int newCol = col + dc[i];
        if (newRow >= 0 && newRow < BOARD_SIZE && newCol >= 0 && newCol < BOARD_SIZE &&
            !visited[newRow][newCol] && board[newRow][newCol] != PIT) {
            if (newRow == goalRow && newCol == goalCol) {
                return true;
            }
            visited[newRow][newCol] = true;
            queue.offer(new int[]{newRow, newCol});
        }
    }
}
return false;
```

### 7. Main Loop

The **game loop** handles user input and updates the game state.

```java
switch (move) {
    case "W": newRow--; break; // Move Up
    case "D": newCol++; break; // Move Right
    case "S": newRow++; break; // Move Down
    case "A": newCol--; break; // Move Left
    default:
        System.out.println("Invalid input. Use W, A, S, D.");
        continue;
}
```

### 8. Conclusion

- The game successfully implements the **BFS pathfinding algorithm**.
- Future improvements:
  - Implement **A\* Algorithm** for better efficiency.
  - Split the code into multiple **Java classes** for modularity.
  - Improve UI/UX by transitioning from **console-based to GUI**.
  - Fix **minor logic bugs** (e.g., pits appearing even when the player tries an invalid move).

---

## References

1. [A\* Pathfinding Algorithm - CodePal](https://codepal.ai/code-generator/query/jLOJJA08/a-star-path-finder-algorithm)
2. [BFS Algorithm Explanation - GeeksForGeeks](https://www.geeksforgeeks.org/queue-interface-java/)
3. [YouTube - Pathfinding Algorithms](https://youtu.be/GC-nBgi9r0U?si=ftVaB1IsX0refGFg)

---

## How to Run

1. Clone this repository:
   ```sh
   git clone https://github.com/rifolio/EssentialComputingCrouse.git
   ```
2. Compile the Java program:
   ```sh
   javac PitGame.java
   ```
3. Run the game:
   ```sh
   java PitGame
   ```
