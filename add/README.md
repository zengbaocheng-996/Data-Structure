|      |                | Description                                          |
| ---- | -------------- | ---------------------------------------------------- |
| 3    | 栈、队列和数组 | 栈<br />队列<br />栈和队列的应用<br />数组和特殊矩阵 |

斐波那契数列

```cpp
//递归
int Fibonacci_recursion(int n)
{
    if(n<=0)return 0;
    if(n==1)return 1;
    return Fibonacci_recursion(n-1)+Fibonacci_recursion(n-2);
}
//非递归，循环
int Fibonacci_loop(int n)
{
    int *a = new int[n]{1,1};//a[0]=a[1]=1,a[n-1]是第n个元素
    for(int i=2;i<n;i++)
        a[i]=a[i-1]+a[i-2];
    return a[n-1];
}
```

假设一个算术表达式中包含圆括号、方括号和花括号3中类型的括号，编写一个算法来判别表达式中的括号是否配对，以字符"\0"作为算术表达式的结束符。

算法的基本思想是扫描每个字符，遇到花、中、圆的左括号时进栈，遇到花、中、圆的右括号时检查栈顶元素是否为相应的左括号，若是，退栈，否则配对错误。最后栈若不为空也为错误。

```cpp
bool BracketsCheck(char *str)
{
    InitStack(S);//初始化栈
    int i=0;
    while(str[i]!='\0')
    {
        switch(str[i])
        {
            //左括号入栈
            case '(':Push(S,'(');break;
            case '[':Push(S,'[');break;
            case '{':Push(S,']');break;
            //遇到右括号，检测栈顶
            case ')':Pop(S,e);
                if(e!='(')return false;
                break;
            case ']':
                if(e!='[')return false;
                break;
            case '}':
                if(e!='{')return false;
                break;
            default:
                break;
        }
        i++;
    }
    if(!isEmpty(S))
    {
        printf("括号不匹配\n");
        return false;
    }
    else
    {
        printf("括号匹配\n");
        return true;
    }
}
```

中缀转后缀

```cpp
void Infix2Suffix(SqStack *S, ElemType str[])
{
    int i=0;
    ElemType e;
    InitStack(s);
    while(str[i]!='\0')
    {
        while(isdigit(str[i]))//遇到数字直接输出，直到下一位不是数字字符，打印空格跳出循环
        {
            printf("%c",str[i++]);
            if(!isdigit(str[i]))
            {
                printf(" ");
            }
        }
        /*
        加减运算符优先级最低，如果栈顶元素为空则直接入栈，否则将栈中存储的运算符全部弹栈，
        如果遇到左括号则停止，将弹出的左括号重新压栈，因为左括号要和右括号匹配时弹出。
        弹出后将优先级低的运算符压入栈中。
        */
        if(str[i]=='+'||str[i]=='-')
        {
            if(!StackLength(S))
            {
                PushStack(S,str[i]);
            }
            else
            {
                do
                {
                    PopStack(S,&e);
                    if(e=='(')
                    {
                        PushStack(S,e);
                    }
                    else
                    {
                        printf("%c",e);
                    }
                }while(StackLength(S)&&e!='(');
                PushStack(S,str[i]);
            }
        }
        /*
        当遇到右括号时，把栈中运算符弹出，直到匹配到左括号为止
        右括号不压栈，左括号只弹出不打印
        */
        else if(str[i]==')')
        {
            PopStack(S,&e);
            while(e!='(')
            {
                printf("%c ",e);
                PopStack(S,&e);
            }
        }
        //乘、除、左括号都是优先级高的，直接压栈
        else if(str[i]=='*'||str[i]=='/'||str[i]=='(')
        {
            PushStack(S,str[i]);
        }
        else if(str[i]=='\0')
        {
            break;
        }
        else
        {
            printf("输入格式错误\n");
            return;
        }
        i++;
        //最后把栈中剩余运算符依次弹栈打印
        while(StackLength(S))
        {
            PopStack(S,&e);
            printf("%c ",e);
        }
    }
}
```

后缀表达式计算

```cpp
int SuffixCal(char* str)
{
    int i=0;
    Stack S;
    InitStack(&S);
    ElemType number_to_push, num1, num2;
    while(str[i]!='\0')//判断是否到末尾
    {
        if(str[i]!=' ')
        {
            if(str[i]>='0'&&str[i]<='9')//是数字
            {
                number_to_push = 0;
                while(str[i]!=' '&&str[i])//将字符串转换为数字
                {
                    number_to_push = number_to_push*10+(str[i]-'0');
                    i++;
                }
                push(&S,number_to_push);
            }
            else
            {
                pop(&S,&num2);
                pop(&S,&num1);
                switch(str[i])
                {
                    case '+':
                        num1 += num2;
                        break;
                    case '-':
                        num1 -= num2;
                        break;
                    case '*':
                        num1 *= num2;
                        break;
                    case '/':
                        num1 /= num2;
                        break;
                    case '%':
                        num1 %= num2;
                        break;
                }
                push(&S,num1);
            }    
        }
        i++;
    }
	pop(&S, &num1);//最后的结果
    return num1;
}
```

