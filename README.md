# A Simple Snake Game 🐍
- By Jaydev Dhabaliya 😎
- Dhruvir S Mayavat 😎
- Rashi Krishnani 👧🏻
- Rudrapratap Singh 😎

# Description 🔍

-The Snake Game in C++ is a classic arcade-style game built to demonstrate fundamental programming concepts such as game loops, user input handling, and collision detection. What makes this version unique is its 
 clean, modular code structure, making it easy to modify and extend with additional features like scoring, speed variations, or graphical enhancements.
-We will be using c++ as the coding language, and here is the step to step process to make the snake.

# Code Overview ⌨️🖥️

## Include Libraries 📚: 

#include <iostream> - OS Agnostic // for doing i/p and o/p operations  
#include <conio.h> - Non-OS Agnostic // for doing console i/p and o/p operations  
#include <windows.h> - Non-OS Agnostic // for system("cls")  

```
    #include <iostream>  
    #include <conio.h>
    #include <windows.h>
    using namespace std;
```
    

## Declaring Variables 🔤 :
Values which will help in setting a graphical representation of the snake
- making a constant width and length of 25 units.
- **bool gameover** - to end the game when one of the many conditions is met.
- **x,y and Xfruit,Yfruit** - Position of the head of the snake and a fruit respectively.
- **int score** - Declaring score as a variable.
- **int tailX[100] and tailY[100]** - to store the two dimensional position of elements in the tail.
- **int tLength** - declaring the length of the tail as a variable.
- **enum direction** - to declare enum of directions like left,up,down and right, and also stop.

```
bool gameover;  
const int width = 25;  
const int length = 25;  
int x, y, Xfruit, Yfruit, score;  
int tailX[100], tailY[100];  
int tLength;  
enum direction { Stop = 0, Up, Down, Left, Right };  
direction dir;  
```

  

## Writing the key functions 🗝️:

there are 4 key functions necessary for the snake program:

- **void game()** - To start/end the game  
- **void construct()** - To construct a graphical representation of border, snake and fruit.  
- **void input()** - To give inputs (ex. w,a,s,d) to move the snake.  
- **void logic()** - To construct the logic of the game ie. when and how to increase the length of the tail, game is over when the snake collides with the border,etc.  
   
### void game() 🎮:- 

- We will set gameover to be false ( to initiate the game )
- We will set dir to Stop intitially ( snake will be stationary unless given command ) and in the middle of the map.
- As the position of the fruit needs to be randomized, we will use rand() to give random values of the fruit, under the border of the map.
- set score and Tlength to zero, to initialize the score and the length of the tail of the snake to be zero.  

```
void game() {
    gameover = false;
    dir = Stop;
    x = width / 2;
    y = length / 2;
    Xfruit = rand() % (width - 1);
    Yfruit = rand() % (length - 2);
    score,tLength = 0;
}
```
### void construct() 🧱:-

- to clear the console screen, we use "**system("cls");**"
```
system("cls");
```
- Now we make a border made up of '#' and below the border we also write the score. the code for making the border under the length and width given for the map is as follows:-
```
for (int i = 0; i < width; i++) {
        cout << "#";
    }
    cout << endl;

    for (int i = 0; i < (length - 1); i++) {
        for (int j = 0; j < width; j++) {
            if (j == 0 || j == (width - 1)){
                cout << "#";
            }
            else {
                cout << " ";
            }
        }
        cout << endl;
    }
    

    for (int i = 0; i < width; i++){
    cout << "#";
    }
    cout << "\nScore: " << score << endl;
```
- Now, we will graphically represent the position of the snake's head and the fruit.  
- Also we will graphicallly represent the elements of the growth of tail.  
- We will represent 'W' as the head, '$' as the fruit and 'X' as the tail.  
```
// Updating previous code:
for (int i = 0; i < width; i++) {
        cout << "#";
    }
    cout << endl;

    for (int i = 0; i < length - 1; i++) {
        for (int j = 0; j < width; j++) {
            if (j == 0 || j == width - 1)
                cout << "#";
            else if (j == x && i == y)
                cout << "W"; 
            else if (j == Xfruit && i == Yfruit)
                cout << "$"; 
            else {
                bool printTail = false;
                for (int k = 0; k < tLength; k++) {
                    if (tailX[k] == j && tailY[k] == i) {
                        cout << "X"; 
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

for (int i = 0; i < width; i++){
    cout << "#";
    }
    cout << "\nScore: " << score << endl;
```
- in this part of the above code:
```
else {
                bool printTail = false;
                for (int k = 0; k < tLength; k++) {
                    if (tailX[k] == j && tailY[k] == i) {
                        cout << "X"; 
                        printTail = true;
                        break;
                    }
                }
                if (!printTail)
                    cout << " ";
            }
```
- Here, we declare bool printTail, to specify if the specific cell at the position (j,i) is printed or not.  
- By default, we take printTail to be false, as we have not printed a tail segment anywhere.  
- After traversing, when (j,i) is equal to the position at tailX and tailY, we print the tail as 'X', and to specify that the cell is printed, we set printTail to be true.  
- Any other position will be printed as " ".

