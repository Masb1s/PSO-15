#include <iostream>
#include <vector>
#include <string>
#include <limits>
#include <regex>
using namespace std;

int getInt(string msg) {
    int x;
    while (true) {
        cout << msg;
        cin >> x;
        if (!cin.fail()) return x;
        cin.clear();
        cin.ignore(9999, '\n');
        cout << "Enter a NUMBER!\n";
    }
}

double getDouble(string msg) {
    double x;
    while (true) {
        cout << msg;
        cin >> x;
        if (!cin.fail()) return x;
        cin.clear(); cin.ignore(9999, '\n');
        cout << "Enter a NUMBER!\n";
    }
}

string getText(string msg) {
    regex rgx("^[A-Za-z ]+$");
    string s;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    while (true) {
        cout << msg;
        getline(cin, s);
        if (regex_match(s, rgx)) return s;
        cout << "Letters only!\n";
    }
}

class Building {
protected:string name; int year; double area;
public:
    Building(string n, int y, double a) :name(n), year(y), area(a) {}
    virtual void show() { cout << "Building: " << name << ", " << year << ", " << area << " m2\n"; }
    virtual ~Building() {}
};

class Campus :public Building {
    int rooms; string uni;
public:
    Campus(string n, int y, double a, int r, string u) :Building(n, y, a), rooms(r), uni(u) {}
    void show() override {
        cout << "Campus: " << name << ", " << year << ", " << area << " m2, rooms:" << rooms << ", uni:" << uni << "\n";
    }
};

class Device {
protected:string brand; double price;
public:
    Device(string b, double p) :brand(b), price(p) {}
    virtual void show() { cout << "Device: " << brand << ", " << price << "$\n"; }
    virtual ~Device() {}
};

class Phone :public Device {
    string model; double screen;
public:
    Phone(string b, double p, string m, double s) :Device(b, p), model(m), screen(s) {}
    void show() override {
        cout << "Phone: " << brand << " " << model << ", " << price << "$, " << screen << "\"\n";
    }
};

template<typename T>
vector<T> searchRange(vector<T> v, T mn, T mx) {
    vector<T> out;
    for (T e : v) if (e >= mn && e <= mx) out.push_back(e);
    return out;
}

template<typename A, typename B>
class Pair {
    A a; B b;
public:
    Pair(A x, B y) :a(x), b(y) {}
    void show() { cout << "(" << a << "," << b << ")\n"; }
};


int main() {
    while (true) {
        cout << "\n--- MENU ---\n"
            << "1. Buildings / Campus\n"
            << "2. Device / Phone\n"
            << "3. Search template\n"
            << "4. Pair template\n"
            << "0. Exit\n";

        int c = getInt("Choose: ");

        if (c == 1) {
            vector<Building*> v;
            v.push_back(new Building(getText("Name: "), getInt("Year: "), getDouble("Area: ")));
            v.push_back(new Campus(getText("Campus name: "), getInt("Year: "),
                getDouble("Area: "), getInt("Rooms: "), getText("University: ")));
            for (auto x : v)x->show();
            for (auto x : v)delete x;
        }
        else if (c == 2) {
            vector<Device*> v;
            v.push_back(new Device(getText("Brand: "), getDouble("Price: ")));
            v.push_back(new Phone(getText("Phone brand: "), getDouble("Price: "),
                getText("Model: "), getDouble("Screen: ")));
            for (auto x : v)x->show();
            for (auto x : v)delete x;
        }
        else if (c == 3) {
            vector<int> a = { 5,15,25,10,20,30 };
            int mn = getInt("min: "), mx = getInt("max: ");
            for (int x : searchRange(a, mn, mx)) cout << x << " ";
            cout << "\n";
        }
        else if (c == 4) {
            Pair<int, string> p1(10, "Hello");
            Pair<string, double> p2("Pi", 3.14);
            p1.show(); p2.show();
        }
        else if (c == 0) break;
        else cout << "Invalid!\n";
    }
}
