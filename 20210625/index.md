# 多重算法实现的迷宫求解问题

## 前言

感谢组员的共同协作。

### 题目背景

迷宫问题是取自心理学的一个古典实验。在该实验中，将一只老鼠放入一个无顶大盒子的门口处，在出口处放置一块奶酪，奶酪吸引老鼠在盒子中寻找出口。对同一只老鼠进行反复实验，最终老鼠学会走通迷宫路线并不走错一步。

### 题目重述

“迷宫求解”是指在规定的迷宫中寻找从入口到出口路径的问题。即从入口出发，顺着某一方向向前探索，若能走通，则继续向前走；否则沿原路退回，换一个方向继续探索，直到探索到所有可能连通的路径为止。

### 题目分析

解决迷宫问题，首先需要利用一种方式，如二维数组将其存储起来。由于计算机解迷宫时，常用的是“穷举求解”的办法，即从入口出发，顺某一方向向前探索，若能走通，则继续往前走；否则沿原路返回，换一个方向继续探索，直至所有可能的通路都探索为止。为了保证在任何位置上都能按原路返回，显然需要用一个后进先出的结构来保存从入口到当前位置的路径。

### 算法设计
```
（1）回溯法
（2）基于深度遍历的算法
（3）基于广度遍历的算法
（4）转换为图的图论算法
```

## 整体代码

### 回溯法-顺序栈

