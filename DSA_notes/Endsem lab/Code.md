## NOTE:
```c++
sort(v.begin(), v.end(), [](const pair<int,int>& a, const pair<int,int>& b){
    return a.second < b.second;  // ascending by second
});
```
How to sort a vector containing pairs by the second element


Question 2: Valid parenthesis

```cpp
class Solution {

public:
	
	bool isValid(string s) {
	
		stack <char> st;
		for (auto i : s){
			if(i == '(' || i == '{' || i == '['){
				st.push(i);
			}
			else{
			
			if (st.empty()) return false;
			bool cond = (i == ')' && st.top() == '(') || (i == '}' && st.top() == '{') || (i == ']' && st.top() == '[');
			if(!cond) return false;
			else{
				st.pop();
			}
			}
			
		}
		if(st.empty()) return true;
		else return false;
	
	}

};
```

Question 3: Distinct numbers

```c++
#include <bits/stdc++.h>

using namespace std;

int main(){

	set <int> s;
	
	int n;
	cin >> n;

	for(int i = 0; i < n; i++){
		int no;
		cin >> no;
		s.insert(no);
	}
	cout << s.size() << "\n";
	return 0;

}
```