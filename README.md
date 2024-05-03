#ifndef Validation_h
#define Validation_h
#include <iostream>
#include "Account.h"
using namespace std;

class Validation: public Account
{
public:

//  Character Validation
    bool Character_input(string str)
    {
            for (int i = 0; i < str.length(); i++)
                if (isalpha(str[i]) == false && str[i] != ' ')
                {
                    cout << "You cannot enter a number in Charater field" << endl
                    << "Please try again" << endl << "--> ";
                    return false;
                }
         return true;
     }
    
//#################################################
//       Numeric Validation
         bool Numeric_input(string str)
         {
                for (int i = 0; i < str.length(); i++)
                    if (isdigit(str[i]) == false)
                    {
                        cout << "You cannot enter a Charater in numeric field" << endl
                        << "Please try again" << endl << "--> ";
                        return false;
                     }
             return true;
         }
//#################################################
//       Mobile Validation
         bool mobile_number(string str)
        {
            while (str.length() != 11)
            {
                cout << "The mobile number should equal 11 digit" << endl;
                cout << "Please try again" << endl << "--> ";
                return false;
            }
             return true;
         }
//#################################################
//  Account Number Validation
        bool Account_Num(string str)
       {
           while (str.length() != 8)
           {
               cout << "The Account number should equal 8 digit" << endl;
               cout << "Please try again" << endl << "--> ";
               return false;
           }
            return true;
        }
};
#endif

#include <iostream>
#include "Account.h"
using namespace std;
#ifndef Search_h
#define Search_h


class Search: public Account
{
public:
    void get_Info(Account [], int z);
};


#endif /* Search_h */
#ifndef Menu_h
#define Menu_h
#include <iostream>
#include "Validation.h"
#include "Account.h"

class Menu : public Account
{
public:
    
    void Display_welcome_screen();
    void main_menu();
    void loanType();
};

#endif
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <string.h>
#include "Account.h"
#include "Menu.h"
#include "Validation.h"
#include "Search.h"
#define PROMPT "class> "
using namespace std;