```C++
#include<stdlib.h>
#include<stdio.h>
#include<iostream>
using namespace std;
#define RANGE 4
#define row 4
#define col 4

#define OK 1
#define ERROR 0
#define INFEASIBLE -1
#define OVER_FLOW -2
typedef int Status;
typedef int DirectiveType;
//位置坐标
typedef struct {
	int x, y;//表示迷宫中的位置信息x行y列
}PostType;
//迷宫类型
typedef struct {
	int map[row + 2][col + 2];//用户输入矩阵（0，1）表示迷宫的初始生成
	char arr[RANGE + 2][RANGE + 2];//程序的输入矩阵，以字符“@# ”表示探索状态
}MazeType;
//栈类型
typedef struct {
	int step;//当前位置在路径上的“序号”
	PostType seat;//当前位置坐标
	DirectiveType di;//往下一坐标位置的方向
}ElemType;//栈元素类型
typedef struct {
	ElemType* base;
	ElemType* top;
	int stacksize;
}Stack;//利用顺序栈实现
Status InitStack(Stack& S) {
	S.base = new ElemType[100];
	if (!S.base) exit(OVER_FLOW);
	S.top = S.base;
	S.stacksize = 100;
	return OK;
}
Status StackEmpty(Stack S) {
	if (S.top == S.base) return OK;
	else return ERROR;
}
Status Push(Stack& S, ElemType e) {
	if (S.top - S.base >= S.stacksize) {
		ElemType* newbase = (ElemType*)realloc(S.base, (S.stacksize + 10) * sizeof(ElemType));
		if (!newbase) exit(OVER_FLOW);
		S.base = newbase;
		S.top = S.base + S.stacksize;
		S.stacksize += 10;
	}
	*S.top++ = e;
	return OK;
}
Status Pop(Stack& S, ElemType& e) {
	if (S.top == S.base) return ERROR;
	e = *--S.top;
	return OK;
}
void InitMaze(MazeType& maze) {
	for (int i = 0; i < row + 2; i++) {
		for (int j = 0; j < col + 2; j++) {
			if (i == 0 || j == 0 || i == row + 1 || j == col + 1) {
				maze.map[i][j] = 0;
				maze.arr[i][j] = '#';
			}
			else {
				cout << "第" << i << "行" << "第" << j << "列：";
				cin >> maze.map[i][j];
				if (maze.map[i][j] == 1) maze.arr[i][j] = '#';
				else maze.arr[i][j] = ' ';
			}
		}
	}
}
Status Pass(MazeType maze, PostType curpos) {//判断格子是否走过且能走
	if (maze.arr[curpos.x][curpos.y] == ' ') return OK;
	return ERROR;
}
Status FootPrint(MazeType& maze, PostType curpos) {//记录走过的格子
	if (maze.arr[curpos.x][curpos.y] == ' ') {
		maze.arr[curpos.x][curpos.y] = '*';
		return OK;
	}
	return ERROR;
}
Status Same(PostType curpos, PostType end) {//判断两个格子位置是否一样
	if (curpos.x == end.x && curpos.y == end.y)
		return OK;
	return ERROR;
}
PostType NextPos(PostType curpos, int di) {
	switch (di)
	{
	case 1: {curpos.y = curpos.y + 1; return curpos; break; }//向右移动一格
	case 2: {curpos.x = curpos.x + 1; return curpos; break; }//向下移动一格
	case 3: {curpos.y = curpos.y - 1; return curpos; break; }//向左移动一格
	case 4: {curpos.x = curpos.x - 1; return curpos; break; }//向上移动一格
	}
}

Status MarkPrint(MazeType& maze, PostType pos) {
	maze.arr[pos.x][pos.y] = '@';
	return OK;
}
Status MazePath(MazeType& maze, PostType start, PostType end) {
	Stack S; InitStack(S);
	PostType curpos = start;//设定“当前位置”为“入口位置”
	int curstep = 1;//探索第一步,记录探索步数
	bool found = ERROR;//判断是否达到终点
	ElemType e;//Stack元素
	do {
		if (Pass(maze, curpos)) {
			//当前位置可以通过，即是未曾走到过的通道留下足迹
			FootPrint(maze, curpos);
			e.di = 1; e.seat = curpos; e.step = curstep;
			Push(S, e);//加入路径
			if (Same(curpos, end))  found = OK; //到达终点（出口）
			else {
				curpos = NextPos(curpos, 1);//下一位置是当前位置的东邻（向右移动一格）
				//NextPos(curpos, 1);
				curstep++;//探索下一步
			}
		}
		else {//当前位置不能通过
			if (!StackEmpty(S)) {
				Pop(S, e);//将刚才不能前进的Stack元素出栈，相当于退回一步
				while (e.di == 4 && !StackEmpty(S)) {
					MarkPrint(maze, e.seat);//对刚才出栈的元素的位置标记
					Pop(S, e);//下一个元素出栈，与上一步的e不一样
				}
				if (e.di < 4) {
					e.di++; Push(S, e);//换下一个方向探索
					curpos = NextPos(e.seat, e.di);//设定当前位置是该新方向向上
				}
			}
		}
	} while (!StackEmpty(S) && !found);
	return found;
}

Status PrintMaze(MazeType maze) {
	for (int i = 0; i < RANGE + 2; i++) {
		for (int j = 0; j < RANGE + 2; j++) {
			cout << maze.arr[i][j] << "  ";
		}
		cout << "\n";
	}
	return OK;
}

int main() {
	MazeType maze;
	InitMaze(maze);
	cout << "生成的迷宫为：" << endl;
	PrintMaze(maze);
	PostType start, end;
	cout << "请输入起点：";  cin >> start.x >> start.y;
	cout << "请输入终点：";  cin >> end.x >> end.y;
	if (MazePath(maze, start, end)) {
		cout << "路径存在，迷宫求解路径为：" << endl;
		PrintMaze(maze);
	}
	else {
		cout << "路径不存在，迷宫求解路径为：" << endl;
		PrintMaze(maze);
	}
	return 0;
}
```

### 回溯法—链栈

只有结构体定义与顺序栈不同，其他程序均相同

```c++
typedef int Status;
typedef int DirectiveType;
//位置坐标
typedef struct {
	int x, y;//表示迷宫中的位置信息x行y列
}PostType;
//迷宫类型
typedef struct {
	int map[row+2][col+2];//用户输入矩阵（0，1）表示迷宫的初始生成
	char arr[RANGE+2][RANGE+2];//程序的输入矩阵，以字符“@# ”表示探索状态
}MazeType;
//栈类型
typedef struct {
	int step;//当前位置在路径上的“序号”
	PostType seat;//当前位置坐标
	DirectiveType di;//往下一坐标位置的方向
}ElemType;//栈元素类型
typedef struct StackNode{
	ElemType data;
	struct StackNode * next;
}StackNode, * LinkStack;//节点类型，指针类型

### 深度优先搜索遍历法
```C++
//显示中文
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8
#include<stdlib.h>
#include<stdio.h>
#include<iostream>
#define M 4
#define N 4
#define MaxSize M * N

