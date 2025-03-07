'''cpp
# Monte Carlo Option Pricing

This project implements a Monte Carlo simulation in C++ to estimate the price of options according to the Black-Scholes model.

## Code Example

```cpp
#include <iostream>
#include <cmath>
#include <random>

double monteCarloSimulation(double S0, double K, double r, double sigma, double T, int numSimulations) {
    std::mt19937 generator(std::random_device{}());
    std::normal_distribution<double> distribution(0.0, 1.0);
    
    double sumPayoff = 0.0;
    for (int i = 0; i < numSimulations; i++) {
        double Z = distribution(generator);
        double ST = S0 * exp((r - 0.5 * sigma * sigma) * T + sigma * sqrt(T) * Z);
        double payoff = std::max(ST - K, 0.0);
        sumPayoff += payoff;
    }
    
    return exp(-r * T) * (sumPayoff / numSimulations);
}

int main() {
    double S0 = 100, K = 100, r = 0.05, sigma = 0.2, T = 1.0;
    int numSimulations = 100000;

    double optionPrice = monteCarloSimulation(S0, K, r, sigma, T, numSimulations);
    std::cout << "Estimated Option Price: " << optionPrice << std::endl;

    return 0;
}

