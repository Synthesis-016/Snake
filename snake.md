# Snake
## 实习任务总结&项目介绍
### 项目名称：贪吃蛇
### 项目简介：用C++及EasyX实现贪吃蛇小游戏并尽可能完善
### 已完成功能：界面UI，基础游戏运行
### 未完成功能：AI，360°转向，地图设计，存档及排行榜，粒子效果
---
说在开头：作为学习编程以来制作的第一个真正意义上的项目，虽然只是个简单的小游戏，但确实是加深了对代码的理解。遗憾的是由于个人知识能力的限制，关于存档等功能代码的编写失败，耗费了大量时间也导致未能开始对AI的挑战（尽管最开始就有了设想）。最终得到的便是这较简陋的近1k行代码。


### 蛇的实现
作为小游戏的关键，在学习之后，我成功的使用链表实现了小蛇，代码如下
``` C++script
typedef struct NODE
{	int x;	
    int y;
    struct NODE* lastnode;
    struct NODE* nextnode;
}//节点结构
void setnode()
{
	newptr = new Node;
	newptr->x = ptr->x;
	newptr->y = ptr->y;
	newptr->lastnode = NULL;
	newptr->nextnode = NULL;
	ptr->nextnode = newptr;
	newptr->lastnode = ptr;
	ptr = newptr;
}
void Snakestartup(int X, int Y)
{
	newptr = new Node;
	newptr->x = X;
	newptr->y = Y;
	newptr->lastnode = NULL;
	newptr->nextnode = NULL;
	headptr = ptr = temp = newptr;
	for (int i = 0; i < 6; i++)
	{
		setnode();
		snakemsg.length++;
	}
}
```
这样就用链表创建了一条小蛇。相比用数组储存数据，链表的处理明显更为灵活，极大便利了之后对于蛇的操作。

参照贪吃蛇大作战，我使用实心圆绘制小蛇，看起来没那么呆板，同时加入简单的数值计算实现渐变色，代码如下（大略）：
```C++script
void drawsnake()
{	int x, y;
	double n = 0;
	temp = ptr;
	while(temp!=headptr)	
	{	x = temp->x;
		y = temp->y;		
		temp = temp->lastnode;
		setfillcolor();
		solidcircle();
	}	
	x = headptr->x;	
	y = headptr->y;
	setfillcolor();
	solidcircle();
	temp = NULL;
}
```
用方向键实现360°转向目前我仅有一种设想：
鉴于蛇的移动都是用像素点刻画，只能按固定方向移动，若将蛇的方向不断进行微调，就能使蛇头在前进中画出近似圆。以蛇头圆心为中心做点阵，则
3×3点阵可以刻画8个方向
5×5点阵可以刻画16个方向..........
以此类推，点阵越大就能画出更好的近似圆。然而大点阵下的方向键控制代码繁杂，故我只能写出8个方向的伪360°


### 食物等的实现
以食物为例，代码如下
```C++script
void setfood(int n)
{
    Food.r = 5;	
    for(int i = 0; i<n;)	
    {	
        Food.x = rand() % 800;
        Food.y = rand() % 600;		
        if (getpixel(Food.x, Food.y) != wallcolor && getpixel(Food.x, Food.y) !=  bombcolor &&getpixel(Food.x, Food.y) != foodcolor && getpixel(Food.x, Food.y) !=  pgrasscolor)		
        {			
            if (Food.x < gap || Food.y < gap)			
            {			
                Food.x += gap;
                Food.y += gap;	
            }			
            if (Food.x > 800 - gap || Food.y > 600 - gap)			
            {				
               Food.x -= gap;				
               Food.y -= gap;			
            }			
            foodmsg[i].x = Food.x;			
            foodmsg[i].y = Food.y;			
            i++;		
            }	
    }
}
void drawfood(int n)
{	
    for(int i = 0; i<n; i++)	
    {		
         setfillcolor(foodcolor);		
         solidcircle(foodmsg[i].x, 
         foodmsg[i].y, Food.r);
    }
}
```
一次生成多个食物，并用二维数组记录了食物坐标，因为二维数组更适合吃一补一的运作方式
用两点间距离判断是否吃到东西

### UI制作
UI界面本打算使用素材包，但在机房写代码时不知是什么原因无法加载图片，反复尝试无果后只能自己用文字输出制作UI，结果就是界面单调，代码还死长，就不贴了（1k行的来源）


### 后记
本次实习任务切实让我体会到了投入的快乐，虽然最终成果未达预期，也是一次宝贵的实践经验。同时也更让我明白自己的不足，稍微了解了游戏背后的汗水，以及带一个自己的电脑的重要性（笑）

以上
