---
layout: post_for_cs
title:  "Create Binary Tree"
date:   2019-01-24
excerpt: "how to create a binary tree through inorder, preorder, postorder and layerorder..."
image: "/images/8-2.jpg"
comments: true
---


## Bing
前几十分钟亲爱的必应还不可访问，就在刚刚，习惯性的在Google页面地址栏敲上"b"自动出来"bing.com"（对其爱的深沉啊），竟然可以访问了！！！之前还以为它也被打入了冷宫，真是很伤心了，不说其多好多不好，单单清爽的界面就足以使我一直使用它，且不需要梯子。God bless you。<br />
言归正传，今天来谈谈那高耸入云、粗壮有力的大树吧。。且先不谈森林之茂密神秘，单就一颗树，就足以让人头疼。<br />

## Binary Tree

二叉树，顾名思义，由根——左子树——右子树递归的组成。根的两个分支构成其孩子，当然，根就是他们的爹了。<br />

相关概念：<br />
- 结点的度(Degree):结点拥有子树数
- 叶子(Leaf)或终端结点:度为0
- 分支结点或终端结点:度不为0
- 树的度:树内各结点的度的最大值
- 孩子(Child):结点的子树的根
- 双亲(Parent):该结点成为孩子双亲
- 兄弟(Sibling):同一个双亲的孩子之间互称
- 堂兄弟:双亲在同一层的结点
- 深度(Depth):从根结点(深度为1)开始自顶向下逐层累加至该结点时的深度值
- 高度(Height):从最底层叶子节点(高度为1)开始自底向上逐层累加至该结点时的高度之。
- 森林(Forest):是m(m>=0)棵互不相交的树的集合

**二叉树**(Binary Tree):每个结点至多有两棵子树，且二叉树的子树有左右之分，其次序不能任意颠倒。

- 性质1:在二叉树的第i层上至多有2^(i-1)个结点(i>=1)
- 性质2:深度为k的二叉树至多有2^k - 1个结点。(k>=1)
- 性质3:对任何一棵二叉树T,如果其终端结点数为N0,度为2的结点数为N2,则 N0 = N2 + 1

**满二叉树**:一棵深度为K且有2^k - 1个结点的二叉树。特点:每一层上的结点数都是最大结点数。<br />

**完全二叉树**:除了最下面亦称之外，其余层的结点个数都达到了党曾能达到的最大结点数，且最下面一层之从左至右连续存在若干节点，而这些连续节点右边的结点全部都不存在。<br />

## 二叉树的遍历

二叉树在遍历过程中可分为两种方案：<br />

- 深度优先搜索(DFS)
- 广度优先搜索(BFS)

这两种实现方式顾名思义，一个是优先向"深"挖掘，一个是一层一层的挖掘。<br />

- 先序遍历(preorder):   根—>左子树—>右子树
- 中序遍历(inorder):  左子树—>根—>右子树
- 后序遍历(postorder):  左子树—>右子树—>根
- 层序遍历(layerorder):  自根至叶结点，逐层遍历

可以看出，前三种的"先、种和后"的由来由"根"的访问时机而定。且前三种采用DFS实现，层序遍历采用BFS。<br />

## 亲手种一棵树

遍历的前提是种一棵树，那么该怎么种一棵树呢？<br />

上面各种的遍历为我们提供了足够的信息，如图所示，以先序的遍历序列和中序的遍历序列为例：<br />
![1](http://www.auroretech.com/images/8-1.png)<br />
中序的遍历为我们提供了必要的信息! 由图上可知，中序将结点一分为两部分：左子树，右子树且为我们提供了一个关键信息k。由此可得到num = k -inL 即左子树的结点数，由此为下一个递归提供了参数。<br />

上关键代码：<br />
![2](http://www.auroretech.com/images/8-1.jpg)<br />
其中pre[]、in[]为存储着树的先序遍历结点值和中序遍历结点值。为了寻找到关键信息k，我们从先序遍历的特点来看，其第一个结点PreL即是根结点值。故在中序中遍历直至中序数组in[]中和此值相等的数据出现，其数组下标其为k值。<br />

得到k值后左子树结点个数numLeft便得知。由此，现在的根结点的左子树作为第一层递归的"根结点"，附加新的PreL, PreR, inL, inR。即完成递归所需的全部信息。右子树同理.。<br />

最后，一个由根结点root链接的二叉树便生成了。返回值node* 即我们所要的指向根结点的指针。<br />

对于其它情况，如中序分别和后序，层序序列均可通过此方法分析。递归实现二叉树的建立。<br />

## 最后

搁置Python爬虫的内容很久了，找机会补上吧。<br />
附完成代码：
``` C++
#include <iostrem>
#include <queue>

using namespace std;

int maxn = 50;
int pre[maxn], in[maxn], post[maxn];
typedef int data_t;

struct node
{
	data_t data;
	node* lchild;
	node* rchild;
};

//二叉树先序遍历
int pre_order(node* &root)
{
	if (root == nullptr)return 0;
	cout << root->data;
	pre_order(root->lchild);
	pre_order(root->rchild);
	return 0;
}

//二叉树中序遍历
int in_order(node* &root)
{
	if (root == nullptr)return 0;
	in_order(root->lchild);
	cout << root->data;
	in_order(root->rchild);
	return 0;
}

int post_order(node* &root)
{
	if (root == nullptr)return 0;
	in_order(root->lchild);
	in_order(root->rchild);
	cout << root->data;
	return 0;
}

int layer_order(node* &root)
{//使用队列实现类似BFS搜索的层序遍历
	queue<node*> q; //队列存地址
	q.push(root); //根结点入队
	while (!q.empty())
	{
		node* now = q.front(); //取出队首元素
		q.pop(); //出队
		cout << now->data;
		if (now->lchild != nullptr)q.push(now->lchild);
		if (now->rchild != nullptr)q.push(now->rchild);
	}
	return 0;
}

node* createbt(int PreL, int PreR, int inL, int inR)
{
	if (PreL > PreR)return nullptr;
	node* root = new node;//创建二叉树根结点
	root->data = pre[PreL]; //根结点值域
	int k;
	for (k = inL; k <= inR; k++)
		if (in[k] == pre[PreL])break;//寻找中序中的根，以便区分左子树和右子树
	int numLeft = k - inL; //左子树的结点个数
	//返回左子树根结点，递归。
	root->lchild = createbt(PreL + 1, PreL + numLeft, inL, k - 1);
	//返回右子树根结点，递归。
	root->rchild = createbt(PreL + numLeft + 1, PreR, k + 1, inR);
	return root;
}

int main()
{
	int n; //结点个数
	cout << "输入结点数:" << endl;
	cin >> n;
	cout << "输入先序序列:" << endl;
	for (int i = 0; i < n; i++)
		cin >> pre[i];
	cout << "输入中序序列:" << endl;
	for (int i = 0; i < n; i++)
		cin >> in[i];
	node* root = createbt(0, n - 1, 0, n - 1);
	layer_order(root);
	system("pause");
	return 0;
}
```