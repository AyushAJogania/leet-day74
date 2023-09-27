# leet-day74

# Decoded String at Index

## Problem Description

You are given an encoded string `s`. To decode the string to a tape, the encoded string is read one character at a time, and the following steps are taken:

1. If the character read is a letter, that letter is written onto the tape.
2. If the character read is a digit `d`, the entire current tape is repeatedly written `d - 1` more times in total.

Given an integer `k`, return the `kth` letter (1-indexed) in the decoded string.

### Example 1

**Input:** `s = "leet2code3", k = 10`
**Output:** "o"
**Explanation:** The decoded string is "leetleetcodeleetleetcodeleetleetcode". The 10th letter in the string is "o".

### Example 2

**Input:** `s = "ha22", k = 5`
**Output:** "h"
**Explanation:** The decoded string is "hahahaha". The 5th letter is "h".

### Constraints

- `2 <= s.length <= 100`
- `s` consists of lowercase English letters and digits `2` through `9`.
- `s` starts with a letter.
- `1 <= k <= 10^9`
- It is guaranteed that `k` is less than or equal to the length of the decoded string.
- The decoded string is guaranteed to have less than 2^63 letters.

## Approach and Algorithm

To solve this problem optimally, we can follow these steps:

1. Calculate the length of the decoded string (`decodedLength`) without actually decoding it. This can be done by iterating through the characters of the input string `s`.
   
2. Iterate through the characters of the input string in reverse order (from the end to the beginning). This is because, to find the `kth` character, we want to work backward from the end of the decoded string.

3. For each character `c` at index `i`, reduce `k` to the equivalent position within the current decoded string by taking the modulo of `k` with `decodedLength`.

4. If `k` becomes `0` and the current character `c` is a letter, return it as the result.

5. If the current character `c` is a digit, update `decodedLength` by dividing it by the value of the digit.

6. If the current character `c` is a letter, decrement `decodedLength` by `1` to account for adding that letter to the decoded string.

7. If the loop completes without finding the `kth` character, return an empty string.

## Implementation

I've provided a C++ solution for this problem. You can use the `decodeAtIndex` method to find the `kth` character in the decoded string.

```cpp
class Solution {
public:
    string decodeAtIndex(string s, int k) {
        long long decodedLength = 0;
        
        for (char c : s) {
            if (isdigit(c)) {
                int digit = c - '0';
                decodedLength *= digit;
            } else {
                decodedLength++;
            }
        }
        
        for (int i = s.size() - 1; i >= 0; i--) {
            char c = s[i];
            k %= decodedLength; // Reduce k to the equivalent position within the current decoded string.
            
            if (k == 0 && isalpha(c)) {
                return string(1, c);
            }
            
            if (isdigit(c)) {
                int digit = c - '0';
                decodedLength /= digit;
            } else {
                decodedLength--;
            }
        }
        
        return "";
    }
};
```
