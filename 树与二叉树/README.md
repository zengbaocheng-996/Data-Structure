|      |            | Description                                                  |
| ---- | ---------- | ------------------------------------------------------------ |
| 5    | 树与二叉树 | 树的基本概念<br />二叉树的概念<br />二叉树的遍历和线索二叉树<br />树、森林<br />树与二叉树的应用 |

5. 已知一棵二叉树按顺序存储结构进行存储，设计一个算法，求编号分别为i和j的两个结点的最近的公共祖先结点的值。

   首先，必须明确二叉树中任意两个结点必然存在最近的公共祖先结点，最坏的情况下是根结点（两个结点分别在根结点的左右分支中），而且从最近的公共祖先结点到根结点的全部祖先结点都是公共的。由二叉树顺序存储的性质可知，任意一个结点i的双亲结点的编号为i/2。求解i和j最近公共祖先结点的算法步骤如下（设从数组下标1开始存储）：

   - 若i>j，则结点i所在层次大于或等于结点j所在层次。结点i的双亲结点为结点i/2，若i/2=j，则结点i/2是原结点i和结点j的最近公共祖先结点，若i/2!=j，则令i=i/2，即以该结点i的双亲结点为起点，采用递归的方法继续查找。
   - 若j>i，则结点j所在层次大于或等于结点i所在层次，结点j的双亲结点为结点j/2，若j/2=i，则结点j/2是原结点i和结点j的最近公共祖先结点，若j/2!=i，则令j=j/2。

   重复上述过程，直到找到它们最近的公共祖先结点为止。

   ```cpp
   ElemType Comm_Ancestor(SqTree T, int i, int j)
   {
       //本算法在二叉树中查找结点i和结点j的最近公共祖先结点
       if(T[i]!='#' && T[j]!='#')//结点存在
       {
           while(i!=j)//两个编号不同时循环
           {
               if(i>j)
                   i=i/2;//向上找i的祖先
               else
                   j=j/2;//向上找j的祖先
           }
       	return T[i];
       }
   }
   ```


3. 编写后序遍历二叉树的非递归算法。

   后序非递归遍历二叉树先访问左子树、再访问右子树，最后访问根结点。

   第一步：沿着根的左孩子，依次入栈，直到左孩子为空。此时栈内元素为ABD。

   第二部：读栈顶元素：若其右孩子不空且未被访问过，将右子树转执行第一步；否则栈顶元素出栈并访问。

   栈顶D的右孩子为空，出栈并访问，它是后序序列的第一个结点；栈顶B的右孩子不空且未被访问过，E入栈，栈顶E的左右孩子均为空，出栈并访问；栈顶B的右孩子不空但已被访问，B出栈并访问；栈顶A的右孩子不空且未被访问过，C入栈，栈顶C的左右孩子均为空，出栈并访问；栈顶A的右孩子不空但已被访问，A出栈并访问。由此得到后序序列DEBCA。

   在上述思想的第二步中，必须分清返回时是从左子树返回的还是从右子树返回的，因此设定一个辅助指针r，用于指向最近访问过的结点。也可在结点中增加一个标志域，记录是否已被访问。

   ```cpp
   void PostOrder(BiTree T)
   {
       InitStack(S);
       BiTNode *p=T;
       BiTNode *r=NULL;
       while(p||!IsEmpty(S))
       {
           if(p)//走到最左边
           {
               push(S,p);
               p=p->lchild;
           }
           else//向右
           {
               GetTop(S,p);//读栈顶结点（非出栈）
               if(p->rchild&&p->rchild!=r)//若右子树存在，且未被访问过
                   p->rchild;//转向右
               else//否则，弹出结点并访问
               {
                   pop(S,p);//将结点弹出
                   visit(p->data);//访问该结点
                   r=p;//记录最近访问过的结点
                   p=NULL;//结点访问完后，重置p指针
               }
           }
       }
   }
   ```

4. 试给出二叉树的自下而上、从右到左的层次遍历算法。

   一般的二叉树层次遍历是自上而下、从左到右，这里的遍历顺序恰好相反。算法思想：利用原有的层次遍历算法，出队的同时将各结点指针入栈，在所有结点入栈后再从栈顶开始依次访问即为所求的算法。具体实现如下：

   第一步：把根结点入列。

   第二步：把一个元素出队列，遍历这个元素。

   第三步：依次把这个元素的左孩子、右孩子入队列。

   第四步：若队列不同，则跳到第二步，否则结束。

   ```cpp
   void InvertLevel(BiTree bt)
   {
       Stack s; Queue Q;
       if(bt!=NULL)
       {
           InitStack(s);//栈初始化，栈中存放二叉树结点的指针
           InitQueue(Q);//队列初始化，队列中存放二叉树结点的指针
           EnQueue(Q,bt);
           while(IsEmpty(Q)==false)//从上而下层次遍历
           {
               DeQueue(Q,p);
               Push(s,P);//出队、入栈
               if(p->lchild)
                   EnQueue(Q,p->lchild);//若左子女不空，则入队列
               if(p->rchild)
                   EnQueue(Q,p->rchild);//若右子女不空，则入队列
           }
           while(IsEmpty(s)==false)
           {
               Pop(s,p);
               visit(p->data);
           }//自下而上、从右到左的层次遍历
       }
   }
   ```

