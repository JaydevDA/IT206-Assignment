# A Simple Snake Game 🐍
- By Jaydev Dhabaliya 😎
- Dhruvir S Mayavat 😎
- Rashi Krishnani 👧🏻
- Rudrapratap Singh 😎

# Description

-The Snake Game in C++ is a classic arcade-style game built to demonstrate fundamental programming concepts such as game loops, user input handling, and collision detection. What makes this version unique is its 
 clean, modular code structure, making it easy to modify and extend with additional features like scoring, speed variations, or graphical enhancements.
-We will be using c++ as the coding language, and here is the step to step process to make the snake.

# Code Overview

## Include Libraries:

#include <iostream> - OS Agnostic // for doing i/p and o/p operations  
#include <conio.h> - Non-OS Agnostic // for doing console i/p and o/p operations  
#include <windows.h> - Non-OS Agnostic // for system("cls")  

``` #include <iostream>  
    #include <conio.h>
    #include <windows.h>
```
    

## Declaring Variables:
Values which will help in setting a graphical representation of the snake
- making a constant width and length of 25 units.
- **bool gameover** - to end the game when one of the many conditions is met.
- **x,y and Xfruit,Yfruit** - Position of the head of the snake and a fruit respectively.
- **int score = 0** - set the score to zero intitially.
- **int tail[100] and tail[100]** - to store the elements in the tail.
- **int tLength = 0** - setting the length of the tail to be zero.
- enum direction - to declare enum of directions like left,up,down and right, and also stop.
  

## Writing the key functions:

there are 4 key functions necessary for the snake program:

- void game() - to start/end the game
- void construct() - to construct a graphical representation of the values 
  

// include libraries

#include <iostream> 
#include <conio.h>
#include <windows.h>  

using namespace std;
bool gameover;
const int width = 25; 
const int length = 25;
int x, y, Xfruit, Yfruit, score;
int tailX[100], tailY[100]; 
int tLength = 0;         
enum direction { Stop = 0, Up, Down, Left, Right };
direction dir;

void game() {
    gameover = false;
    dir = Stop;
    x = width / 2;
    y = length / 2;
    Xfruit = rand() % (width - 1);
    Yfruit = rand() % (length - 2);
    score,tLength = 0;
}
void construct() {
    system("cls"); 

    for (int i = 0; i < width; i++) {
        cout << "#";
    }
    cout << endl;

    for (int i = 0; i < length - 1; i++) {
        for (int j = 0; j < width; j++) {
            if (j == 0 || j == width - 1)
                cout << "#";
            else if (j == x && i == y)
                cout << "O"; 
            else if (j == Xfruit && i == Yfruit)
                cout << "$"; 
            else {
                bool printTail = false;
                for (int k = 0; k < tLength; k++) {
                    if (tailX[k] == j && tailY[k] == i) {
                        cout << "o"; 
                        printTail = true;
                        break;
                    }
                }
                if (!printTail)
                    cout << " ";
            }
        }
        cout << endl;
    }

    for (int i = 0; i < width; i++) cout << "#";
    cout << "\nScore: " << score << endl;
}

void input() {
    if (_kbhit()) {
        switch (_getch()) {
            case 'w': 
                if (dir != Down) 
                    dir = Up; 
                    break;
            case 'a': 
                if (dir != Right) 
                    dir = Left; 
                    break;
            case 's': 
                if (dir != Up) 
                    dir = Down; 
                    break;
            case 'd': 
                if (dir != Left) 
                    dir = Right; 
                    break;
        }
    }
}

void logic() {
    for (int i = tLength - 1; i > 0; i--) {
        tailX[i] = tailX[i - 1];
        tailY[i] = tailY[i - 1];
    }

    if (tLength > 0) {
        tailX[0] = x;
        tailY[0] = y;
    }

    // Move head
    switch (dir) {
        case Up:    
            y--; 
            break;
        case Down:  
            y++; 
            break;
        case Right: 
            x++; 
            break;
        case Left:  
            x--; 
            break;
        default: 
            break;
    }

    
    if (x <= 0 || x >= width - 1 || y < 0 || y >= length - 1) {
        gameover = true;
    }

    
    for (int i = 0; i < tLength; i++) {
        if (tailX[i] == x && tailY[i] == y) {
            gameover = true;
        }
    }

    
    if (x == Xfruit && y == Yfruit) {
        score += 10;
        tLength++; 
        Xfruit = rand() % (width - 1);
        Yfruit = rand() % (length - 2);
    }
}

int main() {
    game();
    while (!gameover) {
        construct();
        input();
        logic();
        Sleep(100);  
    }
    cout << "\nGame Over!\n";
    return 0;
}


