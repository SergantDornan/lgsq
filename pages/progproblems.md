# Тинькофф контесты
collapsed:: true
	- ![Экзамен по программированию.pdf](../assets/Экзамен_по_программированию_1726490651761_0.pdf)
		- 1
		  collapsed:: true
			- Мое Решение
			  collapsed:: true
				- ```c++
				  
				  
				  bool num(char ch){
				  	return (ch == '0') || (ch == '1') || (ch == '2') || (ch == '3') || (ch == '4') || (ch == '5') || (ch == '6') || (ch == '7') || (ch == '8') || (ch == '9');
				  }
				  
				  int main(){
				  std::string str;
				  	std::cin >> str;
				  	std::vector<int> v = {};
				  	int currnum = 0;
				  	bool b = false;
				  	bool write = true;
				  	std::string tmp;
				  	for(int i = 0; i < str.size(); ++i){
				  		if(num(str[i]) && write){
				  			write = false;
				  			tmp.clear();
				  			for(int j = i; j < str.size(); ++j){
				  				if(!num(str[j]))
				  					break;
				  				tmp+=str[j];
				  			}
				  			if(i+tmp.size() == str.size())
				  				v.push_back(atoi(tmp.c_str()));
				  		}
				  		else{
				  
				  			if(str[i] == '-' && !b){
				  				b = true;1
				  				currnum = atoi(tmp.c_str());
				  			}
				  			else if(str[i] == ',' && !b){
				  				
				  				v.push_back(atoi(tmp.c_str()));
				  			}
				  			else if(str[i] == ',' && b){
				  				b = false;
				  				int x = atoi(tmp.c_str());
				  				for(int j = currnum; j <= x; ++j){
				  					v.push_back(j);
				  				}
				  			}
				  			write = true;
				  		}
				  	}
				  	for(int i = 0; i < v.size(); ++i)
				  		std::cout << v[i] << ' ';
				  }
				  ```
		- 2
		  collapsed:: true
			- Мое решение
			  collapsed:: true
				- ```c++
				  
				  int main(){
				  int N;
				  	std::cin >> N;
				  	std::string s;
				  	std::vector<int> v;
				  	for(int i = 0; i < N; ++i){
				  		std::cin >> s;
				  		v.push_back(atoi(s.c_str()));
				  	}
				  	bool b = true;
				  	for(int i = 0; i < N; ++i){
				  		if(v[i] == -1){
				  			if(i == 0)
				  				v[i] = 1;
				  			else
				  				v[i] = v[i-1] + 1;
				  		}
				  		else{
				  			if(i != 0){
				  				if(v[i] <= v[i-1]){
				  				b = false;
				  				break;
				  				}
				  			}
				  		}
				  	}
				  	if(b){
				  		std::cout << "YES" << std::endl;
				  		std::cout << v[0] << ' ';
				  		for(int i = 1; i < v.size(); ++i){
				  			std::cout << v[i] - v[i-1] << ' ';
				  		}
				  	}
				  	else{
				  		std::cout << "NO";
				  	}
				  }
				  ```
		- 3
		  collapsed:: true
			- Мое решение
			  collapsed:: true
				- ```c++
				  bool check(std::string s, std::string s2){
				  	for(int j = 0; j < s2.size(); ++j){
				  					if(s.find(s2[j]) == std::string::npos){
				  						return false;
				  					}
				  				}
				  	return true;
				  }
				  int main(){
				  std::string s1,s2;
				  		int k;
				  		std::cin >> s1;
				  		std::cin >> s2;
				  		std::cin >> k;
				  		std::string v;
				  		std::string pswds;
				  		for(int i = 0; i < s1.size(); ++i){
				  			if(s2.find(s1[i]) != std::string::npos){
				  				if(v.size() < k)
				  					v += s1[i];
				  				else{
				  					std::string tmp = v;
				  					tmp.erase(tmp.begin());
				  					tmp+=s1[i];
				  					if(!check(tmp,s2) && check(v,s2)){
				  						pswds = v;
				  					}
				  					v = tmp;
				  				}
				  				if((i == s1.size() - 1) && check(v,s2))
				  					pswds = v;
				  			}
				  			else{
				  				if(check(v,s2))
				  					pswds = v;
				  				v.clear();
				  			}
				  		}
				  		if(pswds.size() == 0)
				  						std::cout << "-1" << std::endl;
				  					else{
				  						std::cout << pswds << std::endl;
				  					}
				  }
				  ```
		- 4
		  collapsed:: true
			- Мое решение
			  collapsed:: true
				- ```c++
				  int countDiv(std::vector<std::pair<int,int>> v){
				  	int res = 1;
				  	for(int i = 0; i < v.size(); ++i){
				  		res *= (v[i].second+1);
				  	}
				  	return res;
				  }
				  
				  
				  bool simp(int x){
				  	if(x == 1)
				  		return false;
				  	for(int i = 2; i*i <= x; ++i){
				  		if(x % i == 0)
				  			return false;		
				  	}
				  	return true;
				  }
				  
				  std::vector<std::pair<int,int>> Canon(int x){
				  	std::vector<std::pair<int,int>> v;
				  	if(simp(x)){
				  		v.push_back({x,1});
				  	}
				  	else if(x == 1){
				  		v.push_back({1,0});
				  	}
				  	else{
				  		int N = x;
				  	for(int i = 2; i < N; ++i){
				  		if(simp(i)){
				  			if(x % i == 0){
				  				v.push_back({i,0});
				  				while(x % i == 0){
				  					v[v.size()-1].second++;
				  					x = x / i;
				  				}
				  			}
				  		}
				  	}
				  	}
				  	return v;
				  }
				  int main(){
				  int l,r;
				  	std::cin >> l;
				  	std::cin >> r;
				  	int count = 0;
				  	for(int x = l; x <= r; ++x){
				  		if(!simp(x)){
				  			if(simp(countDiv(Canon(x)))){
				  				count++;
				  			}
				  		}
				  	}
				  	std::cout << count << std::endl;
				  }
				  ```
- ajoiajiojoijsiojaiojioijoijionlkncklanscklnaklcnlaksnckl