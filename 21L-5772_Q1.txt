#include<iostream> 
#include<string>
#include<fstream>
#include<cstdlib> 
#include<ctime>  
#include<stack>
#include<queue>
using namespace std;

int healthpoints = 20;
double score = 0;
int highScore = 0;
string name = "\0";
const int MAX_TRIES = 7;

//HELPER CLASS FOR INVENTORY
class node
{
    public:
        node* next;
        int points;
        string description;
    public:
        node()
        {
            next = nullptr;
            points =0;
            description ="\0";
        }
        node(int p , string d)
        {
            next = nullptr;
            points = p;
            description = d;
        }
};

class AVLNode {
public:
    int key;
    int key2;
    int height;
    string name;
    AVLNode *left;
    AVLNode *right;

    AVLNode(int k , string n) {
        key = k;
        height = 1;
        name = n;
        left = right = nullptr;
    }
    AVLNode( int a , int b )
    {
        key = a;
        key2 = b;
        height = 1;
        name = "\0";
        left = right = nullptr;
    }
};

class KeyQueue
{
    node* front;
    node* back;

    public:
    KeyQueue()
    {
        front = nullptr;
        back = nullptr;
    }
    void push(string name , int quantity)
    {
        node* n = new node(quantity,name);
        if(front == nullptr)
        {
            back = n;
            front = n;
            return;
        }
        back->next = n;
        back = n;
    }
    void pop()      /*this will decrement the number of keys*/
    {
        front->points -= 1;
    }
    node* top()
    {
        return front;
    }
    void printQueue()
    {
        node* temp = front;
        while ( temp!= nullptr )
        {
           cout << "\n------------------KEY INVENTORY--------------------\n";
            cout << "Item Name: " << temp->description << "\n";
            cout << "Key Number: " << temp->points << "\n";
            cout << "\n--------------------------------------\n";
            temp = temp->next;
        }
        cout << "\n";
    }
};

class MoveInfo
{
    private:
        string name;
        string type;
        int attP;
    public:
        MoveInfo(string n , string t , int p)
        {
            name =n;
            type = t;
            attP = p;
        }
        string getName()
        {
            return name;
        }
        string getType()
        {
            return type;
        }
        int getPower()
        {
            return attP;
        }
};

class puzzle
{
    private:
        char letter;
        string name;
        string* words;
        int no_of_worng_guess;
        string word;
    //helper function;
    public:
    puzzle()
    {
        words = new string[15];
    }