5. 假设二叉树采用二叉链表存储结构，设计一个非递归算法求二叉树的高度。

   采用层次遍历的算法，设置变量level记录当前结点所在的层数，设置变量last指向当前层的最右结点，每次层次遍历出队时与last指针比较，若两者相等，则层数加1，并让last指向下一层的最右结点，直到遍历完成。level的值即为二叉树的高度。

   ```cpp
   int Btdepth(BiTree T)
   {
   	//采用层次遍历的非递归方法求解二叉树的高度
       if(!T)
           return 0;//树空，高度为0
       int front=-1,rear=-1;
       int last=0,level=0;//last指向当前层的最右结点
       BiTree Q[MaxSize];//设置队列Q，元素是二叉树结点指针且容量足够
       Q[++rear]=T;//将根结点入队
       BiTree p;
       while(front<rear)//队不空，则循环
       {
           p=Q[++front];//队列元素出队，即正在访问的结点
           if(p->lchild)
               Q[++rear]=p->lchild;//左孩子入队
   		if(p->rchild)
               Q[++rear]=p->rchild;//右孩子入队
           if(front==last)//处理该层的最右结点
           {
               level++;//层数增1
               last=rear;//last指向下层
           }
       }
       return level;
   }
   ```

   求某层的结点个数、每层的结点个数、树的最大宽度等，都可采用上述思想。也可使用递归算法。

   ```cpp
   int Btdepth2(BiTree T)
   {
       if(T==NULL)
           return 0;//空树，高度为0
       ldep=Btdepth2(T->lchild);//左子树高度
       rdep=Btdepth2(T->rchild);//右子树高度
       if(ldep>rdep)
           return ldep+1;//树的高度为子树最大高度加根结点
       else
           return rdep+1;
   }
   ```

6. 二叉树按二叉链表形式存储，试编写一个判别给定二叉树是否是完全二叉树的算法。

   根据完全二叉树的定义，具有n个结点的完全二叉树与满二叉树中编号从1~n的结点一一对应。算法思想：采用层次遍历算法，将所有结点加入队列（包括空结点）。遇到空结点时，查看其后是否有非空结点。若有，则二叉树不是完全二叉树。

   ```cpp
   bool IsComplete(BiTree T)
   {
       //本算法判断给定二叉树是否为完全二叉树
       InitQueue(Q);
       if(!T)
       	return true;//空树为满二叉树
       EnQueue(Q,T);
       while(!IsEmpty(Q))
       {
           DeQueue(Q,p);
           if(p)//结点非空，将其左、右子树入队列
           {
               EnQueue(Q,p->lchild);
               EnQueue(Q,p->rchild);
           }
           else//结点为空，检查其后是否有非空结点
           	while(!IsEmpty(Q))
               {
                   DeQueue(Q,p);
                   if(p)//结点非空，则二叉树为非完全二叉树
               		return false;
               }
       }
       return true;
   }
   ```

7. 假设二叉树采用二叉链表存储结构存储，试设计一个算法，计算一棵给定二叉树的所有双分支结点个数。

   计算一棵二叉树b中所有双分支结点个数的递归模型f(b)如下：

   |                                  |                                    |
   | -------------------------------- | ---------------------------------- |
   | f(b=0)                           | 若b=NULL                           |
   | f(b)=f(b->lchild)+f(b->rchild)+1 | 若*b为双分支结点                   |
   | f(b)=f(b->lchild)+f(b->rchild)   | 其他情况（*b为单分支结点或叶结点） |

   ```cpp
   int DsonNodes(BiTree b)
   {
       if(b=NULL)
           return 0;
       else if(b->lchild!=NULL&&b->rchild!=NULL)//双分支结点
           return DsonNodes(b->lchild)+DsonNodes(b->rchild)+1;
       else
           return DsonNodes(b->lchild)+DsonNodes(b->rchild);
   }
   ```

   也可以设置一个全局变量Num，每遍历到一个结点时，判断每个结点是否为分支结点（左、右结点都不为空，注意是双分支），若是则Num++。

8. 设树B是一棵采用链式结构存储的二叉树，编写一个把树B中所有结点的左、右子树进行交换的函数。

   采用递归算法实现交换二叉树的左、右子树，首先交换b结点的左孩子的左、右子树，然后交换b结点的右孩子的左、右子树，最后交换b结点的左、右孩子，当结点为空时递归结束（后序遍历思想）。

   ```cpp
   void swap(BiTree b)
   {
   	//本算法递归地交换二叉树的左、右子树
       if(b)
       {
           swap(b->lchild);//递归地交换左子树
           swap(b->rchild);//递归地交换右子树
           temp=b->lchild;//交换左、右孩子结点
           b->lchild=b->rchild;
           b->rchild=temp;
       }
   }
   ```

