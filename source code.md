#include "raylib.h"
#include<iostream>
using namespace std;

struct Ball
{
	float x, y;
	float speedX, speedY;
	float radius;

	void draw() 
	{
		DrawCircle((int) x, (int) y,radius, WHITE);
	}
};

struct paddle
{
	float x, y;
	float speed;
	float width, height;

	Rectangle GetRect()
	{
		return Rectangle{ x - width / 2, y - height / 2 , 10, 100};
	}

	void draw()
	{
		DrawRectangleRec(GetRect() , DARKGREEN);
	}
	
};




int main()
{
	
	InitWindow(800, 600, "pong");
	SetWindowState(FLAG_VSYNC_HINT);

	Ball ball;
	
	ball.x = GetScreenWidth() / 2.0f;
	ball.y = GetScreenHeight() / 2.0f;
	ball.radius = 5;
	ball.speedX = 300;
	ball.speedY = 300;

	paddle leftpaddle;
	leftpaddle.x = 50;
	leftpaddle.y = GetScreenHeight() / 2;
	leftpaddle.width = 10;
	leftpaddle.height = 100;
	leftpaddle.speed = 500;

	paddle rightpaddle;
	rightpaddle.x = GetScreenWidth() - 50;
	rightpaddle.y = GetScreenHeight() / 2;
	rightpaddle.width = 10;
	rightpaddle.height = 100;
	rightpaddle.speed = 500;


	const char* WinnerText = nullptr;
	const char* retry = nullptr;
	const char* player1 = nullptr;
	const char* player2 = nullptr;
	int score1;
	int score2;



	while (!WindowShouldClose())
	{
	

		ball.x += ball.speedX * GetFrameTime();
		ball.y += ball.speedY * GetFrameTime();

		if (ball.y < 0)
		{
			ball.y = 0;
			ball.speedY *= -1;
		}

		if (ball.y > GetScreenHeight())
		{
			ball.y = GetScreenHeight();
			ball.speedY *= -1;
		}

		if (IsKeyDown(KEY_W))
		{

			leftpaddle.y -= leftpaddle.speed * GetFrameTime();
		}
		if (IsKeyDown(KEY_S))
		{

			leftpaddle.y += leftpaddle.speed * GetFrameTime();
		}
		if (IsKeyDown(KEY_UP))
		{

			rightpaddle.y -= rightpaddle.speed * GetFrameTime();
		}
		if (IsKeyDown(KEY_DOWN))
		{

			rightpaddle.y += rightpaddle.speed * GetFrameTime();
		}

		if (CheckCollisionCircleRec(Vector2{ ball.x, ball.y }, ball.radius, leftpaddle.GetRect()))
		{
			if (ball.speedX < 0)
			{
				ball.speedX *= -1.1f;
				ball.speedY = (ball.y - leftpaddle.y) / (leftpaddle.height / 2) * ball.speedX;
				/*const char* scr1 = nullptr;
				score1 = TextToInteger(scr1);
				DrawText(scr1, 50, 20, 20, RED);
				score1++;*/
			}
			
			
		}
		player1 = "PLAYER 1";
		player2 = "PLAYER 2";
		if (CheckCollisionCircleRec(Vector2{ ball.x, ball.y }, ball.radius, rightpaddle.GetRect()))
		{
			if (ball.speedX > 0)
			{
				ball.speedX *= -1.1f;
				ball.speedY = (ball.y - rightpaddle.y) / (rightpaddle.height / 2) * ball.speedX;
				/*const char* scr12 = nullptr;
				score2 = TextToInteger(scr12);
				DrawText(scr12, 600, 20, 20, RED);
				score2++;*/
			}
		    
		
		}
		/*while(CheckCollisionCircleRec(Vector2{ ball.x, ball.y }, ball.radius, rightpaddle.GetRect()))
		{
		}*/
		if (ball.x < 0)
		{
			WinnerText = "Right player wins!";
			retry = "press space to retry";

		}

		if (ball.x > GetScreenWidth())
		{
			WinnerText = "left player wins!", '\n', "press space for retry";
			retry = "press space to retry";

		}
		if (WinnerText && IsKeyPressed(KEY_SPACE))
		{
			ball.x = GetScreenWidth() / 2;
			ball.y = GetScreenHeight() / 2;
			ball.speedX = 300;
			ball.speedY = 300;
			WinnerText = nullptr;
		}
        


		BeginDrawing();
		ClearBackground(BLACK);
		ball.draw();
		leftpaddle.draw();
		rightpaddle.draw();

		if (WinnerText)
		{
			DrawText(WinnerText, 200, GetScreenHeight() / 2 - 30, 40, YELLOW);
			DrawText(retry, 200, GetScreenHeight() - 200, 40, YELLOW);
		}
		DrawText(player1, 50, 30, 20, YELLOW);
		DrawText(player2, 600, 30, 20, YELLOW);
		
       
		

		DrawFPS(10, 10);
		EndDrawing();
	}

	
	CloseWindow();
	return 0;

}