int main()
{
    int size(1);
    
    Account *Customer = new Account [size];
    Menu Options;
    Validation Show;
    Search About;
    
    string choice;
    string Customer_Name;
    string MobNum;
    string AccNum;
    string Bal;
    string AccType;
    string pass;
    string Trans1, Trans2;
    string amount;
    string check("N");
    string months;
    
    int H = 0;
    
    int T1 = 0, T2 = 0;
    
    Options.Display_welcome_screen();
    while (true)
    {
        Options.main_menu();
        cout << "Enter your choice: ";
        getline(cin >> ws, choice);
        
        while (!Show.Numeric_input(choice) || (choice.length() >= 2 && choice != "10"))
        {
            cout << "Please choose a number from 1->10" << endl;
            cout << "Enter your choice: ";
            getline(cin >> ws, choice);
        }
        if(choice == "1")
        {
            system("CLS");
            for(int i=size-1; i<size; i++)
            {
            //Customer Name Field
                cout << "Enter Your Name: ";
                getline(cin >> ws, Customer_Name);
                while (!Show.Character_input(Customer_Name)) {getline(cin >> ws, Customer_Name);}
                
            //##################################################################################################
            //Mobile Number Field
                cout << "Enter Your Mobile Number in 11 digit: ";
                getline(cin >> ws, MobNum);
                while (true)
                {
                    if (!Show.Numeric_input(MobNum)) {getline(cin >> ws, MobNum);}
                    else if (!Show.mobile_number(MobNum)) {getline(cin >> ws, MobNum);}
                    else {break;}
                }
                
            //##################################################################################################
            //Account Number Field
                cout << "Enter your Account Number in 8 digit" << endl << "--> ";
                getline(cin >> ws, AccNum);
                while (true) {
                    if (!Show.Numeric_input(AccNum)) {getline(cin >> ws, AccNum);}
                    else if (!Show.Account_Num(AccNum)) {getline(cin >> ws, AccNum);}
                    else{break;}
                }
               
            //##################################################################################################
            //Password Field
                cout << "Enter your Password (should equal 4 digit): ";
                getline(cin >> ws, pass);
                while (!Show.Numeric_input(pass)) {getline(cin >> ws, pass);}
                
                while (pass.length() != 4)
                {
                    cout << "Password should equal 4 digit: ";
                    getline(cin >> ws, pass);
                }
            //##################################################################################################
            //Balance Field
                cout << "What is your balance: ";
                getline(cin >> ws, Bal);
                while (!Show.Numeric_input(Bal)) { getline(cin >> ws, Bal);}
                double Balance = stod(Bal);
            //##################################################################################################
            //Confirmation Message
                cout << endl;
                cout << "#############################" << endl;
                cout << "#Account opened successfully#" << endl;
                cout << "#############################" << endl;
                cout << endl;
                
                Customer[i].set_Name(Customer_Name);
                Customer[i].set_MobNum(MobNum);
                Customer[i].set_AccNum(AccNum);
                Customer[i].set_Pass(pass);
                Customer[i].set_Balance(Balance);
            }
            size++;
            continue;
        }
        //##################################################################################################
        // Deposite Part
        if (choice == "2")
        {
            if (size == 1) {cout << "No account yet!" << endl; continue;}
            //          Validation Input
            cout << "Enter your account number: ";
            getline(cin >> ws, AccNum);
            while (!Show.Numeric_input(AccNum)) {getline(cin >> ws, AccNum);}
            cout << "Enter the password: ";
            getline(cin >> ws, pass);
            //##################################################################################################
            for (int i=0; i < size-1; i++)
            {
                if (Customer[i].get_Acc() == AccNum && Customer[i].get_pass() == pass)
                {
                    cout << "Enter the deposit amount: ";
                    getline(cin >> ws, amount);
                    while (!Show.Numeric_input(amount)) { getline(cin >> ws, amount);}
                    double Amount = stod(amount);
                    Customer[i].Deposite(Amount);
                    Customer[i].set_Transaction(Amount, "Deposite");
                    break;
                }
                else
                {
                    cout << "Either the password or account number is wrong";
                }
            }
        }
        //##################################################################################################
        // Withdraw Part
        if (choice == "3")
        {
            if (size == 1) {cout << "No account yet!" << endl; continue;}
            //          Validation Input
            cout << "Enter your account number: ";
            getline(cin >> ws, AccNum);
            while (!Show.Numeric_input(AccNum)) {getline(cin >> ws, AccNum);}
            cout << "Enter the password: ";
            getline(cin >> ws, pass);
        //##################################################################################################
            for (int i=0; i < size-1; i++)
            {
                if (Customer[i].get_Acc() == AccNum && Customer[i].get_pass() == pass)
                {
                    cout << "Enter the withdrawal amount: ";
                    getline(cin >> ws, amount);
                    while (!Show.Numeric_input(amount)) { getline(cin >> ws, amount);}
                    double Amount = stod(amount);
                    Customer[i].Withdraw(Amount);
                    Customer[i].set_Transaction(Amount, "Withdraw");
                }
                else {cout << "Either the password or account number is wrong";}
            }
        }
        
        //##################################################################################################
        // Display All Account Information Part
        if (choice == "4")
        {
            if (size == 1) {cout << "No account yet!" << endl; continue;}
        //  Validation input
            cout << "Enter your account number: ";
            getline(cin >> ws, AccNum);
            while (!Show.Numeric_input(AccNum)) {getline(cin >> ws, AccNum);}
            cout << "Enter the password: ";
            getline(cin >> ws, pass);
            
            for (int i=0; i<size-1; i++)
            {
                if (Customer[i].get_Acc() == AccNum && Customer[i].get_pass() == pass)
                {
                    Customer[i].Display_All_Account();
                    break;
                }
            }
        }
        //##################################################################################################
        // Transaction part
        if (choice == "5")
        {
            if (size == 1) {cout << "No account yet!" << endl; continue;}
            //          Validation Input
            cout << "Enter your account number: ";
            getline(cin >> ws, AccNum);
            while (!Show.Numeric_input(AccNum)) {getline(cin >> ws, AccNum);}
            cout << "Enter the password: ";
            getline(cin >> ws, pass);
            
            
            for (int i=0; i<size-1; i++)
            {
                if (Customer[i].get_Acc() == AccNum && Customer[i].get_pass() == pass)
                {
                    Customer[i].Display_transaction_details();
                }
                else {cout << "Either the password or account number is wrong";}
            }
        }
        //##################################################################################################
        // Delete Account Part
        if (choice == "6")
        {
            cout << "Are you sure you want delete the account and end the program Y/N: ";
            cin >> check;
            while (!Show.Character_input(check)) {cin >> check;}
            if (check == "Y" || check == "y")
            {
                delete [] Customer;
                check = "y";
            }
            if(check == "y")
                choice = "10";
            else
            {
                continue;
            }
        }
        //##################################################################################################
        // Loan Part
        if (choice == "7")
        {
            if (size == 1) {cout << "No account yet!" << endl; continue;}
            //          Validation Input
            cout << "Enter your account number: ";
            getline(cin >> ws, AccNum);
            while (!Show.Numeric_input(AccNum)) {getline(cin >> ws, AccNum);}
            cout << "Enter the password: ";
            getline(cin >> ws, pass);
            
            for (int i=0; i<size-1; i++)
            {
                if (Customer[i].get_Acc() == AccNum && Customer[i].get_pass() == pass)
                {
                    cout << "Enter Loan amount: ";
                    getline(cin >> ws, amount);
                    while (!Show.Numeric_input(amount)) { getline(cin >> ws, amount);}
                    double Amount = stod(amount);
                    cout << "Enter the number of months you will pay in installments: ";
                    getline(cin >> ws, months);
                    while (!Show.Numeric_input(months)) { getline(cin >> ws, months);}
                    int Months = stoi(months);
                    Customer[i].set_Loan(Amount, Months);
                    Customer[i].set_loanType();
                    H = i;
                    break;
                }
            }
            Customer[H].Loan_Details();
            continue;
        }
    //##################################################################################################
    // Search Part
    if (choice == "8")
    {
        if (size == 1) {cout << "No account yet!" << endl; continue;}
        //          Validation Input
        cout << "Enter your account number: ";
        getline(cin >> ws, AccNum);
        while (!Show.Numeric_input(AccNum)) {getline(cin >> ws, AccNum);}
        cout << "Enter the password: ";
        getline(cin >> ws, pass);
        
        for (int i=0; i < size-1; i++)
        {
            if (Customer[i].get_Acc() == AccNum && Customer[i].get_pass() == pass)
            {
                About.get_Info(Customer, i);
                check = "y";
                break;
            }
        }
        if (check == "N") 
        {
            cout
            << "  #############################################" << endl
            << " # There is no matching account you entered #" << endl
            << "##########################################" << endl;
        }
        else {check = "N";}
        continue;
    }
        
    //##################################################################################################
                if (choice == "9")
                {
                    if (size == 1) {cout << "No account yet!" << endl; continue;}
                    //          Validation Input
                    cout << "Enter your account number: ";
                    getline(cin >> ws, Trans1);
                    while (!Show.Numeric_input(Trans1)) {getline(cin >> ws, Trans1);}
                    cout << "Enter The Transfer account number: ";
                    getline(cin >> ws, Trans2);
                    while (!Show.Numeric_input(Trans2)) {getline(cin >> ws, Trans2);}
                    cout << "Enter the conversion value: ";
                    getline(cin >> ws, amount);
                    while (!Show.Numeric_input(amount)) { getline(cin >> ws, amount);}
                    double Amount = stod(amount);
                    
                    for (int i=0; i < size-1; i++) {if (Customer[i].get_Acc() == Trans1) {T1 = i;break;}}
                    for (int j=0; j < size-1; j++) {if (Customer[j].get_Acc() == Trans2) {T2 =j; break;}}
                    
                    Customer[T1].set_Transfer(Customer[T2], Amount);
                    cout 
                    << "# ########" << endl
                    << "# Done #" << endl
                    << "########" << endl;
                    continue;
                }
    //##################################################################################################
    // Search Part
        if (choice == "8")
        {
            if (size == 1) {cout << "No account yet!" << endl; continue;}
            //          Validation Input
            cout << "Enter your account number: ";
            getline(cin >> ws, AccNum);
            while (!Show.Numeric_input(AccNum)) {getline(cin >> ws, AccNum);}
            cout << "Enter the password: ";
            getline(cin >> ws, pass);
            
            for (int i=0; i < size-1; i++)
            {
                if (Customer[i].get_Acc() == AccNum && Customer[i].get_pass() == pass)
                {
                    
                }
            }
            
        }
    //##################################################################################################
    // Quit Part
        if (choice == "10")
        {
            cout << endl;
            cout << "  ###################################################" << endl;
            cout << " # Thank you for using Terminal Banking Management #" << endl;
            cout << "###################################################" << endl;
            cout << endl;
            if (check == "N")
                delete [] Customer;
            break;
        }
    }
}
#include <iostream>
#include <string>

