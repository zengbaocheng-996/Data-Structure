|      |        | Description                                                  |
| ---- | ------ | ------------------------------------------------------------ |
| 2    | 线性表 | 线性表的定义和基本操作<br />线性表的顺序表示<br />线性表的链式表示 |

1. 从顺序表中删除具有最小值的元素（假设唯一）并由函数返回被删元素的值。空出的位置由最后一个元素填补，若顺序表为空，则显示出错信息并退出运行。

   搜索整个顺序表，查找最小值元素并记住其位置，搜索结束后用最后一个元素填补空出的原最小值元素的位置。

   ```cpp
   bool Del_Min(SqList &L, ElemType &value)
   {
       //删除顺序表L中最小值的元素结点，并通过引用型参数value返回其值
       //若删除成功，则返回true，否则返回false
       if(L.length==0)
           return false;//表空，终止操作返回
       value=L.data[0];
       int pos=0;//假定0号元素值最小
       for(int i=1;i<L.length;i++)//循环，寻找具有最小值的元素
           if(L.data[i]<value)//让value记忆当前具有最小值的元素
           {
               value=L.data[i];
               pos=i;
           }
       L.data[pos]=L.data[L.length-1];//空出的位置由最后一个元素填补
       L.length--;
       return true;//此时，value即为最小值
   }
   ```

2. 设计一个高效算法，将顺序表L的所有元素逆置，要求算法的空间复杂度为O(1)。

   扫描顺序表L的前半部分元素，对于元素L.data\[i\](0<=i<L.length/2)，将其与后半部分的对应元素L.data[L.length-i-1]进行交换。

   ```cpp
   void Reverse(SqList &L)
   {
       ElemType temp;//辅助变量
       for(int i=0;i<L.length/2;i++)
       {
           temp=L.data[i];//交换L.data[i]与L.data[L.length-i-1]
           L.data[i]=L.data[L.length-i-1];
           L.data[L.length-i-1]=temp;
       }
   }
   ```

3. 对长度为n的顺序表L，编写一个时间复杂度为O(n)，空间复杂度为O(1)的算法，该算法删除顺序表中所有值为x的数据元素。

   解法1：用k记录顺序表L中不等于x的元素个数（即需要保存的元素个数），扫描时将不等于x的元素移动到下标k的位置，并更新k值。扫描结束后修改L的长度。

   ```cpp
   void del_x_1(SqList &L, ElemType x)
   {
       //实现删除顺序表L中所有值为x的数据元素
       int k=0,i;//记录不等于x的元素个数
       for(i=0;i<L.length;i++)
           if(L.data[i]!=x)
           {
               L.data[k]=L.data[i];
               k++;//不等于x的元素增1
           }
       L.length=k;//顺序表L的长度等于k
   }
   ```

   解法2：用k记录顺序表L中等于x的元素个数，一边扫描L，一边统计k，并将不等于x的元素前移k个位置。扫描结束后修改L的长度。

   ```cpp
   void del_x_2(SqList &L, ElemType x)
   {
       int k=0,i=0;//k记录值等于x的元素个数
       while(i<L.length)
       {
           if(L.data[i]==x)
               k++;
           else
               L.data[i-k]=L.data[i];//当前元素前移k个位置
           i++;
       }
       L.length=L.length-k//顺序表L的长度递减
   }
   ```

   解法3：设头、尾两个指针（i=1, j=n），从两端向中间移动，在遇到最左端值x的元素时，直接将最右端非x的元素左移至值为x的数据元素位置，直到两指针相遇。但这种方法会改变原来表中元素的相对位置。

