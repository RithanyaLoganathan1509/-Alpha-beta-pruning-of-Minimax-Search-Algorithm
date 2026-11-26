<h1>ExpNo 7 : Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game</h1> 
<h3>Name: RITHANYA L       </h3>
<h3>Register Number: 212225040341        </h3>
<H3>Aim:</H3>
<p>
Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
</p>
<h1>GOALS of Alpha-Beta Pruning in MiniMax Search Algorithm</h1>

<h3>Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.</h3>
<h3>Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning and the Minimax algorithm with Python Code.</h3>
<h1>IMPLEMENTATION</h1>

The project involves developing a Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning with the Minimax algorithm. Using this algorithm, the computer player analyzes the game state, evaluates possible moves, and selects the optimal action based on the anticipated outcomes.

<h1>The Minimax algorithm</h1>

recursively evaluates all possible moves and their potential outcomes, creating a game tree.

<h1>Alpha-Beta pruning</h1>

Alpha–Beta (𝛼−𝛽) algorithm is actually an improved minimax using a heuristic. It stops evaluating a move when it makes sure that it’s worse than a previously examined move. Such moves need not to be evaluated further.

When added to a simple minimax algorithm, it gives the same output but cuts off certain branches that can’t possibly affect the final decision — dramatically improving the performance
<hr>
<h2>Sample Input and Output:</h2>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8d5e329a-9aff-41a6-bcf0-46efa10e1b92)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/438b242d-54ba-443e-b040-a936e6ae3b55)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/99a33390-fa11-4ade-a19f-e93bcd7aaec9)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/440797bd-53cb-49c1-b18d-89776864c3e7)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/81575a16-26b2-46f1-a8ac-27c9ed0a0fe5)

## Program:

```
import time

class Game:

    def __init__(self):
        self.board = [['.', '.', '.'],
                      ['.', '.', '.'],
                      ['.', '.', '.']]

    # Display board
    def display(self):
        for row in self.board:
            print(' | '.join(row))
        print()

    # Check winner or draw
    def check_winner(self):

        # Rows and columns
        for i in range(3):
            if self.board[i][0] != '.' and \
               self.board[i][0] == self.board[i][1] == self.board[i][2]:
                return self.board[i][0]

            if self.board[0][i] != '.' and \
               self.board[0][i] == self.board[1][i] == self.board[2][i]:
                return self.board[0][i]

        # Diagonals
        if self.board[0][0] != '.' and \
           self.board[0][0] == self.board[1][1] == self.board[2][2]:
            return self.board[0][0]

        if self.board[0][2] != '.' and \
           self.board[0][2] == self.board[1][1] == self.board[2][0]:
            return self.board[0][2]

        # Check for empty space
        for row in self.board:
            if '.' in row:
                return None

        return 'Draw'

    # MAX player - Computer O
    def max_value(self, alpha, beta):

        result = self.check_winner()

        if result == 'X':
            return -1, -1, -1

        if result == 'O':
            return 1, -1, -1

        if result == 'Draw':
            return 0, -1, -1

        best = -2
        best_move = (-1, -1)

        for i in range(3):
            for j in range(3):

                if self.board[i][j] == '.':

                    # Try O
                    self.board[i][j] = 'O'

                    score, _, _ = self.min_value(alpha, beta)

                    # Undo move
                    self.board[i][j] = '.'

                    if score > best:
                        best = score
                        best_move = (i, j)

                    alpha = max(alpha, best)

                    # Alpha-Beta pruning
                    if alpha >= beta:
                        break

            if alpha >= beta:
                break

        return best, best_move[0], best_move[1]

    # MIN player - Human X
    def min_value(self, alpha, beta):

        result = self.check_winner()

        if result == 'X':
            return -1, -1, -1

        if result == 'O':
            return 1, -1, -1

        if result == 'Draw':
            return 0, -1, -1

        best = 2
        best_move = (-1, -1)

        for i in range(3):
            for j in range(3):

                if self.board[i][j] == '.':

                    # Try X
                    self.board[i][j] = 'X'

                    score, _, _ = self.max_value(alpha, beta)

                    # Undo move
                    self.board[i][j] = '.'

                    if score < best:
                        best = score
                        best_move = (i, j)

                    beta = min(beta, best)

                    # Alpha-Beta pruning
                    if alpha >= beta:
                        break

            if alpha >= beta:
                break

        return best, best_move[0], best_move[1]

    # Play game
    def play(self):

        while True:

            self.display()

            result = self.check_winner()

            if result:
                if result == 'X':
                    print("X wins!")
                elif result == 'O':
                    print("O wins!")
                else:
                    print("Draw!")
                break

            # Player X
            print("Your turn (X)")

            while True:
                x = int(input("Enter row (0-2): "))
                y = int(input("Enter column (0-2): "))

                if 0 <= x < 3 and 0 <= y < 3 and self.board[x][y] == '.':
                    self.board[x][y] = 'X'
                    break

                print("Invalid move!")

            if self.check_winner() == 'X':
                self.display()
                print("X wins!")
                break

            # Computer O
            start = time.time()

            score, x, y = self.max_value(-2, 2)

            self.board[x][y] = 'O'

            end = time.time()

            print("Computer chose:", x, y)
            print("Time:", round(end - start, 6), "seconds")

            if self.check_winner() == 'O':
                self.display()
                print("O wins!")
                break


game = Game()
game.play()
```

## Output:

<img width="283" height="771" alt="image" src="https://github.com/user-attachments/assets/fbb0cfab-df2e-47e8-a7b7-f0fe551dee51" />

## Result:
Thus, Implementation of Minimax Search Algorithm for a Simple TIC-TAC-TOE game has been done successfully.

