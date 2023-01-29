## Simple TicTacToe AI

```cpp
#include <vector>
#include <random>
#include <iomanip>
#include <iostream>

std::random_device rd;
std::mt19937 mt(rd());
std::uniform_int_distribution<int> dist(1, 9);

void greet() {
    std::cout << std::setw(30) << "TIC TAC TOE GAME!" << std::endl;
    std::cout << std::setw(31) << "Player 1 [X]\tCPU [O]" << std::endl;
}

void printBoard(const std::vector<char> &board) {
    std::cout << std::endl;
    std::cout << std::setw(15) << board[0] << "  |  " << board[1] << "  |  " << board[2] << std::endl;
    std::cout << std::setw(29) << "-----+-----+-----" << std::endl;
    std::cout << std::setw(15) << board[3] << "  |  " << board[4] << "  |  " << board[5] << std::endl;
    std::cout << std::setw(29) << "-----+-----+-----" << std::endl;
    std::cout << std::setw(15) << board[6] << "  |  " << board[7] << "  |  " << board[8] << std::endl;
    std::cout << std::endl;
}

bool gameOver(const std::vector<char> &board) {
    // check horizontal rows
    if (board[0] == board[1] && board[1] == board[2] ||
        board[3] == board[4] && board[4] == board[5] ||
        board[6] == board[7] && board[7] == board[8])
        return false;

    // check vertical rows
    if (board[0] == board[3] && board[3] == board[6] ||
        board[1] == board[4] && board[4] == board[7] ||
        board[2] == board[5] && board[5] == board[8])
        return false;

    // check diagonals
    if (board[0] == board[4] && board[4] == board[8] ||
        board[2] == board[4] && board[4] == board[6])
        return false;

    return true;
}

char whoWon(const std::vector<char> &board) {
    // check horizontal rows
    if (board[0] == board[1] && board[1] == board[2])
        return board[0];

    if (board[3] == board[4] && board[4] == board[5])
        return board[3];

    if (board[6] == board[7] && board[7] == board[8])
        return board[6];

    // check vertical columns
    if (board[0] == board[3] && board[3] == board[6])
        return board[0];

    if (board[1] == board[4] && board[4] == board[7])
        return board[1];

    if (board[2] == board[5] && board[5] == board[8])
        return board[2];

    // check diagonals
    if (board[0] == board[4] && board[4] == board[8])
        return board[0];

    if (board[2] == board[4] && board[4] == board[6])
        return board[2];

    return ' ';
}

void userTurn(std::vector<char> &board) {
    Turn:
    std::cout << "Player 1 [X] Turn: ";
    int position{};
    std::cin >> position;

    if (position < 0 || position > 9) {
        std::cout << "Invalid Position! Try Again!" << std::endl;
        goto Turn;
    }

    if (board[position - 1] == 'X' || board[position - 1] == 'O') {
        std::cout << "Invalid Position! Try Again!" << std::endl;
        goto Turn;
    }

    board[position - 1] = 'X';
}

bool isValidPlacement(const std::vector<char> &board, const int &position) {
    if (board[position - 1] != 'X' && board[position - 1] != 'O')
        return true;
    return false;
}

bool draw(const std::vector<char> &board) {
    for (int i = 0; i < 9; i++) {
        if (board.at(i) == ' ') {
            return true;
        }
    }
    return false;
}

void simpleComputer(std::vector<char> &board) {
    Compute:
    int random = dist(mt);
    if (isValidPlacement(board, random))
        board[random - 1] = 'O';
    else
        goto Compute;
}

int main() {
    std::vector<char> board{
            '1', '2', '3',
            '4', '5', '6',
            '7', '8', '9'
    };

    while (gameOver(board) && !draw(board)) {
        greet();
        printBoard(board);

        if (gameOver(board))
            userTurn(board);

        if (gameOver(board))
            simpleComputer(board);

        if (!gameOver(board))
            break;
    }

    greet();
    printBoard(board);

    if (whoWon(board) == 'X') {
        std::cout << "Player 1 [X] Won the Game!";
    } else if (whoWon(board) == 'O') {
        std::cout << "CPU [O] Won the Game!";
    } else if (!draw(board)) {
        std::cout << "Game Draw!";
    }
    return 0;
}
```
