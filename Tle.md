# **What is Bit Masking?**

- **Definition:**  
    Bit masking involves using binary representation and bitwise operators to manipulate or track specific bits in an integer.  
    A "bitmask" is an integer that acts as a filter to turn certain bits "on" or "off."
    
- **Applications:**
    
    - Represent subsets of a set.
    - Solve state-space problems efficiently.
    - Optimize memory usage for boolean states.
- **Advantages:**
    
    - Space-efficient.
    - Fast operations due to low-level bitwise manipulation.

---

## **Basic Bitwise Operations**

### **1. Setting a Bit**

- To turn on the kthk^{th}kth bit:
    - **Using Left Shift (<<):**
        
        cpp
        
        Copy code
        
        `num |= (1 << k);`
        
    - **Using Right Shift (>>):**
        
        cpp
        
        Copy code
        
        `num |= (1 >> (31 - k));`
        

**Example:**  
Turn on the 2nd bit of `5` (binary `101`):

cpp

Copy code

`num = 5; // 101 num |= (1 << 2); // 101 | 100 = 111 (7 in decimal)`

### **2. Checking a Bit**

- To check if the kthk^{th}kth bit is set:
    
    cpp
    
    Copy code
    
    `bool isSet = (num & (1 << k)) != 0;`
    

### **3. Clearing a Bit**

- To turn off the kthk^{th}kth bit:
    
    cpp
    
    Copy code
    
    `num &= ~(1 << k);`
    

### **4. Toggling a Bit**

- To flip the kthk^{th}kth bit:
    
    cpp
    
    Copy code
    
    `num ^= (1 << k);`
    

### **5. Checking the Least Significant Bit (LSB)**

cpp

Copy code

`bool lsb = num & 1;`

### **6. Counting Set Bits**

- Use `__builtin_popcount(num)` in C++ or `bin(num).count('1')` in Python.

---

## **Bit Masking in DP**

- **State Representation:** Use a bitmask to represent subsets or combinations.  
    For example, in the Travelling Salesman Problem (TSP), a bitmask dp[mask][pos]dp[mask][pos]dp[mask][pos] can represent whether a subset of cities has been visited.
    
- **Transitions:** Use bitwise operations to update states. For example:
    
    - Adding an element to a subset:
        
        cpp
        
        Copy code
        
        `newMask = mask | (1 << k);`
        
    - Removing an element:
        
        cpp
        
        Copy code
        
        `newMask = mask & ~(1 << k);`
        

---

## **Question 1: Set a Bit**

### **Using Left Shift**

cpp

Copy code

`void setBitLeftShift(int &num, int k) {     num |= (1 << k); }`

### **Using Right Shift**

cpp

Copy code

`void setBitRightShift(int &num, int k) {     num |= (1 >> (31 - k)); // Only works in specific systems with 32-bit integers }`

---

## **Question 2: Find Sum for Particular Number Using Bit Masking**

### **Problem Statement:**

Given an array of integers, find the sum of elements corresponding to set bits in a number.

### **Approach:**

- Iterate through the bits of the number.
- Check which bits are set.
- Add the corresponding elements of the array.

### **Code:**

cpp

Copy code

`#include <iostream> #include <vector> using namespace std;  int findSumUsingBitMask(const vector<int> &arr, int mask) {     int sum = 0;     for (int i = 0; i < arr.size(); ++i) {         if (mask & (1 << i)) { // Check if i-th bit is set             sum += arr[i];         }     }     return sum; }  int main() {     vector<int> arr = {1, 2, 3, 4}; // Array elements     int mask = 5; // Binary: 0101     cout << "Sum: " << findSumUsingBitMask(arr, mask) << endl;     // Output: 1 + 3 = 4     return 0; }`

---

## **Real-World Problem Example**

### **Problem: Traveling Salesman Problem (TSP)**

- **State Representation:**  
    dp[mask][i]dp[mask][i]dp[mask][i] represents the minimum cost to visit the subset of cities denoted by `mask`, ending at city iii.
    
- **Transition:**  
    Add a city jjj to the subset:
    
    cpp
    
    Copy code
    
    `dp[newMask][j] = min(dp[newMask][j], dp[mask][i] + cost[i][j]);`
    

### **Code Skeleton:**

cpp

Copy code

`#include <iostream> #include <vector> #include <climits> using namespace std;  const int INF = INT_MAX;  int tsp(const vector<vector<int>> &cost, int n) {     int totalStates = 1 << n; // Total subsets of cities     vector<vector<int>> dp(totalStates, vector<int>(n, INF));      dp[1][0] = 0; // Start from city 0      for (int mask = 0; mask < totalStates; ++mask) {         for (int i = 0; i < n; ++i) {             if (mask & (1 << i)) { // If city i is in the subset                 for (int j = 0; j < n; ++j) {                     if (!(mask & (1 << j))) { // If city j is not in the subset                         int newMask = mask | (1 << j);                         dp[newMask][j] = min(dp[newMask][j], dp[mask][i] + cost[i][j]);                     }                 }             }         }     }      int result = INF;     for (int i = 1; i < n; ++i) {         result = min(result, dp[(1 << n) - 1][i] + cost[i][0]); // Return to start     }     return result; }  int main() {     vector<vector<int>> cost = {         {0, 10, 15, 20},         {10, 0, 35, 25},         {15, 35, 0, 30},         {20, 25, 30, 0}     };     int n = cost.size();     cout << "Minimum TSP cost: " << tsp(cost, n) << endl;     return 0; }`

---

## **Practice Questions**

1. **Subset Sum Problem:** Use bit masking to determine if a subset sums to kkk.
2. **Count Valid Words:** Given a list of words and a bitmask for allowed letters, count valid words.
3. **Job Assignment Problem:** Assign nnn jobs to nnn people using DP with bit masking.

This note equips you with a strong foundation for solving bit masking problems in competitive programming.

4o