4. 从顺序表中删除其值在给定值s和t之间（包含s和t，要求s<t）的所有元素，若s或t不合理或顺序表为空，则显示出错信息并退出运行。

   从前向后扫描顺序表L，用k记录值在s和t之间的元素个数（初始时k=0）。对于当前扫描的元素，若其值不在s和t之间，则前移k个位置；否则执行k++。由于每个不在s和t之间的元素仅移动一次，因此算法效率高。

   ```cpp
   bool Del_s_t(SqList &L, ElemType s, ElemType t)
   {
   	//删除顺序表L中值在给定值s和t（要求s<t）之间的所有元素
       int i,k=0;
       if(L.length==0||s>=t)
           return false;//线性表为空或s、t不合法，返回
       for(i=0;i<L.length;i++)
       {
           if(L.data[i]>=s&&L.data[i]<=t)
               k++;
           else
               L.data[i-k]=L.data[i];//当前元素前移k个位置
       }
       L.length-=k;//长度减小
       return true;    
   }
   ```

   也可从后向前扫描顺序表，每遇到一个值在s和t之间的元素，就删除该元素，其后的所有元素全部前移。但移动次数远大于前者，效率不够高。

5. 从有序顺序表中删除所有其值重复的元素，使表中所有元素的值均不同。

   因为是有序顺序表，所以值相同的元素一定在连续的位置上，用类似于直接插入排序的思想，初始时将第一个元素视为非重复的有序表。之后依次判断后面的元素是否与前面非重复有序表的最后一个元素相同，若相同，则继续向后判断，若不同，则插入前面的非重复有序表的最后，直至判断到表尾为止。

   ```cpp
   bool Delete_Same(SeqList& L)
   {
       if(L.length==0)
           return false;
       int i,j;//i存储第一个不相同的元素，j为工作指针
       for(i=0,j=1;j<L.length;j++)
           if(L.data[i]!=L.data[j])//查找下一个与上个元素值不同的元素
               L.data[++i]=L.data[j];//找到后，将元素前移
       L.length=i+1;
       return true;
   }
   ```

6. 将两个有序顺序表合并为一个新的有序顺序表，并由函数返回结果顺序表。

   首先，按顺序不断取下两个顺序表表头较小的结点存入新的顺序表中。然后，看哪个表还有剩余，将剩下的部分加到新的顺序表后面。

   ```cpp
   bool Merge(SeqList A, SeqList B, SeqList &C)
   {
       //将有序顺序表A与B合并为一个新的有序顺序表C
       if(A.length+B.length>C.maxSize)//大于顺序表的最大长度
       	return false;
       int i=0,j=0,k=0;
       while(i<A.length&&j<B.length)//循环，两两比较，小者存入结果表
       {
           if(A.data[i]<＝B.data[j])
               C.data[k++]=A.data[i++];
           else
               C.data[k++]=B.data[j++];
       }
       while(i<A.length)//还剩一个没有比较完的顺序表
           C.data[k++]=A.data[j++];
       while(j<B.length)
           C.data[k++]=B.data[j++];
       C.length=k;
       return true;    
   }
   ```

7. 已知在一维数组A[m+n]中依次存放两个线性表(a1,a2,a3,..,an)和(b1,b2,b3,...,bn)。编写一个函数，将数组中两个顺序表的位置互换，即将(b1,b2,b3,...,bn)放到(a1,a2,a3,..,an)的前面。

   首先将数组A[m+n]中的全部元素(a1,a2,a3,...,am,b1,b2,b3,...,bn)原地逆置为(bn,bn-1,bn-2,...,b1,am,am-1,am-2,...,a1)，然后对前n个元素和后m个元素分别使用逆置算法，即可得到(b1,b2,b3,...,bn,a1,a2,a3,...,am)，从而实现顺序表的位置互换。

   ```cpp
   typedef int DataType;
   void Reverse(DataType A[], int left, int right, int arraySize)
   {
       //逆转(aleft,aleft+1,aleft+2...,aright)为(aright, aright-1, ..., aleft)
       if(left>=right||right>=arraySize)
           return;
       int mid = (left+right)/2;
       for(int i=0;i<mid-left;i++)
       {
           DataType temp=A[left+i];
           A[left+i]=A[right-i];
           A[right-i]=temp;
       }
   }
   void Exchange(DataType A[], int m, int n, int arraySize)
   {
       //数组A[m+n]中，从0到m-1存放顺序表(a1,a2,a3,..,am)，从m到m+n-1存放顺序表(b1,b2,b3,...,bn)，算法将这两个表的位置互换
       Reverse(A,0,m+n-1,arraySize);
       Reverse(A,0,n-1,arraySize);
       Reverse(A,n,m+n-1,arraySize);
   }
   ```