#define OK 1
#define ERROR 0
#define INFEASIBLE -1
#define OVER_FLOW -2
using namespace std;

typedef int Status;

typedef struct {
	int map[M + 2][N + 2];//用户输入矩阵（0，1）表示迷宫的初始生成
}MazeType;

//初始化迷宫
void InitMaze(MazeType&maze) {
	for (int i = 0; i < M + 2; i++) {
		for (int j = 0; j < N + 2; j++) {
			if (i == 0 || j == 0 || i == M + 1 || j == N + 1) {
				maze.map[i][j] = 1;
			}
			else {
				cout << "第" << i << "行" << "第" << j << "列：";
				cin >> maze.map[i][j];
			}
		}
	}
}
//x与y的增量
typedef struct {
	int incX,incY;    
}Direction;

//位置
typedef struct{
	int x,y;     //当前坐标
	int di;     //当前方向
}Box;

//栈
typedef struct 
{
	Box data[MaxSize];
	int top; 
}SqStack;

//初始化栈
void InitStack(SqStack &S)
{
	S=*(SqStack *)malloc(sizeof(SqStack));
	S.top=0;
}

//四个方向增量初始化
void Init(Direction direct[])
{
	//右
	direct[0].incX = 0;
	direct[0].incY = 1;
	//下
	direct[1].incX = 1;
	direct[1].incY = 0;
	//左
	direct[2].incX = 0;
	direct[2].incY = -1;
	//上
	direct[3].incX = -1;
	direct[3].incY = 0;
}

//判栈空
Status StackEmpty(SqStack &S)
{
	if(S.top==0)
	{
		return OK;//栈空返回真 
	}
	else
	{
		return ERROR;
	}
}

//入栈
void Push(SqStack &S,Box e)
{
	if(S.top==MaxSize)
	{
		printf("入栈失败！\n");	
	}
	S.data[S.top]=e;
	S.top++;
}

//出栈
Box Pop(SqStack &S)
{
	Box e;
	S.top--;
	e = S.data[S.top];
	return e;
}

//销毁
void Destroy(SqStack &S)
{
	free(&S);
	cout<<"栈已销毁！"<<endl;
}

//寻找路径
Status findPath(int maze[][N + 2],Direction direct[], SqStack &S)
{
	Box temp;
	int x, y, di;
	int L, C;
	maze[1][1] = -1;
	temp.x = 1; 
	temp.y = 1; 
	temp.di = -1;
	Push(S, temp);//将temp压入到堆栈
	while (!StackEmpty(S))//当栈不空的时候
	{
		temp = Pop(S);
		x = temp.x; y = temp.y; di = temp.di + 1;
		while (di < 4)
		{
			L = x + direct[di].incX;
			C = y + direct[di].incY;
			if (maze[L][C] == 0)
			{
				temp.x = x; 
				temp.y = y; 
				temp.di = di;
				Push(S, temp);    
				x = L;
				y = C;
				maze[L][C] = -1; 
				if (x == M && y == N)
				{
					return OK;
				}
				else
				{
					di = 0;
				}
			}
			else
			{
				di++;
			}
		}
	}
	return ERROR;
}

