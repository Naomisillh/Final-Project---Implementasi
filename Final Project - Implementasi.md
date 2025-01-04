#include <iostream>
#include <string>
using namespace std;

// Konstanta
const int MAX_ORDERS = 100;

// Variabel global
string ownerNames[MAX_ORDERS];
string conditions[MAX_ORDERS];
string treatmentNames[MAX_ORDERS];
double treatmentPrices[MAX_ORDERS];
string statuses[MAX_ORDERS];

int dailyCapacity = 0;
int currentOrders = 0;
double dailyRevenue = 0.0;

// Fungsi
void initializeSystem(int capacity) {
    if (capacity > 0 && capacity <= MAX_ORDERS) {
        dailyCapacity = capacity;
        currentOrders = 0;
        dailyRevenue = 0.0;
        cout << "System initialized with daily capacity: " << dailyCapacity << " orders.\n";
    } else {
        cout << "Invalid capacity. Must be between 1 and " << MAX_ORDERS << ".\n";
    }
}

void addOrder(const string& ownerName, const string& condition, const string& treatmentName, double price) {
    if (currentOrders < dailyCapacity) {
        ownerNames[currentOrders] = ownerName;
        conditions[currentOrders] = condition;
        treatmentNames[currentOrders] = treatmentName;
        treatmentPrices[currentOrders] = price;
        statuses[currentOrders] = "In Progress";
        currentOrders++;
        cout << "Order added for " << ownerName << ".\n";
    } else {
        cout << "Daily capacity reached. Cannot add more orders.\n";
    }
}

void updateStatus(int index, const string& newStatus) {
    if (index >= 0 && index < currentOrders) {
        statuses[index] = newStatus;
        cout << "Status of order " << index << " updated to: " << newStatus << ".\n";
    } else {
        cout << "Invalid order index.\n";
    }
}

void completeOrder(int index) {
    if (index >= 0 && index < currentOrders) {
        if (statuses[index] != "Completed") {
            dailyRevenue += treatmentPrices[index];
            statuses[index] = "Completed";
            cout << "Order " << index << " completed for " << ownerNames[index] << ".\n";
        } else {
            cout << "Order " << index << " is already completed.\n";
        }
    } else {
        cout << "Invalid order index.\n";
    }
}

void showOrders() {
    if (currentOrders == 0) {
        cout << "No orders available.\n";
    } else {
        cout << "Current Orders:\n";
        for (int i = 0; i < currentOrders; ++i) {
            cout << i << ": " << ownerNames[i]
                 << " (" << conditions[i] << ") - " << treatmentNames[i]
                 << " - " << statuses[i] << "\n";
        }
    }
}

void showRevenue() {
    cout << "Total Revenue: $" << dailyRevenue << "\n";
}

int main() {
    initializeSystem(5);
    
    addOrder("Alice", "New", "Cleaning", 50.0);
    addOrder("Bob", "Worn", "Polishing", 30.0);
    
    showOrders();
    
    completeOrder(0);
    updateStatus(1, "Pending");
    
    showOrders();
    showRevenue();
    
    return 0;
}# Final-Project---Implementasi