    int letterFill(char guess, string secretword, string& guessword)
    {
        int i;
        int matches = 0;
        int len = secretword.length();
        for (i = 0; i < len; i++)
        {
            
            if (guess == guessword[i])
                return 0;
        
        
            if (guess == secretword[i])
            {
                guessword[i] = guess;
                matches++;
            }
        }
        return matches;
    }
    void fileIO(string words[])
    {
        int count = 0;
        string st;
        ifstream fin("words.txt");
        if (fin.is_open())
        {
            while (!fin.eof())
            {
                getline(fin, st);
                words[count] = st;
                count++;
            }
            fin.close();
        }
        else
            cout << "NO file found\n";
    }
    bool solvePuzzle()
    {
        string name;
        char letter;
        string words[15];
        fileIO(words);
        int num_of_wrong_guesses = 0;
        string word;

        srand(time(NULL));
        int n = rand() % 10;
        word = words[n];


        string unknown(word.length(), '*');


        cout << "\n\nWelcome to Locked Door Puzzle...";
        cout << "\n\nGuess the word to open the door.";
        cout << "\n\nYou have to type only one letter in one try";
        cout << "\n\nYou have " << MAX_TRIES << " tries to try and guess the word.";
        cout << "\n";

        
        while (num_of_wrong_guesses < MAX_TRIES)
        {
            if(num_of_wrong_guesses==0)
            {
                cout<<"\t\t\t\t\t\t\n";
            }
            if( num_of_wrong_guesses==1)
            {
                cout<<"\t\t\t\t\n";
                cout<<"\t\t\t\t|\n";
            }
            else if(num_of_wrong_guesses==2)
            {
                cout<<"\t\t\t\t| \n";
                cout<<"\t\t\t\tO\n";
            }
            else if(num_of_wrong_guesses==3)
            {
                cout<<"\t\t\t\t| \n";
                cout<<"\t\t\t\tO\n";
                cout<<"\t\t\t\t|\n";
            }
            else if(num_of_wrong_guesses==4)
            {
                cout<<"\t\t\t\t | \n";
                cout<<"\t\t\t\t O \n";
                cout<<"\t\t\t\t/| \n";
            }
            else if(num_of_wrong_guesses==5)
            {
                cout<<"\t\t\t\t | \n";
                cout<<"\t\t\t\t O   \n";
                cout<<"\t\t\t\t/|\\  \n";
            }
            else if(num_of_wrong_guesses==6)
            {
                cout<<"\t\t\t\t  | \n";
                cout<<"\t\t\t\t  O \n";
                cout<<"\t\t\t\t /|\\ \n";
                cout<<"\t\t\t\t / \n";
            }   
            else if(num_of_wrong_guesses==7)
            //else
            {
                cout<<"\t\t\t\t  | \n";
                cout<<"\t\t\t\t  O \n";
                cout<<"\t\t\t\t /|\\ \n";
                cout<<"\t\t\t\t / \\ \n";
            }
        
            cout << "\n\n" << unknown;
            cout << "\n\nGuess a letter: ";
            cin >> letter;
            
            if (letterFill(letter, word, unknown) == 0)
            {
                cout << endl << "Whoops! That letter isn't in there!" << endl;
                num_of_wrong_guesses++;
            }
            else
            {
                cout << endl << "You found a letter! Isn't that exciting!" << endl;
            }
            
            cout << "You have " << MAX_TRIES - num_of_wrong_guesses;
            cout << " guesses left." << endl;
        
            if (word == unknown)
            {
                cout << word << endl;
                cout << "Yeah! You got it!";
                return true;
            }
        }
        if (num_of_wrong_guesses == MAX_TRIES)
        {
            cout<<"\t\t\t\t  | \n";
            cout<<"\t\t\t\t  O \n";
            cout<<"\t\t\t\t /|\\ \n";
            cout<<"\t\t\t\t / \\ \n";
            cout << "\nSorry, you lose...you've been hanged." << endl;
            cout << "The word was : " << word << endl;
            return false;
        }
        cin.ignore();
        cin.get();
        return false;
    }

};

class combatSystem
{
    private:
        string name;
        int health;
        stack<MoveInfo> m;
    public:
        combatSystem()
        {
            name = "\0";
            health = 0;
        }
        combatSystem(string n, int h )
        {
            health = h;
            name = n;
        }
        void AddMove( MoveInfo newM )
        {
            m.push(newM);
        }
        MoveInfo NextMove()
        {
            MoveInfo m1 = m.top();
            m.pop();
            return m1;
        }
        bool movesAvailable()
        {
            return !m.empty();
        }
        bool COMBAT ( combatSystem player , combatSystem enemy)
        {
            int playerScore = 0;
            int enemyScore = 0;

            while (player.movesAvailable() && enemy.movesAvailable())
            {
                MoveInfo playerM = player.NextMove();
                MoveInfo enemyM = enemy.NextMove();

                if (playerM.getType() == "defense" && enemyM.getType() == "attack")
                {
                    enemyScore += enemyM.getPower();
                }
                if (playerM.getType() == "attack" && enemyM.getType() == "defense")
                {
                    playerScore += playerM.getPower();
                }
                if (playerM.getType() == "attack" && enemyM.getType() == "attack")
                {
                    if (playerM.getPower() > enemyM.getPower())
                    {
                        playerScore +=playerM.getPower();
                    }
                    else
                    {
                        enemyScore += enemyM.getPower();
                    }
                }
            }
            if ( playerScore > enemyScore )
            {
                cout << "Player has Won.\n";
                return 1;
            }
            else
            {
                cout << "Enemy has won.\nChoose alternate path.\n";
                return 0;
            }
        }
};

class inventory
{
    private:
        node* storage;
    public:
        inventory()
        {
            storage = nullptr;
        }
        void addToInventory(int p , string d)
        {
            node* newNode = new node(p,d);

            if ( storage == nullptr )
            {
                storage = newNode;
                return;
            }
            node* temp = storage;
            while( temp->next != nullptr )
            {
                temp = temp->next;
            }
            temp->next = newNode;
        }

        void displayLoadout()
        {
            node* temp = storage;
            while ( temp != nullptr )
            {
                cout << "--------------------------------------\n";
                cout << "Item Name: " << temp->description << "\n";
                cout << "Item Point: " << temp->points << "\n";
                cout << "--------------------------------------\n";

                temp = temp->next;
            }
        }
};

class BSTNode
{
    public:
        string data;
        BSTNode* left;
        BSTNode* right;

        BSTNode()
        {
            data = "\0";
            left = right = nullptr;
        }
};

class MazeMovement
{
    private:
        char** maze;
        int size;

        //for obstacles
        int rObstacle;
        int cObstacle;

        //for locked doors
        int rLocked;
        int cLocked;

        //for keys
        int rKeys;
        int ckeys;

        //for enemies
        int rEn;
        int cEn;

        KeyQueue q;
        inventory I;

        combatSystem player;
        combatSystem enemy;
        //Helper Functions
        void printMaze()
        {
              for ( int i = 0 ; i < size ; i++ )
            {
                for ( int j = 0 ; j< size ; j++ )
                {
                    cout << "|" << maze[i][j];

                }
                cout << "|\n";
            }
        }
        void defaultLoadout()
        {
            I.addToInventory(50,"Sword");
            I.addToInventory(20,"Health Stone");
            I.addToInventory(0,"Key");
            q.push("Key" , 0);
        }
        void addKeyCount()
        {
            q.top()->points += 1;
        }
        void openLockedDoors()
        {
            q.top()->points -=1;
            cout << "Door has been opened using the key.\n";
        }
       //Helper Functions
    public:
        void KeyManagement()
        {
            defaultLoadout();
            cout << "\nYour default loadout has been set.\n";
        }
        MazeMovement()
        {
            maze = nullptr;
            size = 0;
            rObstacle = 0;
            cObstacle = 0;
            rLocked = 0;
            cLocked = 0;
            rKeys = 0;
            ckeys = 0;
            rEn = 0;
            cEn = 0;
        }
        void setPlayerAndEnemyMoves()
        {
            combatSystem player("Luffy" , 100);
            combatSystem enemy("Blackbeard" , 100);

            player.AddMove(MoveInfo("Gomu-Gomu No Pistol" , "attack" , 100));
            player.AddMove(MoveInfo("Gomu-Gomu No Jet Culverin" , "attack" , 15));
            player.AddMove(MoveInfo("Gomu-Gomu No King Cobra" , "attack" , 20));
            player.AddMove(MoveInfo("Gear 4: Tank Man" , "defense" , 20));
            player.AddMove(MoveInfo("Evade" , "defense" , 0));

            enemy.AddMove(MoveInfo("Seaquake" , "attack", 10));
            enemy.AddMove(MoveInfo("Dark-End" , "attack", 14));
            enemy.AddMove(MoveInfo("Dark Vortex" , "attack", 20));
            enemy.AddMove(MoveInfo("Black-Hole Liberation" , "attack", 21));
            enemy.AddMove(MoveInfo("Shadow Sneak" , "defense", 0));
        }
        bool battle()
        {
            combatSystem main;
            if (main.COMBAT(player,enemy) == 1 )
            {
                return true;
            }
            else
            {
                return false;
            }
        }
        void generateMaze()
        {
            cout << "\n-------------------------------------------------\n";
            int modPart = 0;
            cout << "\n-------------------------------------------------\n";
            cout << "-----------------Enter Maze Size-------------------\n";
            cout << "Size: ";
            cin >> size;

            maze = new char*[size];
            for ( int i = 0 ; i < size; i++ )
            {
                maze[i] = new char [size];
            }
            
            for ( int i = 0 ; i < size ; i++ )
            {
                for ( int j = 0 ; j< size ; j++ )
                {
                    maze[i][j] = '*';
                }
            }

            //OBSTACLE SETTING
            //----------------------------------------------
            srand(time(0));
            modPart = size % 5;
            if ( modPart == 0 )
            {
                cout << "\nNo Obstacles in the maze.\n";
            }

            if ( modPart == 1 )
            {    
                rObstacle = rand() % size;
                cObstacle = rand() % size;
                maze[rObstacle][cObstacle] = 'O';
            }
            
            if ( modPart > 1)
            {
                for ( int i = 0 ; i < modPart ; i++) 
                {
                    rObstacle = rand() % size;
                    cObstacle = rand() % size;  
                    maze[rObstacle][cObstacle] = 'O';
                    rObstacle = cObstacle = 0;
                }
            }
            //---------------------------------------------------

            //LOCKED DOOR SETTING
            //--------------------------------------------------
            if ( modPart == 0 )
            {
                cout << "\nNo Locked in the maze.\n";
            }

            if ( modPart == 1 )
            {    
                rLocked = rand() % size;
                cLocked = rand() % size;
                maze[rLocked][cLocked] = 'D';
            }
            
            if ( modPart > 1)
            {
                for ( int i = 0 ; i < modPart ; i++) 
                {
                    rLocked = rand() % size;
                    cLocked = rand() % size;  
                    maze[rLocked][cLocked] = 'D';
                    rLocked = cLocked = 0;
                }
            }
            //--------------------------------------------------
            //KEYS SETTING.
            int no_of_keys = modPart;
            cout << no_of_keys << endl;
            if ( modPart == 0 )
            {
                cout << "\nNo Keys in the maze.\n";
            }

            if ( modPart == 1 )
            {    
                rKeys = rand() % size;
                ckeys = rand() % size;
                maze[rKeys][ckeys] = 'K';
            }
            
            if ( modPart > 1)
            {
                for ( int i = 0 ; i < modPart ; i++) 
                {
                    rKeys = rand() % size;
                    ckeys = rand() % size;  
                    maze[rKeys][ckeys] = 'K';
                    rKeys = ckeys = 0;
                }
            }
            //--------------------------------------------------
            //ENEMIES SETTING
            if ( modPart == 0 )
            {
                cout << "\nNo Enemies in the maze.\n";
            }

            if ( modPart == 1 )
            {    
                rEn = rand() % size;
                cEn = rand() % size;
                maze[rEn][cEn] = 'E';
            }
            
            if ( modPart > 1)
            {
                for ( int i = 0 ; i < modPart ; i++) 
                {
                    rEn = rand() % size;
                    cEn = rand() % size;  
                    maze[rEn][cEn] = 'E';
                    rEn = cEn = 0;
                }
            }
            //--------------------------------------------------
            maze[0][0] = 'L';
            maze[size-1][size-1] = 'T';
            cout << "\n-------------------------------------------------\n";
            cout << "\n---------------Generated Maze--------------------\n";
            cout << "\n";
            for ( int i = 0 ; i < size ; i++ )
            {
                for ( int j = 0 ; j< size ; j++ )
                {
                    cout << "|" << maze[i][j];

                }
                cout << "|\n";
            }
        }     
        // U for up , D for down , L for left, R for right
        void movement( char &key )
        {
            puzzle p1;
            ofstream path;
            path.open("path.txt" , ios :: app);
            cout << "\n-------------------------------------------------\n";
            cout << "-----------------Enter Your Name-------------------\n";
            cin >> name;
            cout << "\n-------------------------------------------------\n";
            KeyManagement();
            generateMaze();
            int pRow = 0;
            int pCol = 0;
            maze[pRow][pCol] = 'P';
            cout << "\nWhere do you want to move?\n";
            cout << "U for up.\nD for down.\nL for left.\nR for right.\n";

            while ( pRow != size - 1 || pCol != size -1 )
            {
                if( healthpoints == 0 )
                {
                    cout << "Healthpoints dropped to 0. Ending game.\n";
                    break;
                }
                cout << "Your score is: "<< highScore << "\n";
                cout << "Available Moves are: " << healthpoints << "\n";
                cout << "\n-------------------------------------\n";
                cout << "->Enter your choice: ";
                cin >> key;
                cout << "\n-------------------------------------\n";
               

                if ( key == 'U' || key == 'u')
                {
                    if ( pRow <= 0 )
                    {
                        cout << "->Can not move there.\n";
                        continue;
                    }
                    //obstacle
                    if ( maze[pRow - 1][pCol] == 'O')
                    {
                        cout << "Obstacle found.\nChoose Alternate path.\n";
                        highScore -= 5;
                        healthpoints -=1;
                        continue;
                    }
                    //locked doors
                    if ( maze[pRow - 1][pCol] == 'D')
                    {
                        cout << "Locked Door found.\n";
                        if ( q.top()->points >= 1)
                        {
                            cout <<"Using key to open the door.\n";
                            openLockedDoors();
                            maze[pRow][pCol] = '.';
                            pRow--;
                            maze[pRow][pCol] = 'L';
                            highScore += 10;
                        }
                        else
                        {   
                            cout << "Since you have no keys, a guessing puzzle will be given to you.\nIf you guess the word correctly,you will proceed.\n";
                            if ( p1.solvePuzzle() == 1 )
                            {
                                maze[pRow][pCol] = '.';
                                pRow--;
                                maze[pRow][pCol] = 'L';
                                highScore += 10;
                            }
                            else
                            {
                                highScore -= 10;
                                healthpoints -=1;
                            }
                        }
                       
                        printMaze();
                        continue;
                    }
                    //enemies
                    if ( maze[pRow - 1][pCol] == 'E')
                    {
                        cout << "Enemy found.\n";
                        if ( battle() )
                        {
                            cout << "Player won.\n";
                            maze[pRow][pCol] = '.';
                            pRow--;
                            maze[pRow][pCol] = 'L';
                            highScore += 10;
                        }
                        else
                        {
                            cout << "Player Lost.\n";
                            highScore -= 10;
                            healthpoints -=1;
                        }
                        printMaze();
                        continue;
                    }
                    //for keys
                    if ( maze[pRow - 1][pCol] == 'K')
                    {
                        cout << "Key found.\nAdding to Key Inventory.\n";
                        addKeyCount();
                        maze[pRow][pCol] = '.';
                        pRow--;
                        maze[pRow][pCol] = 'L';
                        highScore += 10;
                        printMaze();
                        continue;
                    }

                    maze[pRow][pCol] = '.';
                    pRow--;
                    maze[pRow][pCol] = 'L';
                    highScore += 10;
                    printMaze();
                    healthpoints -=1;

                    path << pRow << "," << pCol << "\n";
                }
                else if ( key == 'D' || key == 'd')
                {
                    if ( pRow >= size - 1)
                    {
                        cout << "->Can not move there.\n";
                        continue;
                    }
                    //obstacles
                    if ( maze[pRow + 1][pCol] == 'O')
                    {
                        cout << "Obstacle found.\nChoose Alternate path.\n";
                        highScore -= 5;
                        healthpoints -=1;
                        printMaze();
                        continue;
                    }
                    //locked doors
                    if ( maze[pRow + 1][pCol] == 'D')
                    {
                        cout << "Locked Door found.\n";
                        if ( q.top()->points >= 1)
                        {
                            cout <<"Using key to open the door.\n";
                            openLockedDoors();
                            maze[pRow][pCol] = '.';
                            pRow++;
                            maze[pRow][pCol] = 'L';
                            highScore += 10;
                        }
                        else
                        {
                            cout << "Since you have no keys, a guessing puzzle will be given to you.\nIf you guess the word correctly,you will proceed.\n";
                            if ( p1.solvePuzzle() == 1 )
                            {
                                maze[pRow][pCol] = '.';
                                pRow++;
                                maze[pRow][pCol] = 'L';
                                highScore += 10;   
                            }
                            else
                            {
                                highScore -= 10;
                                healthpoints -=1;
                            }
                        }
                        printMaze();
                        continue;
                    }   
                    //enemies
                    if ( maze[pRow + 1][pCol] == 'E')
                    {
                        cout << "Enemy found.\n";
                        if ( battle() )
                        {
                            cout << "Player won.\n";
                            maze[pRow][pCol] = '.';
                            pRow++;
                            maze[pRow][pCol] = 'L';
                            highScore += 10;
                        }
                        else
                        {
                            cout <<"Player Lost.\n";
                            highScore -= 10;
                            healthpoints -=1;
                        }
                        printMaze();
                        continue;
                    }
                    if ( maze[pRow + 1][pCol] == 'K')
                    {
                        cout << "Key has been found.\nAdding to Key Inventory.\n";
                        addKeyCount();
                        maze[pRow][pCol] = '.';
                        pRow++;
                        maze[pRow][pCol] = 'L';
                        highScore += 10;
                        printMaze();
                        continue;
                    }

                    maze[pRow][pCol] = '.';
                    pRow++;
                    maze[pRow][pCol] = 'L';
                    highScore += 10;
                    printMaze();
                    healthpoints -=1;
                    path << pRow << "," << pCol << "\n";
                }
                else if ( key == 'L' || key == 'l')
                {
                    if ( pCol <= 0 )
                    {
                        cout << "->Can not move there.\n";
                        continue;
                    }
                    //obstacle
                    if ( maze[pRow][pCol - 1] == 'O')
                    {
                        cout << "Obstacle found.\nChoose Alternate path.\n";
                        highScore -= 5;
                        healthpoints -=1;
                        printMaze();
                        continue;
                    }
                    //locked door
                    if ( maze[pRow][pCol - 1] == 'D')
                    {
                        cout << "Locked Door found.\n";
                        if (q.top()->points >=1)
                        {
                            cout << "Using key to open the door.\n";
                            openLockedDoors();
                            maze[pRow][pCol] = '.';
                            pCol--;
                            maze[pRow][pCol] = 'L';
                            highScore += 10;
                        }
                        else
                        {
                            cout << "Since you have no keys, a guessing puzzle will be given to you.\nIf you guess the word correctly,you will proceed.\n";
                            if ( p1.solvePuzzle() == 1 )
                            {
                                maze[pRow][pCol] = '.';
                                pCol--;
                                maze[pRow][pCol] = 'L';
                                highScore += 10;
                            }
                            else
                            {
                                highScore -= 10;
                                healthpoints -=1;
                            }
                            
                        }
                        printMaze();
                        continue;
                    }
                    //enemies
                    if ( maze[pRow][pCol - 1] == 'E')
                    {
                        cout << "Enemy found.\n";
                        if ( battle() )
                        {
                            cout << "Player won.\n";
                            maze[pRow][pCol] = '.';
                            pCol--;
                            maze[pRow][pCol] = 'L';
                            highScore += 10;
                        }
                        else
                        {
                            cout << "Player lost.\n";
                            highScore -= 10;
                            healthpoints -=1;
                        }
                        printMaze();
                        continue;
                    }
                    //for keys
                     if ( maze[pRow][pCol - 1] == 'K')
                    {
                        cout << "Key Found.\nAdding to Key Inventory.\n";
                        addKeyCount();
                        maze[pRow][pCol] = '.';
                        pCol--;
                        maze[pRow][pCol] = 'L';
                        highScore += 10;
                        printMaze();
                        continue;
                    }
                    maze[pRow][pCol] = '.';
                    pCol--;
                    maze[pRow][pCol] = 'L';
                    highScore += 10;
                    printMaze();
                    healthpoints -=1;
                    path << pRow << "," << pCol << "\n";
                }
                else if ( key == 'R' || key == 'r')
                {
                    if ( pCol >= size - 1 )
                    {
                        cout << "->Can not move there.\n";
                        continue;
                    }
                    //obstacle
                    if ( maze[pRow][pCol + 1] == 'O')
                    {
                        cout << "Obstacle found.\nChoose Alternate path.\n";
                        highScore -= 5;
                        printMaze();
                        continue;
                    }
                    //locked door
                    if ( maze[pRow][pCol + 1] == 'D')
                    {
                        cout << "Locked Door found.\n";
                        if (q.top()->points >= 1)
                        {
                            cout << "Using key to open locked door.\n";
                            openLockedDoors();
                            maze[pRow][pCol] = '.';
                            pCol++;
                            maze[pRow][pCol] = 'L';
                            highScore += 10;
                        }
                        else 
                        {
                            cout << "Since you have no keys, a guessing puzzle will be given to you.\nIf you guess the word correctly,you will proceed.\n";
                            if ( p1.solvePuzzle() == 1 )
                            {
                                maze[pRow][pCol] = '.';
                                pCol++;
                                maze[pRow][pCol] = 'L';
                                highScore += 10;
                            }
                            else
                            {
                                highScore -= 10;
                                healthpoints -=1;
                            }
                        }
                        printMaze();
                        continue;
                    }
                    //enemy
                    if ( maze[pRow][pCol + 1] == 'E')
                    {
                        cout << "Enemy found.\n";
                        if ( battle() )
                        {
                            cout << "Player won.\n";
                            maze[pRow][pCol] = '.';
                            pCol++;
                            maze[pRow][pCol] = 'L';
                            highScore += 10;
                        }
                        else
                        {
                            highScore -= 10;
                            healthpoints -=1;
                            cout << "Player lost.\n";
                        }
                        printMaze();
                        continue;
                    }
                    //for keys
                    if ( maze[pRow][pCol + 1] == 'K')
                    {
                        cout << "Key found.\nAdding to Key Inventory.\n";
                        addKeyCount();
                        maze[pRow][pCol] = '.';
                        pCol++;
                        maze[pRow][pCol] = 'L';
                        printMaze();
                        continue;
                    }
                    maze[pRow][pCol] = '.';
                    pCol++;
                    maze[pRow][pCol] = 'L';
                    printMaze();
                    highScore += 10;
                    healthpoints -=1;
                    path << pRow << "," << pCol << "\n";
                }
                
                else
                {
                    cout << "\nWRONG INPUT!!!.\n";
                } 
            }
            if ( pRow == size - 1 || pCol == size -1 )
            {
                cout << "\nPlayer found the treasure.\n";
            }
            cout << "\n--------------------------------------\n";


            ofstream myFile;
            myFile.open("HighScores.txt" , ios :: app);
            myFile << name << "\n" << highScore << endl;
            path << "-----------------------\n";
            myFile.close();
            path.close();
        }
        ~MazeMovement()
        {
            for ( int i = 0 ; i < size ; i++ )
            {
                delete[]maze[i];
            }
            delete[]maze;
            maze = nullptr;
        }

};

