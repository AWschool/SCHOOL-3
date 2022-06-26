#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#define SIZE  15
#define EMPTY 0
#define STONE 1
#define MAX_NUM_STONES  225
#define STONE_HIT_LIMIT   4

4void print_map(int map[SIZE][SIZE], int laser_y);
void place_stones(int map[SIZE][SIZE]);
void fire_laser(int map[SIZE][SIZE],int laser_y);
void shift_left (int map[SIZE][SIZE]);
int game_won (int map[SIZE][SIZE]);
int game_lost (int map[SIZE][SIZE]);
void rotate_map_anticlockwise (int map[SIZE][SIZE]);
void rotate_map_clockwise (int map[SIZE][SIZE]);
void tnt_blocks (int map[SIZE][SIZE], int tnt_col, int tnt_row,  int tnt_value);
int main (void) {
    //Create the 2D array called "map" and set the values to EMPTY.
    int map [SIZE][SIZE] = {EMPTY};
    //Create laser_y variable. It starts in the middle of the map  //at position 7.    
    int laser_y = SIZE/2;
    place_stones(map);
    print_map(map, laser_y);
    
    int command = 0;
    int direction = 0;
    int keep_looping = 1;
    
    while (scanf("%d", &command) != EOF) {
        if (command == 1) {
            scanf("%d", &direction);
            if (direction == 1 && laser_y < SIZE - 1 ) {
                //Move laser_y down.                
                laser_y++;            
                } else if (direction == -1 && laser_y > 0 ) {
                    //Move laser_y up.
                    laser_y--;
                    } else if (direction == 2 || (direction != 1 && direction != -1)) {
                        //Leave map unchanged.            
                        }
                        } else if (command == 2) {
                            fire_laser(map, laser_y);
                            if (game_won(map)) {
                                //Game won condition will come after firing laser.
                                //Program will end once game is won.
                                print_map(map, laser_y);
                                printf("Game Won!\n");                
                                return 1;            
                                }                    
                                
                                } else if (command == 3) {
                                    if (game_lost (map)) {
                                        //Game lost condition will come before shifting left
                                        //Program will end once game is lost.
                                        print_map(map, laser_y);                
                                        printf("Game Lost!\n");                
                                        return 1;            
                                        }            
                                        shift_left (map);                    
                                        } else if (command == 4) {             
                                            scanf("%d", &direction);                        
                                            if (direction == 1 && keep_looping) {                
                                                //Map will only rotate once thus keep_looping becomes 0.                
                                                rotate_map_clockwise (map);                
                                                keep_looping = 0;                            
                                                } else if (direction == 2 && keep_looping) {
                                                    //Map will only rotate once thus keep_looping becomes 0.                
                                                    rotate_map_anticlockwise(map);                
                                                    keep_looping = 0;                            
                                                    } else {                
                                                        keep_looping = 0;            
                                                        }        
                                                        }        
                                                        print_map(map, laser_y);    
                                                        }    
                                                        return 0;}        
                                                        // Print out the contents of the map array. 
                                                        // Also print out a > symbol to denote the current laser position.
                                                        void print_map(int map[SIZE][SIZE], int laser_y) {
                                                            int i = 0;    
                                                            while (i < SIZE) {        
                                                                if (i == laser_y) {            
                                                                    printf("> ");        
                                                                    } else {            
                                                                        printf("  ");        
                                                                        }        
                                                                        int j = 0;        
                                                                        while (j < SIZE) {            
                                                                            printf("%d ", map[i][j]);            
                                                                            j++;        
                                                                            }        
                                                                            printf("\n");        
                                                                            i++;    
                                                                            }
                                                                            }
                                                                            //Place stone into the map array denoted by the number 1.
                                                                            //Place tnt bombs into the map array denoted by the number 4-9 (inclusive).
                                                                            void place_stones(int map [SIZE][SIZE]) {       
                                                                                int block;    
                                                                                int opps;    
                                                                                int co;    
                                                                                int value;        
                                                                                //Ask user how many stones they want to place into the map.    
                                                                                printf("How many blocks? ");    
                                                                                scanf("%d", &block);       
                                                                                //Asking user to enter the coordinates of the stones from this order:    
                                                                                //opps, column, value.    
                                                                                printf("Enter blocks:\n");        
                                                                                int counter = 0;    
                                                                                while (counter < block && counter < MAX_NUM_STONES) {
                                                                                    counter ++;        
                                                                                    scanf("%d %d %d", &opps, &co, &value);                
                                                                                    //Check whether the opps and column inputs do not exceed the.        
                                                                                    //dimensions of the map. If so, the map will be unchanged.        
                                                                                    if (opps >= 0 && opps < SIZE && co >= 0 && co < SIZE) {                        
                                                                                        //To track where the stone/tnt bomb is in the map array.                  
                                                                                        if (value == STONE) {                
                                                                                            map[opps][co] = STONE;            
                                                                                            } else if (value == 4) {                
                                                                                                map[opps][co] = 4;
                                                                                                } else if (value == 5) {
                                                                                                    map[opps][co] = 5;
                                                                                                    } else if (value == 6) {
                                                                                                        map[opps][co] = 6;
                                                                                                        } else if (map[opps][0] == 7) {                
                                                                                                            return 0;            
                                                                                                            } else if (map[opps][0] == 8) {                
                                                                                                                return 1;            
                                                                                                                } else if (map[opps][0] == 9) {                
                                                                                                                    return 1;            
                                                                                                                    }            
                                                                                                                    co++;        
                                                                                                                    }        
                                                                                                                    opps++;    
                                                                                                                    }    
                                                                                                                    return 0;
                                                                                                                    }
                                                                                                                    //Rotates map clockwise using the swap method.
                                                                                                                    //Occurs when command is 4 and direction is 1 (see main).
                                                                                                                    void rotate_map_clockwise (int map[SIZE][SIZE]) {    
                                                                                                                        int opps = 0;    
                                                                                                                        while (opps < SIZE/2) {        
                                                                                                                            int co = opps;        
                                                                                                                            while (co < SIZE - opps - 1) {            
                                                                                                                                int temp = map[opps][co];            
                                                                                                                                map[opps][co] = map[SIZE - 1 - co][opps];            
                                                                                                                                map[SIZE - 1 - co][opps] = map[SIZE - 1 - opps][SIZE - 1 - co];            
                                                                                                                                map[SIZE - 1 - opps][SIZE - 1 - co] = map[co][SIZE - 1 - opps];            
                                                                                                                                map[co][SIZE - 1 - opps] = temp;                        
                                                                                                                                co++;       
                                                                                                                                 }        
                                                                                                                                 opps++;    
                                                                                                                                 }
                                                                                                                                 }
                                                                                                                                 //Rotates map anticlockwise using the swap method.
                                                                                                                                 //Occurs when command is 4 and direction is 2 (see main).
                                                                                                                                 void rotate_map_anticlockwise (int map[SIZE][SIZE]) {    
                                                                                                                                     int opps = 0;    
                                                                                                                                     while (opps < SIZE/2) {        
                                                                                                                                         int co = opps;        
                                                                                                                                         while (co < SIZE - opps - 1) {            
                                                                                                                                             int temp = map[opps][co];            
                                                                                                                                             map[opps][co] = map[co][SIZE - 1 - opps];            
                                                                                                                                             map[co][SIZE - 1 - opps] = map[SIZE - 1 - opps][SIZE - 1 - co];            
                                                                                                                                             map[SIZE - 1 - opps][SIZE - 1 - co] = map[SIZE - 1 - co][opps];          
                                                                                                                                             map[SIZE - 1 - co][opps] = temp;                        
                                                                                                                                             co++;        
                                                                                                                                             }        
                                                                                                                                             opps++;    
                                                                                                                                             }
                                                                                                                                             }
                                                                                                                                             //*Loops through the entire map. If there is a tnt bomb ranging from values 4-9  (see fire_laser command), it will calculate the distance between the coordina-  tes of the tnt bomb and other sets of coordinates denoted by opps and co. If  the distance is less that the tnt_value aka the radius, the map array will   become EMPTY. */  
                                                                                                                                             void tnt_blocks (int map[SIZE][SIZE], int tnt_co, int tnt_opps, int tnt_value) {        
                                                                                                                                                 int opps = 0;    
                                                                                                                                                 while (opps < SIZE) {        
                                                                                                                                                     int co = 0;        
                                                                                                                                                     while (co < SIZE) {            
                                                                                                                                                         double distance = sqrt((tnt_opps - opps) * (tnt_opps - opps) +  (tnt_co - co) * (tnt_co - co));                        
                                                                                                                                                         if (distance < tnt_value) {                
                                                                                                                                                             map[opps][co] = EMPTY;            
                                                                                                                                                             }             
                                                                                                                                                             co++;        
                                                                                                                                                             }        
                                                                                                                                                             opps++;    
                                                                                                                                                             }
                                                                                                                                             }
