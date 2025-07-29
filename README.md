#include <iostream>
#include <vector>
#include <string>
#include <iomanip>

using namespace std;

class Airwave {
private:
    vector<int> rates;

public:
    void logBreathingRate(int rate) {
        rates.push_back(rate);
        cout << "Logged: " << rate << " breaths/min" << endl;
    }

    double averageBreathingRate() {
        if (rates.empty()) return 0.0;
        int total = 0;
        for (int rate : rates) {
            total += rate;
        }
        return static_cast<double>(total) / rates.size();
    }

    void checkAbnormal() {
        bool abnormalFound = false;
        for (size_t i = 0; i < rates.size(); ++i) {
            if (rates[i] < 12) {
                cout << "Reading " << i+1 << ": " << rates[i] << " bpm - Below normal (bradypnea)" << endl;
                abnormalFound = true;
            } else if (rates[i] > 20) {
                cout << "Reading " << i+1 << ": " << rates[i] << " bpm - Above normal (tachypnea)" << endl;
                abnormalFound = true;
            }
        }
        if (!abnormalFound) {
            cout << "No abnormal readings detected." << endl;
        }
    }
};

int main() {
    Airwave app;
    string input;

    cout << "Welcome to Airwave - Respiratory Rate Monitor" << endl;
    cout << "Enter breathing rates (breaths per minute). Type 'q' to quit and show report." << endl;

    while (true) {
        cout << "Breathing rate: ";
        getline(cin, input);
        if (input == "q" || input == "Q") {
            break;
        }
        try {
            int rate = stoi(input);
            app.logBreathingRate(rate);
        } catch (...) {
            cout << "Please enter a valid integer or 'q' to quit." << endl;
        }
    }

    double avg = app.averageBreathingRate();
    cout << fixed << setprecision(2);
    cout << "\nAverage Breathing Rate: " << avg << " breaths per minute\n" << endl;
    app.checkAbnormal();

    return 0;
}
