---
{"dg-publish":true,"permalink":"/coding/companies-problems/find-maximum-sub-squares-of-size-k-x-k/","created":"2023-09-23T15:06:59.638+02:00","updated":"2023-09-23T15:06:59.638+02:00"}
---

# Find maximum sub-squares of size k x k
//TO-DO

```cpp
vector<pair<int, int>> getMaxMatrix(vector<vector<int>>& matrix, int k) {
    int n = matrix.size();
    vector<vector<int>> helperMatrix(n - k + 1, vector<int>(n));
    unordered_map<vector<pair<int, int>> > map;
    // k must be smaller than or equal to n
    if (k > n)
        return {};

    for(int j = 0; j < n; j++) {
        for(int i = 0; i < k; i++)
            helperMatrix[0][j] += matrix[i][j];

        for(int i = 1; i < n - k + 1; i++)
            helperMatrix[i][j] = helperMatrix[i - 1][j] + matrix[i + k - 1][j] - matrix[i - 1][j];
    }
    int maxSoFar = 0;
    for(int i = 0; i < n - k + 1; i++) {
        int maxHere = 0;
        for(int j = 0; j < k; j++)
            maxHere += helperMatrix[i][j];

        if(maxHere > maxSoFar) {
            maxSoFar = maxHere;
            map[maxSoFar].push_back({i, 0});
        }    

        for(int j = 1; j < n - k + 1; j++) {
            maxHere += helperMatrix[i][j + k - 1] - helperMatrix[i][j - 1];
            if(maxHere > maxSoFar) {
                maxSoFar = maxHere;
                map[maxSoFar].push_back({i, j});
            }
        }
    }
    
    return map[maxSoFar];
}

int main() {
     vector<vector<int>> matrix = { {1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
    //printSum_v1(matrix, 2);
    vector<pair<int, int>> res = getMaxMatrix(matrix, 2);

    for(auto p : res) {
        cout << "{ " + p.first + ", " + p.second + " }";
    }
}
```