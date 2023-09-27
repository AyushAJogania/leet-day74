# leet-day74

# Simplify Path

## Problem Description

Given an absolute Unix-style file system path, simplify it to the canonical path. The canonical path should follow these rules:

1. The path starts with a single slash `/`.
2. Any two directories are separated by a single slash `/`.
3. The path does not end with a trailing `/`.
4. The path only contains valid directory names or special directory references `.` (current directory) and `..` (parent directory).

## Examples

### Example 1

**Input:**

```
"/home/"
```

**Output:**

```
"/home"
```

**Explanation:**

- There is no trailing slash after the last directory name.
- The canonical path contains a single directory, which is "home."

### Example 2

**Input:**

```
"/../"
```

**Output:**

```
"/"
```

**Explanation:**

- Going one level up from the root directory is a no-op, and the canonical path is still the root directory `/`.

### Example 3

**Input:**

```
"/home//foo/"
```

**Output:**

```
"/home/foo"
```

**Explanation:**

- In the canonical path, multiple consecutive slashes `//` are replaced by a single one, and the path becomes "/home/foo."

## Constraints

- The length of the input path is between 1 and 3000 characters.
- The input path consists of English letters, digits, periods `.` (for the current directory), slashes `/`, or underscores `_`.
- The input path is a valid absolute Unix path.

## Approach

To simplify a Unix-style absolute path, you can follow these steps:

1. Split the path into its components using the `/` delimiter.
2. Initialize an empty stack to keep track of the canonical path.
3. Iterate through the components of the path.
4. For each component:
   - If it is an empty string (due to multiple consecutive slashes `//`), skip it.
   - If it is `.` (current directory), continue to the next component.
   - If it is `..` (parent directory), pop the last directory from the stack (go up one level).
   - Otherwise, push the current component onto the stack.
5. After processing all components, build the simplified canonical path from the stack.
6. Return the canonical path as a string.

## Example Code

```cpp
class Solution {
public:
    string simplifyPath(string path) {
        // Split the path into components using '/' as the delimiter
        vector<string> components;
        stringstream ss(path);
        string component;
        while (getline(ss, component, '/')) {
            components.push_back(component);
        }

        stack<string> canonicalPath;
        
        // Process each component
        for (const string& comp : components) {
            if (comp.empty() || comp == ".") {
                // Ignore empty components and '.'
                continue;
            } else if (comp == "..") {
                // Go up one level by popping from the stack
                if (!canonicalPath.empty()) {
                    canonicalPath.pop();
                }
            } else {
                // Push valid directory names onto the stack
                canonicalPath.push(comp);
            }
        }

        // Build the simplified canonical path from the stack
        string simplifiedPath = "";
        while (!canonicalPath.empty()) {
            simplifiedPath = "/" + canonicalPath.top() + simplifiedPath;
            canonicalPath.pop();
        }
        
        // Handle the case of an empty path
        if (simplifiedPath.empty()) {
            simplifiedPath = "/";
        }

        return simplifiedPath;
    }
};
```

## Complexity Analysis

- Time Complexity: O(N), where N is the length of the input path.
- Space Complexity: O(N) for storing the components and the canonical path in the stack.
