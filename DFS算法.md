# DFS算法

## **排列数字**

* ![image-20230504154641107](https://raw.githubusercontent.com/pkxzs/PicBed/main/Img/image-20230504154641107.png)

* ```c++
  #include<iostream>
  
  using namespace std;
  
  int n;
  int path[10];
  bool st[10];
  
  void dfs(int u)
  {
  	if (u == n)
  	{
  		for (int x = 0; x < n; x++)
  			cout << path[x] << " ";
  		cout << endl;
  		return;
  	}
  	for (int x = 1; x <= n; x++)
  	{
  		if (!st[x])
  		{
  			path[u] = x;
  			st[x] = true;
  			dfs(u + 1);
  			st[x] = false;
  		}
  	}
  }
  
  int main()
  {
  	cin >> n;
  	dfs(0);
  	return 0;
  }
  ```

* ![image-20230504154724656](https://raw.githubusercontent.com/pkxzs/PicBed/main/Img/image-20230504154724656.png)



## **N皇后问题**

* 斜线坐标的推理
  * ![image-20230504204649372](https://raw.githubusercontent.com/pkxzs/PicBed/main/Img/image-20230504204649372.png)

* 第一种搜索顺序：

* 采用排列的方式查找

  * ```c++
    #include<iostream>
    
    using namespace std;
    
    int n;
    const int N = 20;
    char map[N][N];
    bool raw[N], dg[N], udg[N];//判断列和对角线是否还可以放
    
    void DFS(int u)
    {
    	if (u == n)
    	{
    		for (int x = 0; x < n; x++)
    			puts(map[x]);
    		puts("");
    		return;
    	}
    
    	for (int x = 0; x < n; x++)
    	{
    		if (!raw[x] && !dg[u + x] && !udg[n - u + x])//类比x-y坐标轴，将x-y坐标轴倒过来
    		{
    			raw[x] = dg[u + x] = udg[n - u + x] = true;
    			map[u][x] = 'Q';
    			DFS(u + 1);
    			raw[x] = dg[u + x] = udg[n - u + x] = false;
    			map[u][x] = '.';
    		}
    	}
    
    }
    
    int main()
    {
    	cin >> n;
    	for (int x = 0; x < n; x++)
    	{
    		for (int y = 0; y < n; y++)
    		{
    			map[x][y] = '.';
    		}
    	}
    	DFS(0);
    	return 0;
    }
    ```

* 第二种搜索顺序：

* 每个位置挨着搜索

* ```c++
  #include<iostream>
  
  using namespace std;
  
  int n;
  const int N = 20;
  char map[N][N];
  bool row[N],lst[N] ,dg[N], udg[N];//判断列和对角线是否还可以放
  
  void DFS(int x, int y, int m)
  {
  	if (y == n)
  	{
  		y = 0;
  		x++;
  	}
  	if (x == n)
  	{
  		if (m == n)
  		{
  			for (int x = 0; x < n; x++)
  			{
  				puts(map[x]);
  			}
  			puts("");
  		}
  		return;
  	}
  
  	//不放
  	DFS(x, y+1, m);
  
  	//放
  	if (!row[x] && !lst[y] && !dg[x + y] && !udg[y - x + n])
  	{
  		row[x] = lst[y] = dg[x + y] = udg[y - x + n] = true;
  		map[x][y] = 'Q';
  		DFS(x, y+1, m+1);
  		map[x][y] = '.';
  		row[x] = lst[y] = dg[x + y] = udg[y - x + n] = false;
  	}
  }
  
  int main()
  {
  	cin >> n;
  	for (int x = 0; x < n; x++)
  	{
  		for (int y = 0; y < n; y++)
  		{
  			map[x][y] = '.';
  		}
  	}
  	DFS(0, 0, 0);
  	return 0;
  }
  ```

* 

**走迷宫**：

* ```c++
  #include<iostream>
  #include<vector>
  using namespace std;
  
  
  vector<char> way;
  typedef pair<int, int> xy;
  vector<xy> direc = { {0,1},{1,0},{0,-1},{-1,0} };//右下左上
  char op[4] = { 'R', 'D', 'L', 'U' };
  const int N = 6;
  int map[N][N] = {
  	{1,1,1,1,1,1},
  	{0,0,1,0,0,0},
  	{0,0,1,1,0,0},
  	{0,0,0,1,1,0},
  	{0,0,0,0,1,0},
  	{0,0,0,0,1,1}
  
  };
  int walked[N][N] = { 0 };
  
  void DFS(int x, int y)
  {
  	if (x == 5 && y == 5)
  	{
  		for (auto x : way)
  			cout << x << " ";
  		cout << endl;
  		return;
  	}
  	if (x < 0 || x >= 6 || y < 0 || y >= 6)
  		return;
  
  	for (int i = 0; i < 4; i++)
  	{
  		if (walked[x + direc[i].first][y + direc[i].second] == 0 && map[x + direc[i].first][y + direc[i].second] == 1)
  		{
  			walked[x + direc[i].first][y + direc[i].second] = 1;
  			way.push_back(op[i]);
  			DFS(x + direc[i].first, y + direc[i].second);
  			way.pop_back();
  			walked[x + direc[i].first][y + direc[i].second] = 0;
  		}
  	}
  	return;
  }
  
  int main()
  {
  	DFS(0, 0);
  	return 0;
  }
  ```

* 
