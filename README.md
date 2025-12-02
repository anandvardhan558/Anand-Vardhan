# Anand-Vardhan
First repository in github
Author - Anand Vardhan from techno main salt lake

#include <bits/stdc++.h>
using namespace std;

void solve() {
    int N;
    if (!(cin >> N)) return;
    
    vector<long long> A(N);
    vector<long long> candidates;
    
    for(int i = 0; i < N; i++) {
        cin >> A[i];
        candidates.push_back(A[i]);
    }
    
    // Sort and remove duplicate candidates to try as target V
    sort(candidates.begin(), candidates.end());
    candidates.erase(unique(candidates.begin(), candidates.end()), candidates.end());
    
    int max_survivors = 0;
    
    for(long long V : candidates) {
        int cnt = 0;
        long long current_fuel = 0;
        
        for(int i = 0; i < N; i++) {
            if (A[i] > V) {
                // Too tall, must be removed. Becomes fuel.
                current_fuel += A[i];
            } else {
                // A[i] <= V. Candidate for survivor.
                if (current_fuel < 0) {
                    // Previous survivor is starving (debt).
                    // We cannot place a new wall (survivor) here.
                    // Must remove this glass to pay debt.
                    current_fuel += A[i];
                } else {
                    // We are solvent. Keep this glass.
                    cnt++;
                    long long need = V - A[i];
                    
                    if (current_fuel >= need) {
                        // Fully satisfied from left.
                        // Surplus is wasted because survivor is a wall.
                        current_fuel = 0;
                    } else {
                        // Not enough from left. Take what we can and go into debt.
                        // Debt must be paid by future glasses.
                        current_fuel -= need; 
                    }
                }
            }
        }
        
        // If we end with debt, the last survivor cannot be satisfied.
        if (current_fuel < 0) {
            cnt--;
        }
        
        if (cnt > max_survivors) {
            max_survivors = cnt;
        }
    }
    
    // Answer is operations = Total Glasses - Survivors
    cout << (N - max_survivors) << "\n";
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int T;
    if (cin >> T) {
        while(T--) {
            solve();
        }
    }
    return 0;
}