//打印创建的迷宫
Status PrintMaze(MazeType maze) {
	for (int i = 0; i < M + 2; i++) {
		for (int j = 0; j < N + 2; j++) {
			cout << maze.map[i][j] << "  ";
		}
		cout << "\n";
	}
	return OK;
}
//画路径
Status printPath(MazeType maze,Box path[], int count)
{
	//描绘路径
	for(int i = count-1; i >= 0; i--)
	{
		int x = path[i].x;
		int y = path[i].y;
		maze.map[x][y] = 9;
	}
	maze.map[M][N] = 9;
	PrintMaze(maze);
	return OK;
}
int main()
{ 
    //显示中文
    SetConsoleOutputCP(65001);

	cout << "迷宫只能从左上角开始走到右下角" << endl;
    MazeType MAZE;
	InitMaze(MAZE);
	MazeType* result = new MazeType(MAZE);
	cout << "生成的迷宫为：" << endl;
	PrintMaze(MAZE);

	cout<<endl;
	cout << "找到通路的迷宫为(以9表示找到的路径)：" << endl;
	Direction direct[4];//初始化方向结构体 
	Init(direct);//初始化方向结构体 
	SqStack S;
	InitStack(S);
	StackEmpty(S);
	Box path[MaxSize];
	if(findPath(MAZE.map, direct, S))
	{
		int count = 0;
		for(int i = 0; !StackEmpty(S); i++)
		{
			path[i] = Pop(S);
			count++;
		}
		printPath(*result,path,count);
		Destroy(S);
	}
	else
	{
		cout<<"迷宫没有通路"<<endl;
	}
	system("pause");
	return 0;
}
```

### 广度优先搜索遍历法

#### 解法一
```C++
#include <iostream>
using namespace std;
#define nRow 100
#define nCol 100
struct note  // 队列中的元素类型 
{
	int x;
	int y;
};
// 地图 
int ROW_NUM, COL_NUM;
int nMatrix[nRow][nCol] = { 0 };
int flag[100][100] = { 0 };  // 标记是否走过的路
int head = 0;   // 队列的头部索引 
int tail = 0;   // 队列的尾部索引
// 方向 
int direction[4][2] = {
	  0, 1,
	  1, 0,
	  0, -1,
	  -1, 0
};

struct note que[10000];	// 队列

