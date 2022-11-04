## 线性dp:

### 最长上升子序列

```cpp
dp[i]:以ai结尾的最长上升子序列长度
	dp[1]=1:边界值
	dp[i]=max{1,dp[j]+1}{ai>aj,j
```

### 最长有效括号<!--  -->

```cpp
      //状态表示
	dp[i]:以i开始(包含元素i)到结尾的最长有效括号表达式长度，初始全置为0
      //求解过程
	for(i=len-2;i>=0;i--)
	{
	  if(i+1+dp[i+1]号元素存在&&i+1+dp[i+1]号元素为")"&&i号元素为"("){
		dp[i]=dp[i+1]+2;
		if(1+i+dp[i+1]i+1号元素存在){
		   dp[i]+=dp[i+1+dp[i+1]+1]
		}  
	}
```

### 最长公共子序列

```cpp
//状态表示
dp[i,j]:X的前i个字符和Y的前j个元素的最大公共子序列
//状态转移
dp[i,j]=dp[i-1,j-1]+1 (Xi=Yj)
dp[i,j]=max{dp[i-1,j],dp[i,j-1]}(Xi!=Yj)
//边界值
dp[i][j]=0 (i==0 or j==0)
```

### 鸡蛋掉落

```cpp
//K个鸡蛋，有1到N层的楼层,存在楼层F,>F的楼层将鸡蛋扔下都会碎，<=F的楼层将鸡蛋扔下都不会碎，求至少需要的次数(即最坏情况下的次数)
	//状态表示
	dp[n][m]: n个鸡蛋，m层楼，确定F的至少需要的次数
	//状态转移
	dp[i][j]=min{res,max{dp[i-1][floor-1],dp[i][j-floor]}+1}(floor:1-->N)
```

### 编辑距离

版本一：

```cpp
//给定两个字符串A,B 可进行如下操作
//delete:删除A中任意一个字符
//insert:在A的任意某个位置插入任意字符
//change:将A的任意一个字符替换掉
  
  //状态表示
  dp[i][j]:A的开头到i号和B的开头到j号的编辑距离(使用上述操作将A转换成B的最小次数)
  //转移方程
  dp[i][j]=dp[i-1][j-1] (ai=bj)
  dp[i][j]=min{dp[i-1][j-1],dp[i-1][j],dp[i][j-1]}+1
  //边界值
  dp[i][0]=i;
  dp[0][j]=j;
```

### 通配符匹配

```cpp
//"?"和"*": 前面匹配一个特定字符,后者匹配字符串(可以为空串和单个字符):主串s、模式串p
    //状态表示
	dp[i][j]:s前i个是否能和p的前j个匹配初始化为全false
    //状态转移
        dp[i][j]=dp[i-1][j-1] (si==pj or pj=='?' )
        dp[i][j]=dp[i][j-1]||dp[i-1][j] (pj=='*')
   //边界值
 	dp[0][0]=true
	dp[0][i]=dp[0][i-1] (pi=='*')

```

## 区间dp

### 最长回文子串

```cpp
//状态表示
dp[i][j]: 以i开头j结尾是否是式回文串
//状态转移
dp[i][j]=dp[i+1][j-1] (si=sj)
dp[i][j]=0 (si!=sj)
//边界条件
dp[i][i]=1;
```

### 判断是否是扰乱字符串

```cpp

```

## 背包dp

### 组合总和

```cpp
//给一个整数数组nums，一个目标数，从数组中挑数进行组合，总和为目标数的组合个数(每个数可以选择多次)

//状态表示
dp[i]:总和为i的组合个数
//状态转移
dp[i]=sum{dp[i-num]} (num:nums)
//边界
dp[0]=1
```

### 01背包

```cpp
//状态表示
dp[i,w]:在前i个物品中选，背包剩余体积为w时的最大价值
//状态转移
dp[i,w]=dp[i-1,w] (wi>w)
dp[i,w]=max{dp[i-1,w],dp[i-1,w-wi]+vi}
dp[i,w]=0 (i==0)
```

### 完全背包

```cpp
//可以转换成01,所谓完全就是每个物品不止选一个了
//状态表示
dp[i,w]:在前i个物品中选，背包剩余体积为w时的最大价值
//状态转移
dp[i,w]=dp[i-1,w] (wi>w)
dp[i,w]=max{dp[i-1,w],dp[i,w-wi]+vi}
dp[i,w]=0 (i==0)
```

## 树形dp

### 二叉树中最大路径和

```cpp
//此方法较为繁琐，但易于理解
max(root){
  maxval="min"
  return max(dfs(root),maxval)
}
dfs(root){
 if(!root):return "min";
 left=dfs(root.left)
 right=dfs(root.right)
 maxval=max{maxval,left+root.val+right,left,right}
 return max(left+root.val,right+root,val+root.val)
}
```

## 状态压缩dp

## 数位dp

### 数字为1的个数

```cpp
//十进制下，给出一个整数n，计算所有小于等于n的非负整数中数字1出现的个数
例如n=13，则个数为6

//状态标识
```

## 计数型dp

### 无障碍物版不同路径dp

```cpp
//mxn的网格的左上角出发，到达右下角,每次只能向下or向右走，问多少种走法
//状态表示
dp[i][j]:从起点到i，j的走法
//状态转移
dp[i][j]=dp[i-1][j]+dp[i][j-1]
```

### 障碍物版不同路径dp

```cpp
//和上面一样，但在网格某处有一个障碍物

```

## 记忆化搜索:
