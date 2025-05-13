# Login-and-Registration-System-
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

class UserSystem {
public:
    void registerUser() {
        string username, password;
        cout << "Enter a username: ";
        cin >> username;
        cout << "Enter a password: ";
        cin >> password;

        ifstream check("users.txt");
        string line, existingUser;
        while (getline(check, line)) {
            existingUser = line.substr(0, line.find(' '));
            if (existingUser == username) {
                cout << "Username already exists. Try another one.\n";
                return;
            }
        }
        check.close();

        ofstream file("users.txt", ios::app);
        file << username << " " << password << endl;
        file.close();

        // Create user-specific file
        ofstream userFile(username + ".txt");
        userFile << "User File for: " << username << endl;
        userFile.close();

        cout << "Registration successful. User file created.\n";
    }

    bool loginUser() {
        string username, password;
        cout << "Enter your username: ";
        cin >> username;
        cout << "Enter your password: ";
        cin >> password;

        ifstream file("users.txt");
        string line, storedUser, storedPass;
        bool found = false;

        while (file >> storedUser >> storedPass) {
            if (storedUser == username && storedPass == password) {
                found = true;
                break;
            }
        }
        file.close();

        if (found) {
            cout << "Login successful! Welcome, " << username << "!\n";
            showUserFile(username);
            return true;
        } else {
            cout << "Invalid username or password.\n";
            return false;
        }
    }

    void showUserFile(const string& username) {
        ifstream file(username + ".txt");
        if (!file) {
            cout << "User file not found.\n";
            return;
        }

        string line;
        cout << "\n--- " << username << "'s File Content ---\n";
        while (getline(file, line)) {
            cout << line << endl;
        }
        file.close();
    }
};

// ===================== MAIN MENU =====================
int main() {
    UserSystem system;
    int choice;

    do {
        cout << "\n===== LOGIN & REGISTRATION MENU =====\n";
        cout << "1. Register\n";
        cout << "2. Login\n";
        cout << "0. Exit\n";
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
        case 1:
            system.registerUser();
            break;
        case 2:
            system.loginUser();
            break;
        case 0:
            cout << "Exiting...\n";
            break;
        default:
            cout << "Invalid choice.\n";
        }
    } while (choice != 0);

    return 0;
}
