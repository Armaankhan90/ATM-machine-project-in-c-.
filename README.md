

// project ATM MACHINE in c++ //


#include<iostream>
#include<vector>
#include<string>

using namespace std;
 
// Sample Account class
class Account {
private:
    string accountNumber;
    string pin;
    double balance;

public:
    Account(string accNumber, string pin, double balance) : accountNumber(accNumber), pin(pin), balance(balance) {}

    string getAccountNumber() const {
        return accountNumber;
    }
 
    string getPin() const {
        return pin;
    }

    double getBalance() const {
        return balance;
    }

    void deposit(double amount) {
        balance += amount;
    }

    bool withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }
};

// Sample ATM class
class ATM {
private:
    vector<Account> accounts;
    Account* currentAccount;

public:
    ATM() {
        // Sample accounts
        accounts.push_back(Account("123456", "1234", 1000.0));
        accounts.push_back(Account("789012", "5678", 500.0));

        currentAccount = nullptr;
    }

    bool authenticate(string accountNumber, string pin) {
        for (auto& acc : accounts) {
            if (acc.getAccountNumber() == accountNumber && acc.getPin() == pin) {
                currentAccount = &acc;
                return true;
            }
        }
        return false;
    }

    void displayMenu() {
        cout << "1. Check Balance" << endl;
        cout << "2. Deposit" << endl;
        cout << "3. Withdraw" << endl;
        cout << "4. Logout" << endl;
    }

    void run() {
        string accountNumber, pin;
        cout << "Enter Account Number: ";
        cin >> accountNumber;
        cout << "Enter PIN: ";
        cin >> pin;

        if (authenticate(accountNumber, pin)) {
            int choice;
            do {
                displayMenu();
                cout << "Enter your choice: ";
                cin >> choice;

                switch (choice) {
                    case 1:
                        cout << "Balance: $" << currentAccount->getBalance() << endl;
                        break;

                    case 2:
                        double depositAmount;
                        cout << "Enter deposit amount: $";
                        cin >> depositAmount;
                        currentAccount->deposit(depositAmount);
                        cout << "Deposit successful. New balance: $" << currentAccount->getBalance() << endl;
                        break;

                    case 3:
                        double withdrawAmount;
                        cout << "Enter withdraw amount: $";
                        cin >> withdrawAmount;
                        if (currentAccount->withdraw(withdrawAmount)) {
                            cout << "Withdrawal successful. New balance: $" << currentAccount->getBalance() << endl;
                        } else {
                            cout << "Insufficient funds or invalid amount." << endl;
                        }
                        break;

                    case 4:
                        cout << "Logging out..." << endl;
                        break;

                    default:
                        cout << "Invalid choice. Try again." << endl;
                }

            } while (choice != 4);
        } else {
            cout << "Authentication failed. Invalid account number or PIN." << endl;
        }
    }
};

int main() 
{
    ATM atm;
    atm.run();

    return 0;
}
