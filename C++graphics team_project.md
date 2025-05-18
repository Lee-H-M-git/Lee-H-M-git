# 미로 탈출 게임 - C++ 코드 (Raylib 사용)

```cpp
#include "raylib.h"

// 각 칸의 크기는 60, 배열의 크기는 10칸
const int cell = 60;
const int map = 10;

// 미로의 기본 틀 설정을 위한 배열 선언
// 출구 = 2, 벽 = 1, 이동가능 칸 = 0
int maze[map][map] = {
    {0,1,0,0,0,1,0,1,0,0},
    {0,1,0,1,0,1,0,1,1,0},
    {0,0,0,1,0,0,0,0,1,0},
    {1,1,0,1,1,1,1,0,1,0},
    {0,0,0,0,0,0,1,0,0,0},
    {0,1,1,1,1,0,1,1,1,0},
    {0,1,0,0,0,0,0,0,1,0},
    {0,1,0,1,1,1,1,0,1,0},
    {0,0,0,0,0,0,0,0,1,0},
    {1,1,1,1,1,1,1,0,0,2} 
};

int playerX = 0;    // 플레이어의 x위치 좌표 값
int playerY = 0;    // 플레이어의 y위치 좌표 값

// 기본 상태는 게임을 해야하기 때문에 클리어가 되지 않은 상태
bool gameClear = false;

int main() {
    InitWindow(cell * map, cell * map, "Maze Escape 10x10");
    SetTargetFPS(60);

    while (!WindowShouldClose()) {      
        // 이동 입력 처리
        // 게임 클리어 전까지는 플레이어에게 이동을 입력 받음
        if (!gameClear) {

            // (IsKeyPressed()) 특정 키를 입력해는지 확인하는 함수
            // (playerX,playerY < mapSize - 1) 배열의 범위를 벗어나지 않도록
            // (maze[playerY][playerX + 1] != 1) 오른쪽 칸이 벽(1)이 아니면 이동 가능
            // (playerX > 0) 왼쪽끝이 아니면 이동가능

            if (IsKeyPressed(KEY_RIGHT) && playerX < map - 1 && maze[playerY][playerX + 1] != 1)
                playerX++; // 오른쪽으로 이동
            if (IsKeyPressed(KEY_LEFT) && playerX > 0 && maze[playerY][playerX - 1] != 1)
                playerX--; // 왼쪽으로 이동
            if (IsKeyPressed(KEY_DOWN) && playerY < map - 1 && maze[playerY + 1][playerX] != 1)
                playerY++; // 아래로 이동
            if (IsKeyPressed(KEY_UP) && playerY > 0 && maze[playerY - 1][playerX] != 1)
                playerY--; // 위로 이동
        }

        // 플레이어가 출구 도달시 게임 클리어 처리
        if (maze[playerY][playerX] == 2) {
            gameClear = true;
        }

        // 이미지 생성 시작
        BeginDrawing();
        ClearBackground(WHITE);      // 이동 가능한 칸의 색깔은 흰색

        // 반복문으로 배열을 순회후 해당 값에 맞는 미로를 시각화
        for (int y = 0; y < map; y++) {
            for (int x = 0; x < map; x++) {
                Rectangle cells = { x * cell, y * cell, cell, cell };
                if (maze[y][x] == 1) {
                    DrawRectangleRec(cells, GRAY);               // 벽 색깔은 회색
                }
                else if (maze[y][x] == 2) {
                    DrawRectangleRec(cells, GREEN);              // 출구 색깔은 초록색
                }
                else {
                    DrawRectangleLinesEx(cells, 1, BLACK);       // 각 칸의 경계선의 색깔은 검은색
                }
            }
        }

        // 플레이어 표시
        // (playerX,playerY * cell + cell / 2) 현재 플레이어가 있는 칸의 중심 계산을 위한 식
        DrawCircle(playerX * cell + cell / 2, playerY * cell + cell / 2, cell / 4, RED);

        // 게임 클리어시에 띄울 메시지 창
        if (gameClear) {
            DrawText("GAME CLEAR!", 200, 280, 40, BLUE);
        }

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
```