8. 线性表(a1,a2,a3,...,an)中的元素递增有序且按顺序存储于计算机内。要求设计一个算法，完成用最少时间在表中查找数值为x的元素，若找到，则将其与后继元素位置相交换，若找不到，则将其插入表中并使表中元素仍递增有序。

   顺序存储的线性表递增有序，可以顺序查找，也可以折半查找。折半耗时低。

   ```cpp
   void SearchExchangeInsert(ElemType A[], ElemType x)
   {
       int low=0, high=n-1,mid;//low和high指向顺序表下界和上界的下标
       while(low<=high)
       {
           mid=(low+high)/2;//找中间位置
           if(A[mid]==x)break;//找到x，退出while循环
           else if(A[mid]<x)low=mid+1;//到中点mid的右半部去查
           else high=mid-1;//到中点mid的左半部去查
       }//下面两个if语句只会执行一个
       //若最后一个元素与x相等，则不存在与其后继交换的操作
       if(A[mid]==x&&mid!=n-1)
       {
           t=A[mid];
           A[mid]=A[mid+1];
           A[mid+1]=t;
       }
       if(low>high)//查找失败，插入数据元素x
       {
           for(i=n-1;i>high;i--)A[i+1]=A[i];//后移元素
           A[i+1]=x;//插入x
       }
   }
   ```

   也可写成三个函数：查找函数、交换后继函数和插入函数。

9. 给定三个序列A、B、C，长度均为n，且均为无重复元素的递增序列，请设计一个时间上尽可能高效的算法，逐行输出同时存在于这三个序列中的所有元素。例如，数组A为{1，2，3}，数组B为{2，3，4}，数组C为{-1，0，2}，则输出2。

   - 给出算法的基本设计思想。
   - 根据设计思想，采用C或C++语言描述算法，关键之处给出注释。
   - 说明你的算法的时间复杂度和空间复杂度。

   使用三个下标变量从小到大遍历数组。当三个下标变量指向的元素相等时，输出并向前推进指针，否则仅移动小于最大元素的下标变量，直到某个下标变量移出数组范围，即可停止。

   ```cpp
   void sameKey(int A[], int B[], int C[], int n)
   {
       int i=0,j=0,k=0;//定义三个工作指针
       while(i<n&&j<n&&k<n)//相同则输出，并集体后移
       {
           if(A[i]=B[j]&&B[j]==C[k])
           {
               printf("%d\n",A[i]);
               i++;j++;k++;
           }
           else
           {
               int maxNum=max(A[i],max(B[j],C[k]));
               if(A[i]<maxNum)i++;
               if(B[j]<maxNum)j++;
               if(C[k]<maxNum)k++;
           }
       }
   }
   ```

   每个指针移动的次数不超过n次，且每次循环至少有一个指针后移，所以时间复杂度为O(n)，算法只用到了常数个变量，空间复杂度为O(1)。

