
# EX 3A N Queens Problem - Backtracking Approach.
## DATE: 24.09.25
## AIM:
To Write a Java program for N queens using backtracking approach.
You are given an integer N. For a given N x N chessboard, find a way to place 'N' queens such that no queen can attack any other queen on the chessboard.
A queen can be attacked when it lies in the same row, column, or the same diagonal as any of the other queens. You have to print one such configuration.
Chess Board
<img width="241" height="209" alt="image" src="https://github.com/user-attachments/assets/96aacb61-4f34-423f-b324-5e34454e42b8" />


Note :

Get the input from the user for N . The value of N must be from 1 to 4

If solution exists Print a binary matrix as output that has 1s for the cells where queens are placed

If there is no solution to the problem  print  "Solution does not exist"

## Algorithm
1. Start the program and read the value of N (size of the chessboard) from the user.
2. Initialize an N x N board filled with zeros to represent an empty chessboard.
3. Use the isSafe() function to check if a queen can be safely placed at a given position (no other queen in the same row, column, or diagonal).
4. Use recursion (solveNQUtil()) to try placing queens column by column, backtracking if a conflict occurs.
5. If a solution is found, print the board with 1s marking queen positions; otherwise, print "Solution does not exist". 

## Program:
```
/*
Program to implement Reverse a String
Developed by: MUKESH R
Register Number: 212223240100
*/

import java.util.Scanner;

public class NQueens {
    static int N;

    static void printSolution(int[][] board) {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                System.out.print(board[i][j] + " ");
            }
            System.out.println();
        }
    }

    static boolean isSafe(int[][] board, int row, int col) {
        for (int i = 0; i < col; i++)
            if (board[row][i] == 1)
                return false;

        for (int i = row, j = col; i >= 0 && j >= 0; i--, j--)
            if (board[i][j] == 1)
                return false;

        for (int i = row, j = col; i < N && j >= 0; i++, j--)
            if (board[i][j] == 1)
                return false;

        return true;
    }

    static boolean solveNQUtil(int[][] board, int col) {
        if (col >= N)
            return true;

        for (int i = 0; i < N; i++) {
            if (isSafe(board, i, col)) {
                board[i][col] = 1;

                if (solveNQUtil(board, col + 1))
                    return true;

                board[i][col] = 0; // Backtrack
            }
        }

        return false;
    }

    static boolean solveNQ() {
        int[][] board = new int[N][N];

        if (!solveNQUtil(board, 0)) {
            System.out.println("Solution does not exist");
            return false;
        }

        printSolution(board);
        return true;
    }


    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        N = scanner.nextInt(); // Accept board size
        solveNQ();
    }
}

```

## Output:

<img width="621" height="259" alt="image" src="https://github.com/user-attachments/assets/b1d82175-96e4-4d73-93e7-5975a554996f" />

## Result:
The program successfully implemented and the ouput is verified. 



# EX 3B Rat in Maze- Backtracking 
## DATE: 24.09.25
## AIM:
To write a Java program to for given constraints.
here is a ball in a maze with empty spaces (represented as 0) and walls (represented as 1). The ball can go through the empty spaces by rolling up, down, left or right, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the m x n maze, the ball's start position and the destination, where start = [startrow, startcol] and destination = [destinationrow, destinationcol], return true if the ball can stop at the destination, otherwise return false.

You may assume that the borders of the maze are all walls (see examples).
<img width="573" height="573" alt="image" src="https://github.com/user-attachments/assets/d6f1c054-cdc2-4bb3-9c55-512fb2cf0fb7" />
Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [4,4]
Output: true
Explanation: One possible way is : left -> down -> left -> down -> right -> down -> right.


## Algorithm
1. Input: m × n maze grid, start position, and destination position; initialize visit[][] array to track visited cells.
2. DFS: Start from start cell; mark it as visited.
3. Base case: If current cell equals destination, return true.
4. Explore directions: For each direction (up, down, left, right), roll the ball until it hits a wall, then recursively call DFS from the stopping position.
5. Output: Return true if a path exists to destination; otherwise, return false.  

