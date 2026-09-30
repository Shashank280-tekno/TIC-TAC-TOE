#include <iostream>
#include <vector>
#include <limits>

using namespace std;

class TicTacToe {
private:
    char board[3][3];
    char currentPlayer;

    void resetBoard() {
        char initialCell = '1';
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                board[i][j] = initialCell++;
            }
        }
        currentPlayer = 'X';
    }

    void displayBoard() const {
        cout << "\n";
        cout << " " << board[0][0] << " | " << board[0][1] << " | " << board[0][2] << "\n";
        cout << "---+---+---\n";
        cout << " " << board[1][0] << " | " << board[1][1] << " | " << board[1][2] << "\n";
        cout << "---+---+---\n";
        cout << " " << board[2][0] << " | " << board[2][1] << " | " << board[2][2] << "\n\n";
    }

    bool makeMove(int choice) {
        if (choice < 1 || choice > 9) return false;

        int row = (choice - 1) / 3;
        int col = (choice - 1) % 3;

        // Check if cell contains the original number (unoccupied)
        if (board[row][col] != 'X' && board[row][col] != 'O') {
            board[row][col] = currentPlayer;
            return true;
        }
        return false;
    }

    bool checkWin() const {
        // Check Rows and Columns
        for (int i = 0; i < 3; ++i) {
            if (board[i][0] == board[i][1] && board[i][1] == board[i][2]) return true;
            if (board[0][i] == board[1][i] && board[1][i] == board[2][i]) return true;
        }
        // Check Diagonals
        if (board[0][0] == board[1][1] && board[1][1] == board[2][2]) return true;
        if (board[0][2] == board[1][1] && board[1][1] == board[2][0]) return true;

        return false;
    }

    bool checkDraw() const {
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                if (board[i][j] != 'X' && board[i][j] != 'O') {
                    return false; // Still an open slot
                }
            }
        }
        return true;
    }

    void switchPlayer() {
        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

public:
    void play() {
        char playAgain = 'y';

        while (playAgain == 'y' || playAgain == 'Y') {
            resetBoard();
            bool gameOver = false;

            cout << "===========================\n";
            cout << "   WELCOME TO TIC-TAC-TOE  \n";
            cout << "===========================\n";

            while (!gameOver) {
                displayBoard();
                int choice;
                cout << "Player " << currentPlayer << ", enter a cell position (1-9): ";

                if (!(cin >> choice)) {
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "--> Invalid input! Please enter a number between 1 and 9.\n";
                    continue;
                }

                if (!makeMove(choice)) {
                    cout << "--> Invalid move! That spot is either taken or out of range.\n";
                    continue;
                }

                if (checkWin()) {
                    displayBoard();
                    cout << "***********************************\n";
                    cout << "  CONGRATULATIONS! Player " << currentPlayer << " wins!\n";
                    cout << "***********************************\n";
                    gameOver = true;
                } else if (checkDraw()) {
                    displayBoard();
                    cout << "***********************************\n";
                    cout << "  IT'S A DRAW! Good game.\n";
                    cout << "***********************************\n";
                    gameOver = true;
                } else {
                    switchPlayer();
                }
            }

            cout << "\nDo you want to play again? (y/n): ";
            cin >> playAgain;
        }

        cout << "\nThanks for playing! Goodbye.\n";
    }
};

int main() {
    TicTacToe game;
    game.play();
    return 0;
}
