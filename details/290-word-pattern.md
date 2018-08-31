# 290.Word Pattern 

Given a `pattern` and a string `str`, find if `str` follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `str`.

**Example 1:**

```
Input: pattern = "abba", str = "dog cat cat dog"
Output: true
```

**Example 2:**

```
Input:pattern = "abba", str = "dog cat cat fish"
Output: false
```

**Example 3:**

```
Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false
```

**Example 4:**

```
Input: pattern = "abba", str = "dog dog dog dog"
Output: false
```

**Notes:**
You may assume `pattern` contains only lowercase letters, and `str` contains lowercase letters separated by a single space.

## Swift代码

思路：双向校验。使用HashMap判断是否存在，不存在的话，存到两个HashMap里，存在是否和反向内容一致，不一致则返回false

```swift
func wordPattern(_ pattern: String, _ str: String) -> Bool {
        var strArr = str.components(separatedBy: " ");
        if pattern.count != strArr.count {
            return false
        }
        var keyValueDic = Dictionary<Character, String>()
        var keyValueDic2 = Dictionary<String, Character>()
        for (index,key) in pattern.enumerated() {
            let tempStr = keyValueDic[key]
            if tempStr == nil {
                if keyValueDic2[strArr[index]] != nil {
                    return false;
                }
                keyValueDic[key] = strArr[index]
                keyValueDic2[strArr[index]] = key
            }
            else {
                if tempStr == strArr[index] && keyValueDic2[tempStr!] == key {
                    continue
                }
                else {
                    return false
                }
            }
        }
        return true
    }
```

