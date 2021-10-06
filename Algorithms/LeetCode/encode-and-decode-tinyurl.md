## 535. Encode and Decode TinyURL

### Descirption 
> Note: This is a companion problem to the System Design problem: Design TinyURL.

TinyURL is a URL shortening service where you enter a URL such as https://leetcode.com/problems/design-tinyurl and it returns a short URL such as http://tinyurl.com/4e9iAk. Design a class to encode a URL and decode a tiny URL.

There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

Implement the Solution class:
- Solution() Initializes the object of the system.
- String encode(String longUrl) Returns a tiny URL for the given longUrl.
- String decode(String shortUrl) Returns the original long URL for the given shortUrl. It is guaranteed that the given shortUrl was encoded by the same object.

#### Constraints
- 1 <= url.length <= 10,000
- url is guranteed to be a valid URL.

### Sol 1: 
Adopt a map as a lookup table for codes and either use a hashing function or a random code generator to generate the code. Since we're storing the information anyway (hashes only work one-way), we might as well just use a random code generator.

Based on the example, we can create a function that creates a random 6-character code, using the 62 alphanumeric characters. We should make sure to come up with a new code in the rare case that we randomly create a duplicate.

To avoid having to encode the same url twice with different random codes, we can create a reverse lookup table to store already encoded urls. The decode function will just return the entry from the code map.

```C++
class Solution {
public:
	// Encodes a URL to a shortened URL.
	string encode(string longUrl) {
		string code;
		if (!url_code.count(longUrl)) {
			for (int i = 0; i < 6; i++)
				code.push_back(charset[rd() % charset.size()]);
			url_code.insert(pair<string, string>(longUrl, code));
			code_url.insert(pair<string, string>(code, longUrl));
		} else
			code = url_code[longUrl];
		return "http://tinyurl.com/" + code;
	}
	// Decodes a shortened URL to its original URL.
	string decode(string shortUrl) {
		if (shortUrl.size() != 25 || !code_url.count(shortUrl.substr(19, 6)))
			return "";
		return code_url[shortUrl.substr(19, 6)];
	}
private:
	const string charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
	map<string, string> url_code, code_url;
	random_device rd;
};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```
Note:
- Runtime: 8 ms
- Memory Usage: 7.5 MB

### Reference
- [JS, Python, Java, C++ | Easy Map Solution w/ Explanation](https://leetcode.com/problems/encode-and-decode-tinyurl/discuss/1110551/JS-Python-Java-C%2B%2B-or-Easy-Map-Solution-w-Explanation)