9. 假设二叉树采用二叉链存储结构存储，设计一个算法，求先序遍历序列中第k（1<=k<=二叉树中结点个数）个结点的值。

   设置一个全局变量i（初值为1）来表示进行先序遍历时，当前访问的是第几个结点。然后可以借用先序遍历的代码模型，先序遍历二叉树。当二叉树b为空时，返回特殊字符'#'；当k==i时，该结点即为要找的结点，返回b->data；当k!=i时，递归地在左子树中查找，若找到则返回该值，否则继续递归地在右子树中查找，并返回其结果。

   |                                                          |            |
   | -------------------------------------------------------- | ---------- |
   | f(b, k)='#'                                              | 当b=NULL时 |
   | f(b, k)=b->data                                          | 当i=k时    |
   | f(b, k)=((ch=f(b->lchild, k))=='#'?f(b->rchild, k) : ch) | 其他情况   |

   ```cpp
   int i=1;//遍历序号的全局变量
   ElemType PreNode(BiTree b, int k)
   {
       //本算法查找二叉树先序遍历序列中第k个结点的值
       if(b==NULL)//空结点，则返回特殊字符
           return '#';
       if(i==k)//相等，则当前结点即为第k个结点
           return b->data;
       i++;//下一个结点
       ch=PreNode(b->lchild, k);//左子树中递归寻找
       if(ch!='#')//在左子树中，则返回该值
           return ch;
       ch=PreNode(b->rchild,k);//在右子树中递归寻找
       if(ch!='#')//在右子树中，则返回该值
           return ch;
   }
   ```

   代码实质上就是一个遍历算法的实现，只不过用一个全局变量来记录访问的序号，求其他遍历序列的第k个结点也采用相似的方法。二叉树的遍历算法可以引申出大量的算法题。

10. 已知二叉树以二叉链表存储，编写算法完成：对于树中每个元素值为x的结点，删除以它为根的子树，并释放相应的空间。

    删除以元素值x为根的子树，只要能删除其左、右子树，就可以释放值为x的根结点，因此宜采用后序遍历。算法思想：删除值为x的结点，意味着应将其父结点的左（右）子女指针置空，用层次遍历易于找到某结点的父结点。要求删除树中每个元素值为x的结点的子树，因此要遍历完整棵二叉树。

    ```cpp
    void DeleteXTree(BiTree &bt)//删除以bt为根的子树
    {
        if(bt)
        {
            DeleteXTree(bt->lchild);
            DeleteXTree(bt->rchild);//删除bt的左子树、右子树
            free(bt);//释放被删结点所占的存储空间
        }
    }
    //在二叉树上查找所有以x为元素值的结点，并删除以其为根的子树
    void Search(BiTree bt, ElemType x)
    {
        BiTree Q[];//Q是存放二叉树结点指针的队列，容量足够大
        if(bt)
        {
            if(bt->data==x)//若根结点值为x，则删除整棵树
            {
                DeleteXTree(bt);
                exit(0);
            }
            InitQueue(Q);
            EnQueue(Q,bt);
            while(!IsEmpty(Q))
            {
                DeQueue(Q,p);
                if(p->lchild)//若左子女非空            
                    if(p->lchild->data==x)//左子树符合则删除左子树
                    {
                        DeleteXTree(p->lchild);
                        p->lchild=NULL;
                    }//父结点的左子女置空
                    else
                        EnQueue(Q,p->lchild);//左子树入队列
                if(p->rchild)//若右子女非空
                    if(p->rchild->data==x)//右子女符合则删除右子树
                    {
                        DeleteXTree(p->rchild);
                        p->rchild=NULL;
                    }
                	else//父结点的右子女置空
                    	EnQueue(Q,p->rchild);//右子女入队列            
            }
        }
    }
    ```

11. 在二叉树中查找值为x的结点，试编写算法（用C语言）打印值为x的结点的所有祖先，假设值为x的结点不多于一个。

    采用非递归后序遍历，最后访问根结点，访问到值为x的结点时，栈中所有元素均为该结点的祖先，依次出栈打印即可。

    ```cpp
    typedef struct{
        BiTree t;
        int tag;
    }stack;//tag=0表示左子女被访问，tag=1表示右子女被访问
    void Search(BiTree bt, ElemType x)
    {
        //在二叉树bt中，查找值为x的结点，并打印其所有祖先
        stack s[];//栈容量足够大
        top=0;
        while(bt!=NULL||top>0)
        {
            while(bt!=NULL&&bt->data!=x)//结点入栈
            {
                s[++top].t=bt;
                s[top].tag=0;
                bt=bt->lchild;//沿左右分支向下
            }
            if(bt!=NULL&&bt->data==x)
            {
                printf("所查结点的所有祖先结点的值为:\n");//找到x
                for(int i=1;i<=top;i++)
                    printf("%d",s[i].t->data);//输出祖先值后结束
                exit(1);
            }
            while(top!=0&&s[top].tag==1)
            	top--;//退栈（空遍历）
            if(top!=0)
            {
                s[top].tag=1;
                bt=s[top].t->rchild;//沿右分支向下遍历
            }
        }
    }
    ```

    因为查找的过程就是后序遍历的过程，所以使用的栈的深度不超过树的深度。