## Program:
```
/*
Program to implement Reverse a String
Developed by: MUKESH R
Register Number: 212223240100
*/

import java.util.*;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int m = sc.nextInt();
        int n = sc.nextInt();

        int[][] maze = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                maze[i][j] = sc.nextInt();
            }
        }

        int[] start = new int[]{sc.nextInt(), sc.nextInt()};

        int[] destination = new int[]{sc.nextInt(), sc.nextInt()};

        Solution sol = new Solution();
        boolean result = sol.hasPath(maze, start, destination);

        System.out.println(result);
    }
}

class Solution {
    public boolean dfs(int m, int n, int[][] maze, int[] curr, int[] destination, boolean[][] visit) {
       if(visit[curr[0]][curr[1]]) return false;
       if(curr[0]==destination[0]&&curr[1]==destination[1]) return true;
       visit[curr[0]][curr[1]] = true;
       int[] dirX = {0, 1, 0, -1};
       int[] dirY = {-1, 0, 1, 0};

        for (int i = 0; i < 4; i++) {
            int r = curr[0], c = curr[1];
            while (r >= 0 && r < m && c >= 0 && c < n && maze[r][c] == 0) {
                r += dirX[i];
                c += dirY[i];
            }
            r -= dirX[i];
            c -= dirY[i];
            if (dfs(m, n, maze, new int[]{r, c}, destination, visit)) {
                return true;
            }
        }
        return false;
    }
    public boolean hasPath(int maze[][],int start[],int destination[])
    {
        int m=maze.length;
        int n=maze[0].length;
        boolean [][]visit=new boolean[m][n];
        return dfs(m,n,maze,start,destination,visit);
        
    }
}

```

## Output:

<img width="362" height="524" alt="image" src="https://github.com/user-attachments/assets/b012e703-f11f-4231-afe9-abac1eccafee" />

## Result:
The program successfully implemented and the expected output is verified.



# EX 3C Tug of War problem - Backtracking.
## DATE: 08.10.25
## AIM:
To write a Java program to for given constraints.
Given an integer array nums, return true if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or false otherwise.
Example 1:
Input: Enter the number of elements: 4
Enter the elements of the array:
1 5 11 5
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].

Constraints:

1 <= nums.length <= 200
1 <= nums[i] <= 100

## Algorithm
1. Start the program and read the number of elements n, then input n integers into the array nums.
2. Calculate the total sum of all elements in the array.
3. If the total sum is odd, print false (since it cannot be divided equally).
4. Set the target as half of the total sum and create a boolean array dp to track possible subset sums.
5. Use dynamic programming to fill the dp array and finally print true if a subset with the target sum exists, else false.

## Program:
```
/*
Program to implement Reverse a String
Developed by: MUKESH R
Register Number: 212223240100
*/

import java.util.Scanner;
public class Solution {
    public boolean canPartition(int[] nums) {
        if (nums.length == 0)
            return false;
        int totalSum = 0;
        for (int num : nums) {
            totalSum += num;
        }
        if (totalSum % 2 != 0)
            return false;
        int subSetSum = totalSum / 2;
        boolean[] dp = new boolean[subSetSum + 1];
        dp[0] = true;
        for (int curr : nums) {
            for (int j = subSetSum; j >= curr; j--) {
                dp[j] |= dp[j - curr];
            }
        }
        return dp[subSetSum];
    }
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Solution sol = new Solution();
        int n = scanner.nextInt();
        int[] nums = new int[n];
        for (int i = 0; i < n; i++) {
            nums[i] = scanner.nextInt();
        }
        boolean canBePartitioned = sol.canPartition(nums);
        System.out.println(canBePartitioned);
    }
}

```

## Output:

<img width="398" height="227" alt="image" src="https://github.com/user-attachments/assets/4dd4fb7a-ac78-4bd8-85b9-080b08f4778f" />

## Result:
The program successfully implemented and the expected output is verified.



# EX 3D Sudoku solver - Backtracking.
## DATE: 07.10.25
## AIM:
To write a Java program to solve a Sudoku puzzle by filling the empty cells.

For example:
<img width="357" height="322" alt="image" src="https://github.com/user-attachments/assets/334b8c39-d547-4743-aca0-de92e38bdd1c" />



## Algorithm
1. Start the program and read a 9×9 Sudoku board input, where empty cells are represented by 0.
2. Define a function isSafe() to check if a number can be placed in a cell without violating Sudoku rules (row, column, and 3×3 box).
3. Use the recursive function solveSudoku() to fill empty cells one by one using backtracking.
4. If the number placement leads to a valid configuration, continue recursively; otherwise, backtrack by resetting the cell to 0.
5. After solving, print the completed Sudoku board if a solution exists; otherwise, display "No solution exists."

## Program:
```
/*
Program to implement Reverse a String
Developed by: MUKESH R
Register Number: 212223240100
*/
import java.util.Scanner;

public class SudokuSolver {

    // Check if it's safe to place the number
    static boolean isSafe(int[][] board, int row, int col, int num) {
        // Check row and column
        for (int i = 0; i < 9; i++) {
            if (board[row][i] == num || board[i][col] == num)
                return false;
        }

        // Check 3x3 subgrid
        int startRow = row - row % 3;
        int startCol = col - col % 3;

        for (int i = 0; i < 3; i++)
            for (int j = 0; j < 3; j++)
                if (board[startRow + i][startCol + j] == num)
                    return false;

        return true;
    }

    // Recursive backtracking solver
    static boolean solveSudoku(int[][] board, int row, int col) {
        if (row == 8 && col == 9)
            return true;

        if (col == 9) {
            row++;
            col = 0;
        }

        if (board[row][col] != 0)
            return solveSudoku(board, row, col + 1);

        for (int num = 1; num <= 9; num++) {
            if (isSafe(board, row, col, num)) {
                board[row][col] = num;
                if (solveSudoku(board, row, col + 1))
                    return true;
                board[row][col] = 0; // Backtrack
            }
        }

        return false;
    }

    // Utility to print the board
    static void printBoard(int[][] board) {
        for (int[] row : board) {
            for (int val : row)
                System.out.print(val + " ");
            System.out.println();
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int[][] board = new int[9][9];

      //  System.out.println("Enter the Sudoku puzzle row by row (use 0 for empty cells):");

        for (int i = 0; i < 9; i++) {
            //System.out.print("Enter row " + (i + 1) + ": ");
            for (int j = 0; j < 9; j++) {
                board[i][j] = sc.nextInt();
            }
        }

      //  System.out.println("\nSolving...\n");

        if (solveSudoku(board, 0, 0)) {
            System.out.println("Solved Sudoku:");
            printBoard(board);
        } else {
            System.out.println("No solution exists.");
        }

        sc.close();
    }
}

```

## Output:

<img width="653" height="595" alt="image" src="https://github.com/user-attachments/assets/e539b722-e2b1-4ffd-879a-48bde7ecb750" />

## Result:
The program successfully implemented and the expected output is verified.



# EX 3E Generate Permutations using Backtracking  Approach.
## DATE: 08.10.25
## AIM:
To write a Java program to for given constraints.
Given an array nums of distinct integers, return all the possible Permutation. You can return the answer in any order.
Example 1:
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
For example:
## Algorithm
1. Start the program and read the input array of integers from the user.
2. Initialize an empty list ans to store all possible permutations.
3. Use the recursive backtrack() method to build permutations by adding unused elements one by one.
4. When the current permutation length equals the array length, add it to the result list.
5. After recursion completes, print the list of all generated permutations.

## Program:
```
/*
Program to implement Reverse a String
Developed by: MUKESH R
Register Number: 212223240100
*/

import java.util.*;

public class Solution {

    public List<List<Integer>> permute(int[] nums) {
        //add your code here
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(new ArrayList<>(), ans, nums);
        return ans;

    }

    public void backtrack(List<Integer> curr, List<List<Integer>> ans, int[] nums) {
        //Add your code here
        if (curr.size() == nums.length) {
            ans.add(new ArrayList<>(curr));
            return;
        }

        for (int num : nums) {
            if (!curr.contains(num)) {
                curr.add(num);
                backtrack(curr, ans, nums);
                curr.remove(curr.size() - 1);
            }
        }

    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String inputLine = scanner.nextLine().trim();
        inputLine = inputLine.replaceAll(".*\\[|\\].*", ""); 
        String[] parts = inputLine.split(",");

        int[] nums = new int[parts.length];
        for (int i = 0; i < parts.length; i++) {
            nums[i] = Integer.parseInt(parts[i].trim());
        }
        Solution solution = new Solution();
        List<List<Integer>> permutations = solution.permute(nums);
        System.out.println(permutations);
        scanner.close();
    }
}

```

## Output:

<img width="1223" height="220" alt="image" src="https://github.com/user-attachments/assets/fdc239f4-b8a5-4ed2-8e5a-bba583301849" />

## Result:
The program successfully implemented and the expected output is verified.



