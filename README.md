#include <iostream>
#include <string>
#include <cstdlib>
#include <ctime>

using namespace std;

class Character {
private:
    string name;
    int health;
    int attack;
    int level;
    int experience;

public:
    Character(string n) : name(n), health(100), attack(10), level(1), experience(0) {}

    void displayStats() {
        cout << "Name: " << name << "\n";
        cout << "Level: " << level << "\n";
        cout << "Health: " << health << "\n";
        cout << "Attack: " << attack << "\n";
        cout << "Experience: " << experience << "\n\n";
    }

    bool isAlive() {
        return health > 0;
    }

    void takeDamage(int damage) {
        health -= damage;
    }

    void levelUp() {
        level++;
        attack += 5;
        health = 100;
        experience = 0;
        cout << "Congratulations! You leveled up to level " << level << "!\n";
        cout << "Your health is restored to full.\n";
    }

    void gainExperience(int exp) {
        experience += exp;
        if (experience >= 100) {
            levelUp();
        }
    }

    int getAttack() {
        return attack;
    }

    int getHealth() const {
        return health;
    }
};

class Monster {
private:
    string name;
    int health;
    int attack;

public:
    Monster(string n, int h, int a) : name(n), health(h), attack(a) {}

    bool isAlive() {
        return health > 0;
    }

    void takeDamage(int damage) {
        health -= damage;
    }

    int getAttack() {
        return attack;
    }

    void displayStats() {
        cout << "Name: " << name << "\n";
        cout << "Health: " << health << "\n";
        cout << "Attack: " << attack << "\n\n";
    }

    int getHealth() const {
        return health;
    }
};

void fight(Character& player, Monster& enemy, const string& enemyName) {
    cout << "A wild " << enemyName << " appears!\n\n";
    while (player.isAlive() && enemy.isAlive()) {
        cout << "Your health: " << player.getHealth() << "\n";
        cout << "Enemy health: " << enemy.getHealth() << "\n\n";
        
        // Player's turn
        cout << "Choose your action:\n";
        cout << "1. Attack\n";
        cout << "2. Run\n";
        int choice;
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "You attack the " << enemyName << "!\n";
                enemy.takeDamage(player.getAttack());
                break;
            case 2:
                cout << "You run away!\n";
                return;
            default:
                cout << "Invalid choice! Try again.\n";
                break;
        }

        // Enemy's turn
        if (enemy.isAlive()) {
            cout << "The " << enemyName << " attacks you!\n";
            player.takeDamage(enemy.getAttack());
        }
    }

    if (player.isAlive()) {
        cout << "You defeated the " << enemyName << "!\n";
        player.gainExperience(50);
    } else {
        cout << "You were defeated by the " << enemyName << "...\n";
    }
}

int main() {
    srand(time(0)); // Seed random number generator

    cout << "Welcome to Adventure Quest!\n";
    cout << "What's your name, adventurer? ";
    string playerName;
    cin >> playerName;

    Character player(playerName);

    while (player.isAlive()) {
        cout << "\nYou are in a forest. What would you like to do?\n";
        cout << "1. Explore\n";
        cout << "2. Quit\n";
        int choice;
        cin >> choice;

        switch (choice) {
            case 1: {
                int random = rand() % 3; // Random number between 0 and 2
                if (random == 0) {
                    Monster enemy("Goblin", 30, 5);
                    fight(player, enemy, "Goblin");
                } else if (random == 1) {
                    Monster enemy("Orc", 50, 10);
                    fight(player, enemy, "Orc");
                } else {
                    cout << "You didn't encounter any enemies while exploring.\n";
                }
                break;
            }
            case 2:
                cout << "Thanks for playing Adventure Quest!\n";
                return 0;
            default:
                cout << "Invalid choice! Try again.\n";
                break;
        }
    }

    cout << "Game over! Your adventure has come to an end.\n";
    return 0;
}
