# Grocery-billing-system-C++
#include <iostream>
#include <fstream>
#include <limits>
#include <cctype>
using namespace std;

class Bill 
{
public:
    int qua, tqua, price;
    int tprice[10], pri[10], quan[10];
    string name, nam[10];
    int total = 0;
    fstream f;

    void billCal() 
    {
        f.open("oops.txt", ios::app);
        f << "\n--------------------------------------------------------------" << endl;
        f << "\t\t*** List of Items ***\n";
        f << "--------------------------------------------------------------" << endl;
        f << "\tName\t\tPrice\t\tQty\t\tTotal Price\n";

        for (int i = 0; i < tqua; i++) {
            tprice[i] = pri[i] * quan[i];
        }
        for (int i = 0; i < tqua; i++) {
            total = total + tprice[i];
        }
        for (int i = 0; i < tqua; i++) {
            f << "\t" << nam[i] << "\t\t" << pri[i] << "\t\t" << quan[i] << "\t\t" << tprice[i] << "\n";
            f << "\n--------------------------------------------------------------" << endl;
            f << "\t\t\t\t\t\tTotal Bill:" << total << endl;
        }
    }

    void input() 
    {
        int i = 0;
        char ch;
        do 
        {
            cout << "\n\t*Details of " << i + 1 << " Item*" << endl;
            cout << "\nEnter the Name of Item (alphabets only): ";
            cin >> name;
            if (!isalpha(name[0]))
            {
                cout << "Invalid input! Please enter alphabets only for the item name." << endl;
                continue;
            }
            nam[i] = name;
            cout << "\nEnter the Total Quantity of Item (integer only): ";
            cin >> qua;
            if (cin.fail() || qua < 0) 
            {
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                cout << "Invalid input! Please enter a valid positive integer for quantity." << endl;
                continue;
            }
            quan[i] = qua;
            cout << "\nEnter the Price of Item (integer only): ";
            cin >> price;
            if (cin.fail() || price < 0) 
            {
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                cout << "Invalid input! Please enter a valid positive integer for price." << endl;
                continue;
            }
            pri[i] = price;
            cout << "Do you want to continue? (y/n): ";
            cin >> ch;
            i++;
        } 
        while (ch == 'y' || ch == 'Y');
        tqua = i;
    }
    void billDisplay() 
    {
        cout << "\n--------------------------------------------------------------" << endl;
        cout << "\t\t*** List of Items ***\n";
        cout << "--------------------------------------------------------------" << endl;
        cout << "\tName\t\tPrice\t\tQty\t\tTotal Price\n";
        for (int i = 0; i < tqua; i++) 
        {
            cout << "\t" << nam[i] << "\t\t" << pri[i] << "\t\t" << quan[i] << "\t\t" << tprice[i] << "\n";
        }
        cout << "\n--------------------------------------------------------------" << endl;
        cout << "\t\t\t\t\t\tTotal Bill:" << total << endl;
    }
    void payment() 
    {
        int cash, more, jast = 0;

        cout << "\n----------------------------------------------------" << endl;
        cout << "\t\t*** Payment ***\n";
        cout << "----------------------------------------------------" << endl;
        cout << "\nEnter the Total Amount To give to shopkeeper: ";
        cin >> cash;

        while (cash < total) 
        {
            cout << "\nThe Given Money is Not Enough ";
            more = total - cash;
            cout << more << " Rs more needed" << endl;
            cout << "\nEnter the Additional Amount To give to shopkeeper: ";
            int additional;
            cin >> additional;
            cash += additional;
        }

        while (cash != total) 
        {
            if (cash > total) 
            {
                jast = cash - total;
                cout << "The given money is more\t";
                cout << jast << " Rs take back" << endl;
                cout << "\nEnter the Return amount from shopkeeper:";
                int returnn;
                cin >> returnn;
                cash -= returnn;
            } 
            else 
            {
                cout << "The given money is not enough. ";
                cout << "More amount needed: " << total - cash << " Rs" << endl;
                cout << "\nEnter additional amount to make it exact: ";
                int additional;
                cin >> additional;
                cash += additional;
            }
        }

        cout << "\nPayment Successful..!!\n";
        cout << "\n\n\n\tThank You For Visiting....Please Visit Again\n\n";
    }
};

int main() 
{
    cout << "\n--------------------------------------------------------------" << endl;
    cout << "\t\t*** Welcome to Store ***\n";
    cout << "--------------------------------------------------------------" << endl;

    Bill b;
    b.input();
    if (b.tqua > 0) 
    {
        b.billCal();
        b.billDisplay();
        b.payment();
    } 
    else 
    {
        cout << "\nNo items entered. Exiting...\n";
    }
    return 0;
}
