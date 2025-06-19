```c++
#include <iostream>  
#include <vector>  
#include <queue>  
  
#define X_MAZE_SIZE 8  
#define Y_MAZE_SIZE 6  
  
#define NOT_VISITED -1  
  
struct coordinate{  
    int x;  
    int y;  
};  
  
  
  
  
char MAZE[Y_MAZE_SIZE][X_MAZE_SIZE] = {  
//      x  0   1   2   3   4   5   6   7  
//                                               y  
        { 'w','w','w','w','w','w','w','w' },//   0  
        { 'w',' ',' ','w',' ',' ',' ','w' },//   1  
        { 'w',' ','w','w',' ','w',' ','w' },//   2  
        { 'w',' ',' ',' ',' ','w',' ','w' },//   3  
        { 'w','w','w','w',' ','w',' ','w' },//   4  
        { 'w',' ',' ',' ',' ',' ',' ','w' }//    5  
};  
  
std::vector<coordinate> directions = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};  
  
  
void wave(coordinate start, coordinate end){  
    std::queue<coordinate> q;  
    q.push(start);  
    std::vector<std::vector<int>> wave_matrix(Y_MAZE_SIZE, std::vector<int>(X_MAZE_SIZE, NOT_VISITED));  
  
    wave_matrix[start.y][start.x] = 0;  
  
    while(!q.empty()){  
        coordinate curr = q.front(); q.pop();  
  
        for (auto [i, j]: directions){  
            int x_i = curr.x + i;  
            int y_j = curr.y + j;  
            if (x_i < 0 || y_j < 0 || x_i >= X_MAZE_SIZE || y_j >= Y_MAZE_SIZE) continue;  
  
            if (wave_matrix[y_j][x_i] == NOT_VISITED && MAZE[y_j][x_i] == ' '){  
                wave_matrix[y_j][x_i] = wave_matrix[curr.y][curr.x] + 1;  
                q.emplace(x_i, y_j);  
            }        }    }}
```