### void input() :-
- Here, we basically set dir to the respective enumerator by giving an input of w,a,s or d.  
- for ex. if we type 'w', dir is set to 'Up', if we type 'a', dir is set to 'Left'.
- also, to avoid going directly to opposite direction, we add a condition that if dir is assigned to a specific direction, giving a command of going completely to the opposite direction will not be executed.
- This can be done using switch-case.
- This is where the properties of conio.h library will be used.  
```
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
```
### void logic() 🧠:-
- We have to make the elements in the tail to follow a path of the previous tail segments.  
- This can be done by implementing a for loop in which we set the position of the current tail segment to the position of the previous tail segment and then, setting the first tail segment to be the previous position of the head of the snake.  
```
for (int i = tLength - 1; i > 0; i--) {
        tailX[i] = tailX[i - 1];
        tailY[i] = tailY[i - 1];
    }

    if (tLength > 0) {
        tailX[0] = x;
        tailY[0] = y;
    }
```
- Now, we update the position of the head of the snake, depending on the current value of dir.
- for ex. if dir = Up, y = y-1; if dir = Right, x = x + 1;

```
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
```

- Now, we focus on how the game will end.
- Game can be ended by setting gameover to true, if one of the few conditions is met.
- there are 2 conditions: whether the snake collides with the map border or it collides with one of its tail segment. Thus we write the following code:
```
// if the positions x and y exceed beyond the limit of the border width or length respectively, we set gameover to be true.
if (x <= 0 || x >= width - 1 || y < 0 || y >= length - 1) {
    gameover = true;
}
//traversing the positions of each tail, if the position of the head of the snake is the same as the position of any other tail segment, we set gameover to be true.
for (int i = 0; i < tLength; i++) {
    if (tailX[i] == x && tailY[i] == y) {
        gameover = true;
    }
}
```
- when the snake eats the fruit, ie. when the position of the head of the snake is equal to that of the fruit, we increase the score and the length of the tail, and also set a new position for the fruit.
```
if (x == Xfruit && y == Yfruit) {
        score += 10;
        tLength++; 
        Xfruit = rand() % (width - 1);
        Yfruit = rand() % (length - 2);
}
```
- Thus, the final complete code for the function logic().
```
void logic() {
    for (int i = tLength - 1; i > 0; i--) {
        tailX[i] = tailX[i - 1];
        tailY[i] = tailY[i - 1];
    }

    if (tLength > 0) {
        tailX[0] = x;
        tailY[0] = y;
    }

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
```
## Running the program 🚀

- First of all, we will implement the function **game()**, which will set the parameters of the grpahics of the game.  
- while gameover is false:  
  we will implement **construct()** to construct the graphics of the game,  
  we will implement **input()** to type which key alphabets to assign dir to a specific direction(Up,Down,Left,Right)  
  we will then implement **logic()** to give logic to the program and how it works.  
  we will write Sleep(100) for the program to update at a moderate time interval, that makes the game playable.  
- when gameover becomes true, it skips the while loop and gives the output that the game has ended.  

```
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
```

