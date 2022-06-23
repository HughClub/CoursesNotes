## EBNF

扩充元语言符号 `{}` 表示多次出现 相当于正则的`*`

$\{{\alpha}\}_0^n$相当于正则的$\alpha\{0,n\}$

$[\alpha]===\{\alpha\}_0^1$

## 文法条件

### 递归下降

> 问题: 无限左递归

### LL(1)

1. 不含左递归
2. 没有左公共因子
3. 如果First集包含$\epsilon$ First与Follow集合交集为空

### 伪代码

文法

```bnf
E -> TE'
E' -> + TE'|e
T->FT'
T'->*FT'|e
F->(E)|i
```



```C
char SYM;
void F(){
    if(SYM=='i'){advance();}
    else if(SYM=='('){
        advance();E();
        if(SYM==')'){advance();}
        else{error();}
    }else{error();}
}
void E(){
    T();
    _E();
}
void _E/*E'*/(){
    if(SYM=='+'){
        advance();
        T();_E();
    }
}
void T(){
    F();
    _T();
}
void _T(){
    if(SYM=='*'){
        advance();
        F();_T();
    }
}
int main(){
    advance();
    E();
    if(SYM!='$')/* end of input/file */{error();}
}
```



### LR(0)

不存在 S/R 和 R/R 冲突

### SLR(1)

> 问题: Follow集信息太泛

## 术语

短语:$S\Rightarrow\alpha A\delta$ 且 $A\Rightarrow\beta$ 则$\beta$是句型$\alpha A\delta$相对于非终结符A的短语

直接短语:$A\to\beta$

句柄:一个句型最左直接短语



综合属性:只能通过子结点或本身属性值定义

继承属性:只能通过N的父结点 兄弟结点和N本身的属性值来确定

> 终结符只有**综合属性**

S属性文法:只包含综合属性的文法

L属性文法

1. 属性为综合属性
2. 属性为继承属性 则自从之前的之前的符号或者父结点获得

带优化的布尔表达式翻译

```bnf
E ::= E or E { if E true else E }
E ::= E and E { if E E else false }
```

truelist: 记录为`true`时地址的链表