#ifndef ACCOUNT_H
#define ACCOUNT_H
using namespace std;



class Account
{
private:
    string customer_name;
    string Mobile_Number;
    string account_type;
    string Account_Number;
    string Transaction;
    string Password;
    double Account_Balance;
    
    int Transaction_ID;
    string Transaction_Type;
    string date;
    double Amount;
    
    int Loan_ID;
    int Months;
    double Loan_Amount;
    const double  Interest_Rate = 0.05;
    double Installment_Amount;
    string Loan_Type;
    
    
public:
    Account();
    
    void set_Name(string Name);
    void set_MobNum(string MobNum);
    void set_AccNum(string AccNum);
    void set_Pass(string Pass);
    void set_Balance(double Balance);
    void set_Transaction(double amount, string transaction_type);
    void set_Loan(double loan_amount, int months);
    void set_loanType();
    void Loan_Details();
    string get_C();
    string get_M();
    string get_T();
    string get_Acc();
    string get_pass();
    double get_B();
   
    
    void Deposite(int amount);
    void Withdraw(int amout);
    
    void Display_All_Account();
    void Display_transaction_details();
    
    void set_Transfer(Account &p, int amount);
    
};
#endif

#include <iostream>
#include <ctime>
#include "Account.h"
#include "Menu.h"
#include "Validation.h"
#include "Search.h"
#define PROMPT "class> "
using namespace std;



