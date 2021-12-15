# Time based Key Value Store 
## https://leetcode.com/problems/time-based-key-value-store

Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

Implement the TimeMap class:

TimeMap() Initializes the object of the data structure.

void set(String key, String value, int timestamp) Stores the key key with the value value at the given time timestamp.

String get(String key, int timestamp) Returns a value such that set was called previously, with timestamp_prev <= timestamp. 

If there are multiple such values, it returns the value associated with the largest timestamp_prev (closest previous, if exact timestamp value is not available). If there are no values, it returns "".
 
![Time based key value](example.JPG?raw=true) 
 
## Constraints :
```
1. 1 <= key.length, value.length <= 100
2. key and value consist of lowercase English letters and digits.
3. 1 <= timestamp <= 107
4. All the timestamps timestamp of set are strictly increasing.
5. At most 2 * 105 calls will be made to set and get.
```

```
Example 1:

Input
["TimeMap", "set", "get", "get", "set", "get", "get"]
[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]
Output
[null, null, "bar", "bar", null, "bar2", "bar2"]

Explanation
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
timeMap.get("foo", 1);         // return "bar"
timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.
timeMap.get("foo", 4);         // return "bar2"
timeMap.get("foo", 5);         // return "bar2"
``` 

## Implementation :
```java

class TimeMap {
    
    Map<String, List<Data>> map;
    class Data {
    int timestamp;
    String value;
        Data(int time, String val) {
            this.timestamp = time;
            this.value = val;
        }
    }
    
    public TimeMap() {
        map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        map.putIfAbsent(key, new ArrayList<Data>());
        map.get(key).add(new Data(timestamp, value));
    }
    
    public String get(String key, int timestamp) {
        if(!map.containsKey(key))
            return "";
        return findClosestValue(map.get(key), timestamp);
    }
    
    private String findClosestValue(List<Data> values, int timestamp) {
        int left = 0, right = values.size() - 1;
        while(left < right) {
            int mid = (left + right + 1) / 2;
            if(values.get(mid).timestamp <= timestamp) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        Data closestData = values.get(left);
        return closestData.timestamp > timestamp ? "" : closestData.value;
    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */

```

# References :
https://www.youtube.com/watch?v=kZAZqn_J8Fo