10. 设将n(n>1)个整数存放到一维数组R中。设计一个在时间和空间两方面都尽可能高效的算法。将R中保存的序列循环左移p(0<p<n)个位置，即将R中的数据由(X0,X1,...,Xn-1)变换为(Xp,Xp+1,...,Xn-1,X0,X1,...,Xp-1)。要求：

    - 给出算法的基本设计思想。
    - 根据设计思想，采用C或C++或Java语言描述算法，关键之处给出注释。
    - 说明你所设计算法的时间复杂度和空间复杂度。

    可将问题视为把数组ab转换成数组ba（a代表数组的前p个元素，b代表数组中余下的n-p个元素），先将a逆置得到a-1b，再将b逆置得到a-1b-1，最后将整个a-1b-1逆置得到（a-1b-1）-1=ba。设Reverse函数执行将数组逆置的操作，对abcdefgh向左循环移动3（p=3）个位置的过程如下：

    Reverse(0,p-1)得到cbadefgh;

    Reverse(p,n-1)得到cbahgfed;

    Reverse(0,n-1)得到defghabc。

    在Reverse中，两个参数分别表示数组中待转换元素的始末位置。

    ```cpp
    void Reverse(int R[], int from, int to)
    {
        int i, temp;
        for(i=0;i<(to-from+1)/2;i++)
        {
            temp=R[from+i];
            R[from+i]=R[to-i];
            R[to-i]=temp;
        }
    }
    void Converse(int R[], int n, int p)
    {
        Reverse(R, 0, p-1);
        Reverse(R, p, n-1);
        Reverse(R, 0, n-1);
    }
    ```

    上述算法中三个Reverse函数的时间复杂度分别为O(p/2)、O((n-p)/2)和O(n/2)，故所设计的算法的时间复杂度为O(n)，空间复杂度为O(1)。

    也可借助辅助数组来实现，创建大小为p的辅助数组S，将R中前p个整数依次暂存在S中，同时将R中后n-p个整数左移，然后将S中暂存的p个数依次放回到R中的后续单元。时间复杂度为O(n)，空间复杂度为O(p)。

11. 一个长度为L（L>=1）的升序序列为S，处在第[L/2]个位置的数称为S的中位数。例如，若序列S1=（11，13，15，17，19），则S1的中位数是15，两个序列的中位数是含它们所有元素的升序序列的中位数。例如，若S2=（2，4，6，8，20），则S1和S2的中位数是11。现在有两个等长升序序列A和B，试设计一个在时间和空间两方面都尽可能高效的算法，找出两个序列A和B的中位数。要求：

    - 给出算法的设计思想。
    - 根据设计思想，采用C或C++或Java语言描述算法，关键之处给出注释。
    - 说明你所设计算法的时间复杂度和空间复杂度。

    分别求两个升序序列A、B的中位数，设为a和b，求序列A、B的中位数过程如下：

    条件1：若a=b，则a或b即为所求中位数，算法结束。

    条件2：若a<b，则舍弃序列A中较小的一半，同时舍弃序列B中较大的一半，要求两次舍弃的长度相等。

    条件3：若a>b，则舍弃序列A中较大的一半，同时舍弃序列B中较小的一半，要求两次舍弃的长度相等。

    在保留的两个升序序列中，重复过程，直到两个序列中均只含一个元素时为止，较小者即为所求的中位数。

    ```cpp
    int M_Search(int A[], int B[], int n)
    {
        int s1,d1,m1,s2,d2,m2;
        s1=0;d1=n-1;
        s2=1;d2=n-1;
        while(s1!=d1||s2!=d2)
        {
            m1=(s1+d1)/2;
            m2=(s2+d2)/2;
            if(A[m1]==B[m2])
                return A[m1];//满足条件1
            if(A[m1]<B[m2])//满足条件2
            {
                if((s1+d1)%2==0)//若元素个数为奇数
                {
                    s1=m1;//舍弃A中间点以前的部分，且保留中间点
                    d2=m2;//舍弃B中间点以后的部分，且保留中间点
                }
                else//元素个数为偶数
                {
                	s1=m1+1;//舍弃A的前半部分
                    d2=m2;//舍弃B的后半部分
                }
            }
            else//满足条件3
            {
                if((s1+d1)%2==0)//若元素个数为奇数
                {
                    d1=m1;//舍弃A中间点以后的部分，且保留中间点
                    s2=m2;//舍弃B中间点以前的部分，且保留中间点
                }
                else//元素个数为偶数
                {
                    d1=m1;//舍弃A的后半部分
                    s2=m2+1;//舍弃B的前半部分
                }
            }
        }
        return A[s1]<B[s2]?A[s1]:B[s2];
    }
    ```

    算法的时间复杂度为O(log2n)，空间复杂度为O(1)。

    另解：对两个长度为n的升序序列A和B中的元素按从小到大的顺序依次访问，这里访问的含义只是比较序列中两个元素的大小，并不实现两个序列的合并，因此空间复杂度为O(1)。按照上述规则访问第n个元素时，这个元素即为两个序列A和B的中位数。