int main()
{
	for (int i = 0; i < 100; i++)
		for (int j = 0; j < 100; j++)
			flag[i][j] = 1;

	cout << "你想要构造迷宫的长度为:" << endl;
	cin >> ROW_NUM;
	cout << "你想要构造迷宫的宽度为：" << endl;
	cin >> COL_NUM;
	cout << "以0为通路，以非零实数，如1为墙壁，构造你想要的迷宫：" << endl;
	int m = 1;
	for (int i = 0; i < ROW_NUM; i++)
	{
		cout << "请输入第" << m << "行的数字" << endl;
		for (int j = 0; j < COL_NUM; j++)
		{
			cin >> nMatrix[i][j];
			flag[i][j] = 0;
		}
		if (m < ROW_NUM)
		{
			m++;
		}
	}
	cout << "你构造的迷宫为：" << endl;
	for (int i = 0; i < ROW_NUM; i++)
	{
		for (int j = 0; j < COL_NUM; j++)
		{
			if (nMatrix[i][j] == 0)
				cout << "   ";
			else cout << "[ ]";
		}
		cout << endl;
	}// 输出二维迷宫图
	// 初始化每一个队列元素的值（开始寻找的地方）
	que[head].x = que[head].y = 0;
	tail++;  // 尾部索引 + 1
	flag[0][0] = 1;  // 标记初始位置已经查找过了
	cout << "它走过的路径为：" << endl;
	cout << "(0,0)" << endl;
	while (head < tail)
	{
		int i = 0;
		
		for (; i < ROW_NUM; i++)
		{
			// 每次都是以队头为单位向四个方向开始
			int nx = que[head].x + direction[i][0];
			int ny = que[head].y + direction[i][1];

			if (nx < 0 || nx > ROW_NUM || ny < 0 || ny > COL_NUM)
				continue;
			
			if (nMatrix[nx][ny] == 0 && flag[nx][ny] == 0)
			{
				flag[nx][ny] = 1;
				que[tail].x = nx;     // 加入队列 
				que[tail].y = ny;
				cout << "("<<que[tail].x <<","<< que[tail].y<<")" << endl;
				tail++;
			}
		}
		head++;
	}
	return 0;
}
```

#### 解法二
```C++
#include <thread>//多线程头文件
#include<Windows.h>//用于窗口等待Sleep
#include <iostream>
using namespace std;
#define WAIT_TIME 1000//加载时间，越小越快
string* maze = NULL;//输入的迷宫
int maze_height = 0;//迷宫高度
int flag = 0;//结束标志
int aim_x = 0, aim_y = 0;//终点坐标
int** maze_road;//迷宫数组
int road_num = 0, road_num_flag = -1;//创建寻路线程次数，寻路线程次数标记
void printMap();
void findRoad(int x, int y, int direction);
void continuePrintMap();
int main()
{
/*输入数据*/
cout << "迷宫高为：";
cin >> maze_height;
cout << "请输入迷宫（墙壁为#）：" << endl;
maze = new string[maze_height];
for (int i = 0; i < maze_height; i++)
{
cin >> maze[i];
}
cout << "请输入迷宫终点(x,y)：" << endl;
cin >> aim_x >> aim_y;
/*构造迷宫数组*/
maze_road = new int* [maze_height];
if (maze_road)
memset(maze_road, 0, sizeof(int*) * maze_height);
for (int i = 0; i < maze_height; i++)
{
maze_road[i] = new int[maze[i].size() + 1];
for (unsigned int j = 0; j < maze[i].size() + 1; j++)
{
if (maze[i][j] != &apos;#&apos;)
maze_road[i][j] = 0;
else
maze_road[i][j] = -1;
}
}
/*打开多线程*/
system("cls");//清屏
thread print_map(continuePrintMap);
print_map.detach();
thread find_road(findRoad, 1, 1, 0);
find_road.detach();
/*后续*/
while (1)
{
/*没找到路径不继续*/
if (flag == 1)
break;
}
system("cls");
printMap();//最终迷宫图
/*收尾删除*/
for (int i = 0; i < maze_height; i++)
{
delete[] maze_road[i];
}
if (maze_road)
delete[] maze_road;
maze_road = NULL;
delete[] maze;
maze = NULL;
return 0;
}
/*多线程持续打印迷宫*/
void continuePrintMap()
{
while (1)
{
/*不再找了也停止（机器人全灭）*/
if (road_num_flag == road_num)
{
flag = 1;
return;
}
/*找到位置就停止*/
if (flag == 1)
return;
printMap();
}

}
/*打印迷宫*/
void printMap()
{
/*光标移动到（0，0），不用cls因为会闪*/
road_num_flag = road_num;
printf_s("\33[0;0H");
for (int i = 0; i < maze_height; i++)
{
for (unsigned int j = 0; j < maze[i].size(); j++)
{
if (maze_road[i][j] == -1)
printf_s("%3c", maze[i][j]);//打印墙
else
printf_s("%3d", maze_road[i][j]);//打印路
//if
//(maze_road[i][j] == -2);
//printf_s("Y");
}
cout << endl;
}
Sleep(WAIT_TIME);
}
/*
多线程迷宫寻路机器人
x 当前x坐标
y 当前y坐标
direction:
0 没有前一个位置
1 前一个位置在↑
2 前一个位置在↓
3 前一个位置在←
4 前一个位置在→
*/
void findRoad(int x, int y, int direction)
{
Sleep(WAIT_TIME);
road_num++;
/**/
if (flag == 1)//寻路完成提前退出
return;
/*记录此地距离原点距离*/
switch (direction)
{
case 1: {
maze_road[x][y] = maze_road[x - 1][y] + 1;
break;
}
case 2: {
maze_road[x][y] = maze_road[x + 1][y] + 1;
break;
}
case 3: {
maze_road[x][y] = maze_road[x][y - 1] + 1;
break;
}
case 4: {
maze_road[x][y] = maze_road[x][y + 1] + 1;
break;
}
default:
break;
}
/*寻路成功，标记并退出*/
if (x == aim_x && y == aim_y)
{
flag = 1;
return;
}
/*向四个方向，有路则释放新的机器人（创建新线程）*/
if (maze_road[x - 1][y] == 0)
{
thread find_road_up(findRoad, x - 1, y, 2);
find_road_up.detach();
}
if (maze_road[x + 1][y] == 0)
{
thread find_road_down(findRoad, x + 1, y, 1);
find_road_down.detach();
}
if (maze_road[x][y - 1] == 0)
{
thread find_road_left(findRoad, x, y - 1, 4);
find_road_left.detach();
}
if (maze_road[x][y + 1] == 0)
{
thread find_road_right(findRoad, x, y + 1, 3);
find_road_right.detach();
}
/*分裂成新的机器人后，本机器人销毁*/
Sleep(WAIT_TIME * 2);
return;
}
```

**完整文档详见：[博客相关资源-数据结构课设文件](https://www.spiritlhl.top/ziyuan/)**