class AVLTree {
private:

    void searchInLeaderBoard(AVLNode* root , string n)
    {
        if ( root == nullptr )
        {
            return;
        }
        if ( root->name == n )
        {
            cout << "Player Name: " << root->name <<" \t Score: " << root->key << "\n";
        }
        if ( name < root->name )
        {
            searchInLeaderBoard(root->left , n);
        }
        else
        {
            searchInLeaderBoard(root->right , n);
        }
    }

    void PostOrderTraversal(AVLNode *node) 
    {
        if (node == nullptr)
            return;

        PostOrderTraversal(node->right);
        cout << "Player Name:" << node->name <<  "\t,Player Score:" << node->key << "\n";
        PostOrderTraversal(node->left);
    }
    
    void scores(AVLNode* node)
    {
        if (node == nullptr)
            return;

        PostOrderTraversal(node->right);
        cout << ++i << "Player Score:" << node->key << "\n";
        PostOrderTraversal(node->left);
    }
    AVLNode *root;
public:
    
    int i;

    AVLTree()
    {
        root = nullptr;
        i = 0;
    }
    int getHeight(AVLNode *node)
    {
        if (node == nullptr)
            return 0;
        return node->height;
    }
    int getBalance(AVLNode *node) 
    {
        if (node == nullptr)
            return 0;
        return  getHeight(node->left) - getHeight(node->right) ;
    }
    AVLNode* rotateRight(AVLNode *node) 
    {
        AVLNode *newRoot = node->left;
        node->left = newRoot->right;
        newRoot->right = node;

        node->height =  1 + max(getHeight(node->left), getHeight(node->right));
        newRoot->height = 1 + max(getHeight(newRoot->left), getHeight(newRoot->right)) ;

        return newRoot;
    }
    AVLNode* rotateLeft(AVLNode *node) 
    {
        AVLNode *newRoot = node->right;
        node->right = newRoot->left;
        newRoot->left = node;

        node->height = 1 + max(getHeight(node->left), getHeight(node->right)) ;
        newRoot->height = 1 + max(getHeight(newRoot->left), getHeight(newRoot->right));

        return newRoot;
    }
    AVLNode* insertNode(AVLNode *node, int key , string n)
    {
        if (node == nullptr)
            return new AVLNode(key , n);

        if (key < node->key)
        {
            node->left = insertNode(node->left, key , n);
        }
        else if (key > node->key)
        {
            node->right = insertNode(node->right, key , n);
        }
        else
        {
            return node;
        }

        node->height = 1 +  max(getHeight(node->left), getHeight(node->right));

        int balance = getBalance(node);

        if (balance > 1 && key < node->left->key)
        {
            return rotateRight(node);
        }

        if (balance < -1 && key > node->right->key)
        {
            return rotateLeft(node);
        }

        if (balance > 1 && key > node->left->key) 
        {
            node->left = rotateLeft(node->left);
            return rotateRight(node);
        }

        if (balance < -1 && key < node->right->key) 
        {
            node->right = rotateRight(node->right);
            return rotateLeft(node);
        }
        return node;
    }
    
   
    void insert() 
    {
        int key;
        string name;

        ifstream fin("HighScores.txt");
        while (!fin.eof())
        {
            fin >> name;
            fin >> key;
            root = insertNode(root, key , name);
        }
    }
    void ScoreBoard()
    {
        insert();
        cout << "Score Board is: \n";
        PostOrderTraversal(root);
    }
    void displayScores()
    {
        insert();
        scores(root);
    }
    
};

class Shortestpath
{
    private:
        void print(AVLNode *node)
        {
            if ( node == nullptr )
            {
                return;
            }
            print(node->right);
            cout << "Row = " << node->key << ", Col = " << node->key2 << "\n";
            print(node->left);
        }
    public:
        AVLNode *root;
        Shortestpath()
        {
            root = nullptr;
        }
        int getHeight(AVLNode *node)
        {
            if (node == nullptr)
            {
                return 0;
            }
            return node->height;
        }
        int getBal( AVLNode *node)
        {
            if (node == nullptr)
            {
                return 0;
            }
            return getHeight(node->left) - getHeight(node->right);
        }
        AVLNode* rotateRight(AVLNode *node) 
        {
            AVLNode *newRoot = node->left;
            node->left = newRoot->right;
            newRoot->right = node;

            node->height =  1 + max(getHeight(node->left), getHeight(node->right));
            newRoot->height = 1 + max(getHeight(newRoot->left), getHeight(newRoot->right)) ;

            return newRoot;
        }
        AVLNode* rotateLeft(AVLNode *node) 
        {
            AVLNode *newRoot = node->right;
            node->right = newRoot->left;
            newRoot->left = node;

            node->height = 1 + max(getHeight(node->left), getHeight(node->right)) ;
            newRoot->height = 1 + max(getHeight(newRoot->left), getHeight(newRoot->right));

            return newRoot;
        }
        AVLNode* insertNode1(AVLNode *node, int key , int n)
        {
        if (node == nullptr)
            return new AVLNode(key , n);

        if (key < node->key)
        {
            node->left = insertNode1(node->left, key , n);
        }
        else if (key > node->key)
        {
            node->right = insertNode1(node->right, key , n);
        }
        else
        {
            return node;
        }

        node->height = 1 +  max(getHeight(node->left), getHeight(node->right));

        int balance = getBal(node);

        if (balance > 1 && key < node->left->key)
        {
            return rotateRight(node);
        }

        if (balance < -1 && key > node->right->key)
        {
            return rotateLeft(node);
        }

        if (balance > 1 && key > node->left->key) 
        {
            node->left = rotateLeft(node->left);
            return rotateRight(node);
        }

        if (balance < -1 && key < node->right->key) 
        {
            node->right = rotateRight(node->right);
            return rotateLeft(node);
        }
        return node;
    }
    void insert()
    {
        int row = 0;
        int col = 0;
        char comma;
        ifstream fin("path.txt");
        while ( fin >> row >> comma >> col )
        {
            root = insertNode1(root,row,col);
            cout << row << "," << col << endl;
        }
        
    }
};

int main()
{
    char k;
    int choice = 0;
    string naam = "\0";
    MazeMovement m1;
    AVLTree tree , tree2;
    Shortestpath s;
    do
    {
        cout << "WELCOME TO MAZE GAME.\n";
        cout << "PRESS (1) TO PLAY.\n";
        cout << "PRESS (2) TO DISPLAY LEADERBOARD.\n";
        cout << "PRESS (3) TO DISPLAY THE SHORTEST DISTANCE TO TREASURE OF THE LATEST PLAYER.\n";
        cout << "PRESS (0) TO EXIT PROGRAM.\n";
        cout << "ENTER YOUR CHOICE: ";
        cin >> choice;
        cout << endl;

        switch(choice)
        {
            case 1:
            {
                m1.movement(k);
                cout << "\n";
                break;
            }
            case 2:
            {
                tree.ScoreBoard();
                cout << "\n";
                break;
            }
            case 3:
            {
                s.insert();
                cout << "\n";
                break;
            }
            default:
            {
                cout << "Wrong Option Selected.\n";
                break;
            }
        }
    } while (choice != 0);
    cout << "\n\nMAZE GAME ENDED.\n\n";
    return 0;
}

