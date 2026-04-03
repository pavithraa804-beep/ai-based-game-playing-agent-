📌 Overview
This project implements an AI agent that plays a turn-based game using the Minimax algorithm. The AI evaluates possible moves and chooses the optimal one assuming the opponent also plays optimally.
We use a custom-designed game state structure instead of relying on built-in representations.
🧠 What is Minimax?
Minimax is a decision-making algorithm used in:
Game AI (Tic-Tac-Toe, Chess, etc.)
Adversarial environments
It works by:
Maximizing the AI's score
Minimizing the opponent's score
🏗️ Custom Game Design
We define our own GameState structure:
C
typedef struct {
    char board[3][3];   // Game board
    int currentPlayer;  // 1 = AI, -1 = Human
} GameState;
⚙️ Key Components
1. Evaluation Function
Returns:
+10 → AI wins
-10 → Human wins
0 → Draw
2. Minimax Algorithm
C
int minimax(GameState state, int depth, int isMax) {
    int score = evaluate(state);

    if (score == 10 || score == -10)
        return score;

    if (isMovesLeft(state) == 0)
        return 0;

    if (isMax) {
        int best = -1000;

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (state.board[i][j] == '_') {
                    state.board[i][j] = 'X';
                    best = max(best, minimax(state, depth + 1, 0));
                    state.board[i][j] = '_';
                }
            }
        }
        return best;
    } else {
        int best = 1000;

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (state.board[i][j] == '_') {
                    state.board[i][j] = 'O';
                    best = min(best, minimax(state, depth + 1, 1));
                    state.board[i][j] = '_';
                }
            }
        }
        return best;
    }
}
3. Finding the Best Move
C
void findBestMove(GameState *state) {
    int bestVal = -1000;
    int bestRow = -1, bestCol = -1;

    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (state->board[i][j] == '_') {
                state->board[i][j] = 'X';

                int moveVal = minimax(*state, 0, 0);

                state->board[i][j] = '_';

                if (moveVal > bestVal) {
                    bestRow = i;
                    bestCol = j;
                    bestVal = moveVal;
                }
            }
        }
    }

    state->board[bestRow][bestCol] = 'X';
}
▶️ Example Game Flow
Initialize board
Human plays ('O')
AI calculates best move using Minimax
Repeat until:
Win
Loss
Draw
📊 Advantages
✔ Always finds optimal move
✔ Guarantees best outcome (if opponent is optimal)
✔ Works well for small games
⚠️ Limitations
❌ Slow for large games (like Chess)
❌ Exponential time complexity
❌ Needs optimization (Alpha-Beta pruning)
🚀 Future Improvements
Add Alpha-Beta Pruning
Support larger boards
Add GUI (using C graphics / Python)
🧪 Sample Output

AI Move:
X | O | X
O | X | _
_ | _ | O
🧾 Conclusion
This project demonstrates how Minimax enables intelligent decision-making in games using a custom-designed data structure. It forms the foundation for more advanced AI systems.
