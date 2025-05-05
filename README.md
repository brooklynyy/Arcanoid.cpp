Практическая работа №5. Введение в разработку игр с использованием C++ и raylib
1. Инициализация окна и переменных
const int screenWidth = 800;
const int screenHeight = 450;
InitWindow(screenWidth, screenHeight, "Arkanoid на raylib");
SetTargetFPS(60);

Данный код:
Создает окно размером 800x450 пикселей с заголовком "Arkanoid на raylib".
Устанавливает частоту обновления экрана на 60 FPS.
2. Создание игровых объектов
Мяч: начальная позиция в центре экрана, скорость 5 по X и 4 по Y, радиус 20.

Vector2 ballPosition = { (float)screenWidth / 2, (float)screenHeight / 2 };
Vector2 ballSpeed = { 5.0f, 4.0f };
int ballRadius = 20;
Ракетка: начальная позиция 50 пикселей от левого края, высота 100, ширина 20.

Vector2 paddlePosition = { 50.0f, (float)screenHeight / 2 - 50.0f };
Vector2 paddleSize = { 20.0f, 100.0f };
Счет: Изначально 0.

int score = 0;
3. Движения мяча
Мяч движется по X и Y, обновляя свою позицию.

ballPosition.x += ballSpeed.x;
ballPosition.y += ballSpeed.y;
Если мяч касается верхней или нижней границы, он отскакивает (меняем знак скорости по Y).

if ((ballPosition.y >= (screenHeight - ballRadius)) ||
    (ballPosition.y <= ballRadius))
{
    ballSpeed.y *= -1.0f;
}
Если мяч касается правой границы, он отскакивает (меняем знак скорости по X).

if (ballPosition.x >= (screenWidth - ballRadius))
{
    ballSpeed.x *= -1.0f;
}
Если мяч уходит за левую границу, игра сбрасывает позицию мяча, скорость и счет.

if (ballPosition.x < 0)
{
    Vector2 ballPosition = { (float)screenWidth / 2, (float)screenHeight / 2 };
    Vector2 ballSpeed = { 5.0f, 4.0f };
    score = 0;
}
4. Движение ракетки (управление мышью)
Ракетка следует за Y координатой мыши, ограничена рамками экрана.

paddlePosition.y = GetMouseY() - paddleSize.y / 2;
if (paddlePosition.y < 0) paddlePosition.y = 0;
if (paddlePosition.y > screenHeight - paddleSize.y) paddlePosition.y = screenHeight - paddleSize.y;
5. Обнаружение столкновений (мяч и ракетка)
Для реализации удара ракеткой по мячу используется следующий код:

if (CheckCollisionCircleRec(ballPosition, ballRadius,
    (Rectangle) { paddlePosition.x, paddlePosition.y, paddleSize.x, paddleSize.y }))
{
    if (ballSpeed.x < 0)
    {
        ballSpeed.x *= -1.1f;
        score++;
    }
}

В данном коде проверяется столкновение мяча с ракеткой. Если мяч летел влево, он отскакивает и ускоряется на 10%, а счет увеличивается.

6. Отрисовка графики
Для графического представления игры используется следющий код:

BeginDrawing();
ClearBackground(RAYWHITE);
DrawCircleV(ballPosition, ballRadius, MAROON);
DrawRectangleV(paddlePosition, paddleSize, BLACK);
DrawText(TextFormat("Score: %i", score), 10, 10, 20, DARKGRAY);
DrawText("Управляйте ракеткой с помощью мыши", screenWidth - 350, screenHeight - 30, 15, GRAY);
EndDrawing();

Данный код:

Очищает экран перед отрисовкой (ClearBackground).
Рисует мяч (DrawCircleV).
Рисует ракетку (DrawRectangleV).
Выводит счет (DrawText).
Выводит инструкцию (DrawText).
7. Закрытие окна
Для завершения игры используется следующий код

CloseWindow();
return 0;
8. Результат выполнения программы




![12](https://github.com/user-attachments/assets/e97d3fef-ad77-45f0-9afa-a629c3089e03)
![145](https://github.com/user-attachments/assets/355547e0-abab-4ce3-afa8-cd63c830a901)
![1234](https://github.com/user-attachments/assets/e2059e9d-58d0-4794-9c1f-635e453cb5cc)