Account:: Account()
{
    customer_name = "";
    Account_Number = "";
    Password = "";
    Mobile_Number = "";
    Account_Balance = 0;
    
    Transaction_Type = "";
    date = "";
    Transaction_ID = 0;
    Amount = 0;
    
    
    Loan_ID = 0;
    Months = 0;
    Loan_Amount = 0;
    Installment_Amount = 0;
    Loan_Type = "";
}



void Account:: set_Name(string Name) {customer_name = Name;}
void Account:: set_MobNum(string MobNum){Mobile_Number = MobNum;}
void Account:: set_AccNum(string AccNum){Account_Number = AccNum;}
void Account:: set_Pass(string Pass){Password = Pass;}
void Account:: set_Balance(double Balance){Account_Balance = Balance;}


string Account:: get_C(){return customer_name;}
string Account:: get_M(){return Mobile_Number;}
string Account:: get_T(){return Transaction;}
string Account:: get_Acc(){return Account_Number;}
string Account:: get_pass(){return Password;}
double Account:: get_B(){return Account_Balance;}


void Account::Deposite(int amount)
{
    Account_Balance += amount;
    cout 
    << "########" << endl
    << "# Done #" << endl
    << "########" << endl;
}
void Account::Withdraw(int amount)
{
    if (amount > Account_Balance) {cout << "Sorry, the balance is not enough" << endl;}
    
    else 
    {
        Account_Balance -= amount;
        cout
        << "########" << endl
        << "# Done #" << endl
        << "########" << endl;
    }
}