12. 已知一个整数序列A=(a0,a1,...,an-1)，其中0<=ai<n(0<=i<n)。若存在ap1=ap2=...=apm=x且m>n/2(0<=pk<n,1<=k<m)，则称x为A的主元素。例如A=（0，5，5，3，5，7，5，5），则5为主元素；又如A=（0，5，5，3，5，1，5，7），则A中没有主元素。假设A中的n个元素保存在一个一维数组中，请设计一个尽可能高效的算法，找出A的主元素。若存在主元素，则输出该元素；否则输出-1。要求：

    - 给出算法的基本设计思想
    - 根据设计思想，采用C或C++或Java语言描述算法，关键之处给出注释。
    - 说明你所设计算法的时间复杂度和空间复杂度。

    算法的策略是从前向后扫描数组元素，标记出一个可能成为主元素的元素Num。然后重新计数，确认Num是否是主元素。

    第一步：选取候选的主元素。依次扫描所给数组中的每个整数，将第一个遇到的整数Num保存到c中，记录Num的出现次数为1；若遇到的下一个整数仍等于Num，则计数+1，否则计数减1；当计数减到0时，将遇到的下一个整数保存到c中，计数重新记为1，开始新一轮计数，即从当前位置开始重复上述过程，直到扫描完全部数组元素。

    第二部：判断c中元素是否是真正的主元素。再次扫描该数组，统计c中元素出现的次数，若大于n/2，则为主元素；否则，序列中不存在主元素。

    ```cpp
    int Majority(int A[], int n)
    {
        int i,c,count=1;//c用来保存候选主元素，count用来计数
        c=A[0];//设置A[0]为候选主元素
        for(i=1;i<n;i++)//查找候选主元素
        	if(A[i]==c)
                count++;//对A中的候选主元素计数
            else
                if(count>0)//处理不是候选主元素的情况
                    count--;
            	else//更换候选主元素，重新计数
                {
                    c=A[i];
                    count=1;
                }
        if(count>0)
            for(i=count=0;i<n;i++)//统计候选主元素的实际出现次数
                if(A[i]==c)
                    count++;
        if(count>n/2)return c;//确认候选主元素
        else return -1;//不存在主元素
    }
    ```

    实现的程序的时间复杂度为O(n)，空间复杂度为O(1)。

13. 给定一个含n(n>=1)个整数的数组，请设计一个在时间上尽可能高效的算法，找出数组中未出现的最小正整数。例如，数组{-5，3，2，3}中未出现的最小正整数是1；数组{1，2，3}中未出现的最小正整数是4。要求：

    - 给出算法的设计思想
    - 根据设计思想，采用C或C++语言描述算法，关键之处给出注释。
    - 说明你所设计算法的时间复杂度和空间复杂度。

    要求在时间上尽可能高效，因此采用空间换时间的办法。分配一个用于标记的数组B[n]，用来记录A中是否出现了1~n中的正整数，B[0]对应正整数1，B[n-1]对应正整数n，初始化B中全部为0。由于A中含有n个整数，因此可能返回的值是1~n+1，当A中n个数恰好为1~n时返回n+1。当数组A中出现了小于或等于0或大于n的值时，会导致1~n出现空余位置，返回结果必然在1~n中，因此对于A中出现了小于或等于0或大于n的值，可以不采取任何操作。

    经过以上分析可以得出算法流程：从A[0]开始遍历A，若0<A[i]<=n，则令B[A[i]-1]=1;否则不做操作。对A遍历结束后，开始遍历数组B，若能查找到第一个满足B[i]==0的下标i，返回i+1即为结果，此时说明A中未出现的最小正整数在1和n之间。若B[i]全部不为0，返回i+1(跳出循环时i=n，i+1等于n+1)，此时说明A中未出现的最小正整数是n+1。

    ```cpp
    int findMissMin(int A[], int n)
    {
        int i, *B;//标记数组
        B=(int *)malloc(sizeof(int)*n);//分配空间
        memset(B,0,sizeof(int)*n);//赋初值为0
        for(i=0;i<n;i++)
            if(A[i]>0&&A[i]<=n)//若A[i]的值介于1~n，则标记数组B
                B[A[i]-1]=1;
        for(i=0;i<n;i++)//扫描数组B，找到目标值
            if(B[i]==0)break;
        return i+1;//返回结果
    }
    ```

    时间复杂度：遍历A一次，遍历B一次，两次循环内操作步骤为O(1)量级，因此时间复杂度为O(n)。空间复杂度：额外分配了B[n]，空间复杂度为O(n)。

