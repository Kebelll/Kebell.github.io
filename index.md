# BATTLESHIP
Dalam post kali ini saya akan membuat sebuah game  

```C
#include <stdio.h>
#include <stdlib.h>

void reset(int *ammo, int *score, char mapcopy[10][10]);
void draw(char mapcopy[10][10], int ammo);
void game(char mapcopy[10][10], int *ammo, int *score);
void splash(void);

char map[][10] = {
    {'S','S','S','S',' ',' ',' ',' ',' ', ' '},
    {' ',' ',' ',' ',' ',' ',' ',' ',' ', ' '},
    {' ','S',' ',' ',' ',' ','S','S',' ', ' '},
    {' ','S',' ',' ',' ',' ',' ',' ',' ', ' '},
    {' ','S',' ','S',' ',' ',' ',' ',' ', ' '},
    {' ',' ',' ','S',' ',' ',' ',' ',' ', ' '},
    {' ',' ',' ','S',' ',' ',' ',' ',' ', ' '},
    {' ',' ',' ',' ',' ',' ',' ',' ',' ', ' '},
    {' ',' ',' ','S','S','S','S','S',' ', ' '},
    {' ',' ',' ',' ',' ',' ',' ',' ',' ', ' '},
};

int main(void)
{
    int ammo;
    int score;
    char mapcopy[10][10];
    reset(&ammo, &score, mapcopy);
}

void reset(int *ammo, int *score, char mapcopy[10][10])
{
    char choice;
    *ammo = 1;
    *score = 0;
    for (int i = 0; i < 10; i++)
    {
        for (int j = 0; j < 10; j++)
        {
            mapcopy[i][j] = map[i][j];
        }
    }
    system("cls");
    printf("=================\n");
    printf("1. Play the game\n");
    printf("2. Exit\n>>> ");
    scanf("%c", &choice);
    getchar();
    if (choice == '2') return;
    game(mapcopy, ammo, score);
}

void draw(char mapcopy[10][10], int ammo)
{
    system("cls");
    printf("Current ammo: %i\n", ammo);
    for (int i = 0; i < 45; i++) printf("="); // print sisi atas boards
    printf("\n");
    printf("|   |");
    for (int i = 0; i < 10; i++) printf(" %i |", i); // penanda koordinat x
    printf("\n");
    for (int i = 0; i < 45; i++) printf("="); // cetak pembatas
    printf("\n");

    //cetak map dan sumbu y
    for (int i = 0; i < 10; i++)
    {
        printf("|");
        printf(" %i |", i);
        for (int j = 0; j < 10; j++)
        {
            printf(" %c |", (mapcopy[i][j] == 'S') ? ' ' : mapcopy[i][j]);
        }
        printf("\n");
        for (int i = 0; i < 45; i++) printf("=");
        printf("\n");
    }
}

void game(char mapcopy[10][10], int *ammo, int *score)
{
    int x, y;
    do
    {
        draw(mapcopy, *ammo);

        //minta koordinat
        printf("Input X [0 - 9][-1 to exit]: ");
        scanf("%i", &x);
        getchar();
        if (x == -1) break;

        printf("Input Y [0 - 9][-1 to exit]: ");
        scanf("%i", &y);
        if (x == -1) break;
        getchar();

        // jika koordinat ngaco
        if (x > 9 || y > 9 || x < -1 || y < -1)
        {
            printf("Invalid coordinates! (Press any key to continue)\n");
            getchar();
        }
        else
        {
            if (mapcopy[y][x] == 'S') // apakah ada perahu
            {
                printf("You hit the target! (Press any key to continue)");
                mapcopy[y][x] = 'x';
                (*ammo)--;
                (*score)++;
                getchar();
            }
            else if (mapcopy[y][x] == ' ') // jika kosong
            {
                printf("You missed the target! (Press any key to continue)");
                mapcopy[y][x] = '-';
                (*ammo)--;
                getchar();
            }
            else if (mapcopy[y][x] == 'x' || map[y][x] == '-')
            {
                printf("Try another target! (Press any key to continue)");
                mapcopy[y][x] = 'x';
                getchar();
            }

            if (*score == 17 && *ammo > 0)
            {
                printf("You won! (Press any key to continue)\n");
                getchar();
                break;
            }
        }
    }
    while (*ammo > 0);

    if (x != -1 && y != -1 && *ammo <= 0)
    {
        printf("Game over, you lose (Press any key to continue)\n");
        getchar();
    }

    reset(ammo, score, mapcopy);
}
```
