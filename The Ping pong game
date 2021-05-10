#include<stdio.h>
#include<unistd.h>
#include<curses.h>
#include"conio.h"
// #include<conio.h>

#define ROW_SZ 40
#define COL_SZ 140
#define TRUE 1
#define FALSE 0

#define PADDLE_LEN 7
#define PADDLE_MGN 3

void draw_grid(int ball_r, int ball_c, int p1_r, int p2_r) {
	char a[ROW_SZ][COL_SZ+1];
	for(int r = 0; r < ROW_SZ; r++) {
		for(int c = 0; c < COL_SZ; c++) {
			a[r][c] = ' ';
		}
	}
	for(int r = 0; r < ROW_SZ; r++) {
		a[r][0] = '.';    // left boundary
		a[r][COL_SZ-1] = '.';  // right boundary
		a[r][COL_SZ] = 0; // null terminating the string
	}

	for(int c = 0; c < COL_SZ; c++) {
		a[0][c] = '.';  // top boundary
		a[ROW_SZ-1][c] = '.'; // bottom boundary
	}

	a[ball_r][ball_c] = '*';

	for(int r = p1_r; r < p1_r + PADDLE_LEN; r++) {
		a[r][PADDLE_MGN] = '1';
	}

	for(int r = p2_r; r < p2_r + PADDLE_LEN; r++) {
		a[r][COL_SZ-1 - PADDLE_MGN] = '2';
	}

	clear();
	for(int r = 0; r < ROW_SZ; r++) {
		printw("%s\n", a[r]);
	}
	refresh();
}

int paddle1_hit(int ball_r, int ball_c, int p_r) {
	if(ball_c > PADDLE_MGN)
		return FALSE;
	if(ball_r < p_r)
		return FALSE;
	if(ball_r >= p_r + PADDLE_LEN)
		return FALSE;
	return TRUE;
}

int paddle2_hit(int ball_r, int ball_c, int p_r) {
	if(ball_c < COL_SZ - 1 - PADDLE_MGN)
		return FALSE;
	if(ball_r < p_r)
		return FALSE;
	if(ball_r >= p_r + PADDLE_LEN)
		return FALSE;
	return TRUE;
  }

int move_paddle_up(int p_r, int speed) {
	p_r -= speed;
	if(p_r < 1)
		p_r = 1;
	return p_r;
}

int move_paddle_down(int p_r, int speed) {
	p_r += speed;
	if(p_r + PADDLE_LEN > ROW_SZ - 1)
		p_r = ROW_SZ - 1 - PADDLE_LEN;
	return p_r;
}

int main() {
	int ball_r = ROW_SZ/2;
	int ball_c = COL_SZ/2;

	int ball_dr = 1;
	int ball_dc = 2;
	// angle = ball_dr / ball_dc
	//
	int p1_r = ROW_SZ/2;
	int p2_r = ROW_SZ/2;
	initscr();
	nonblock();
	// Homework
	// Add scores: first to 5 points win
	// Show score
	// >Change angle depending on where it hits the paddle
	// Ball keeps speeding up if the game goes for longer
	// "Smarter AI"
	//   Paddle is moving slower than the ball
	//   how to move paddle when ball keeps changing angle
	while(TRUE) {
		// draw a grid
		draw_grid(ball_r, ball_c, p1_r, p2_r);
		
		// move the ball
		ball_r += ball_dr;
		ball_c += ball_dc;

		if(ball_r <= 0) {
			ball_r = 1;
			ball_dr *= -1;
			// ball_dc remains the same
		}
		if(ball_r >= ROW_SZ-1) {
			ball_r = ROW_SZ-2;
			ball_dr *= -1;
			// ball_dc remains the same
		}
		
		// check for winner
		if(ball_c < 0) {
			// ball_c = 0;
			endwin();
			printf("Player 2 wins\n");
			break;
		}
		if(ball_c > COL_SZ-1) {
			// ball_c = COL_SZ-1;
			endwin();
			printf("Player 1 wins\n");
			break;
		}

		if(paddle1_hit(ball_r, ball_c, p1_r)) {
			ball_c = PADDLE_MGN;
			ball_dc *= -1;
			// ball_dr remains the same
		}

		if(paddle2_hit(ball_r, ball_c, p2_r)) {
			ball_c = COL_SZ - 1 - PADDLE_MGN;
			ball_dc *= -1;
			// ball_dr remains the same
		}

		// move the paddles
		char c = getch(); 
		// scanf("%c", &c);
		switch(c) {
			case 'w':
				p1_r = move_paddle_up(p1_r, 1);
				break;
			case 's':
				p1_r = move_paddle_down(p1_r, 1);
				break;
			case 'i':
				p2_r = move_paddle_up(p2_r, 1);
				break;
			case 'k':
				p2_r = move_paddle_down(p2_r, 1);
				break;
		}
		// if(ball_r > p1_r)
		// 	p1_r = move_paddle_down(p1_r, 1);
		// 	// p2_r += 2;
		// else if(ball_r < p1_r)
		// 	p1_r = move_paddle_up(p1_r, 1);
		// 	// p2_r -= 2;
		// if(ball_r > p2_r)
		// 	p2_r = move_paddle_down(p2_r, 1);
		// 	// p2_r += 2;
		// else if(ball_r < p2_r)
		// 	p2_r = move_paddle_up(p2_r, 1);
		// 	// p2_r -= 2;
		// sleep(1);
		// faster than 1/16 seconds feels like movement
		// 30 frames per second = 30 fps = 1000/30 ~ new frame every 33 ms 
		// usleep(33000);
		usleep(99000);
		// usleep(1000000);
	}
}