14. 定义三元组(a,b,c)（a,b,c均为整数）的距离D=|a-b|+|b-c|+|c-a|。给定3个非空整数集合S1、S2、S3，按升序分别存储在3个数组中。请设计一个尽可能高效的算法，计算并输出所有可能的三元组(a,b,c)（a属于S1，b属于S2，c属于S3）中的最小距离。例如S1={-1,0,9}，S2={-25,-10,10,11}，S3={2,9,17,30,41}，则最小距离为2，相应的三元组为(9,19,9)。要求：

    - 给出算法的基本设计思想。
    - 根据设计思想，采用C语言或C++语言描述算法，关键之处给出注释。
    - 说明你所涉及算法的时间复杂度和空间复杂度。

    <img src="1.png" style="zoom:10%;" />
    $$
    D=|a-b|+|b-c|+|c-a|\geq0\\
    假设a\leq b\leq c\\
    D=L_1+L_2+L_3=2L_3
    $$
    由D的表达式可知，事实上决定D大小的关键是a和c之间的距离，于是问题就可以简化为每次固定c找一个a，使得L3=|c-a|最小。
    
    第一步：使用Dmin记录所有已处理的三元组的最小距离，初值为一个足够大的整数。
    
    第二部：集合S1、S2和S3分别保存在数组A、B、C中。数组的下标变量i=j=k=0，当i<|S1|、j<|S2|且k<|S3|时（|S|表示集合S中的元素个数），循环执行下面的a到c
    
    a) 计算(A[i],B[j],C[k])的距离D；（计算D）
    
    b) 若D<Dmin，则Dmin=D；（更新D）
    
    c) 将A[i]、B[j]、C[k]中的最小值的下标+1；（对照分析：最小值为a，最大值为c，这里c不变而更新a，试图寻找更小的距离D）
    
    第三步：输出Dmin，结束。
    
    ```cpp
    #define INT_MAX 0x7fffffff
    int abs_(int a)//计算绝对值
    {
        if(a<0)return -a;
        else return a;
    }
    bool xls_min(int a, int b, int c)
    {
        //a是否是三个数中的最小值
        if(a<=b&&a<=c)return true;
        return false;
    }
    int findMinofTrip(int A[], int n, int B[], int m, int C[], int p)
    {
        //D_min用于记录三元组的最小距离，初值赋为INT_MAX
        int i=0,j=0,k=0,D_min=INT_MAX,D;
        while(i<n&&j<m&&k<p&&D_min>0)
        {
            D=abs_(A[i]-B[j])+abs_(B[j]-C[k])+abs_(C[k]-A[i]);//计算D
            if(D<D_min)D_min=D;//更新D
            if(xls_min(A[i],B[j],C[k]))i++;//更新a
            else if(xls_min(B[j],C[k],A[i]))j++;
            else k++;
        }
        return D_min;
    }
    ```
    
    设n=(|S1|+|S2|+|S3|)，时间复杂度为O(n)，空间复杂度为O(1)。