void Account:: set_Transaction(double amount, string transaction_type)
{
    time_t now = time(0);
    tm *gmtm = gmtime(&now);
    char *dt = asctime(gmtm);
    
    Transaction_ID = rand();
    Amount = amount;
    Transaction_Type = transaction_type;
    date = dt;
}

void Account:: set_Loan(double loan_amount, int months)
{
    Loan_ID = rand();
    Loan_Amount = loan_amount;
    Months = months;
    Installment_Amount = ( (loan_amount + (loan_amount * Interest_Rate)) / months);
    
}
void Account:: set_loanType()
{
    string C;
    Validation show;
    string Choice [3] = {"Personal loans", "Home loan", "Vehicle loan"};
    cout
    << "1. Personal loans" << endl
    << "2. Home loan" << endl
    << "3. Vehicle loan" << endl;
    
    cout << "Choose the number of Loan Type: ";
    getline(cin >> ws, C);
    int choice = stoi(C);
    while (!show.Numeric_input(C)) {getline(cin >> ws, C);}
    Loan_Type = Choice[choice -1];
}

void Account:: Loan_Details()
{
    cout << "##########################################" << endl;
    cout
    << "\nLoan ID: " << Loan_ID << endl
    << "Account Number: " << Account_Number << endl
    << "Loan Type: " << Loan_Type << endl
    << "Installment Amount: " << Installment_Amount << " Per Month" << endl;
    cout << "##########################################" << endl;
}

void Account:: Display_transaction_details()
{
    cout << "##########################################" << endl;
    cout
    << "\nTransaction Type: "  << Transaction_Type << endl
    << "Transaction ID: " << Transaction_ID << endl
    << "Amount: " << Amount << endl
    << "Date: " << date << endl;
    cout << "##########################################" << endl;
}

void Account:: set_Transfer(Account &p, int amount)
{
    time_t now = time(0);
    tm *gmtm = gmtime(&now);
    char *dt = asctime(gmtm);
    
    Account_Balance -= amount;
    p.Account_Balance += amount;
    Transaction = dt;
    p.Transaction = dt;
}

void Account:: Display_All_Account()
{
    cout << "##########################################" << endl;
    cout
    << "\nCustomer name: " << customer_name << endl
    << "Mobile Number: " << Mobile_Number << endl
    << "Balance: " << Account_Balance << endl;
    cout << "##########################################" << endl;
}

void Menu:: Display_welcome_screen()
{
    cout << "##########################################" << endl;
    cout << "# Welcome To Terminal Banking Management #" << endl;
    cout << "##########################################" << endl;
}
void Menu:: main_menu()
{
    system("Color 2");
    cout
    << "1. Open Account" << endl
    << "2. Deposite Amount" <<endl
    << "3. Withdraw Amount" << endl
    << "4. Display All Account" << endl
    << "5. Display Transaction" << endl
    << "6. Delete Account" << endl
    << "7. Loan" << endl
    << "8. Search" << endl
    << "9. Transfer Amount" << endl
    << "10. Quit" << endl;
}


void Search:: get_Info(Account A[], int z)
{
    {
        cout << "Here is the account information" << endl;
        cout << "=========================================" << endl;
        cout << "Account Name: " << A[z].get_C() << endl;
        cout << "Account Mobile: " << A[z].get_M() << endl;
        cout << "Account Balance: " << A[z].get_B() << endl;
        cout << "=========================================" << endl;
    }
}
