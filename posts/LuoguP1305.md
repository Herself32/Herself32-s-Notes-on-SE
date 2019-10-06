# LuoguP1305题解
> 简单的Splay操作题

<!--more-->
### Description
描述 县城里有n个用地道相连的房子，第i个只与第i-1和第i+1个相连。这是有m个消息依次传来

1、消息为D x：鬼子将x号房子摧毁了，地道被堵上。

2、消息为R ：村民们将鬼子上一个摧毁的房子修复了。

3、消息为Q x：有一名士兵被围堵在x号房子中。

李云龙收到信息很紧张，他想知道每一个被围堵的士兵能够到达的房子有几个。
### Samples
#### Input #1
```markdown
7 9
D 3
D 6
D 5
Q 4
Q 5
R
Q 4
R
Q 4
```
#### Output #1
```markdown
1
0
2
4
```

### Solution
Splay裸题.

首先对于摧毁操作`D x`, 我们把一个`val = x`的节点插入`Splay`.

这样可以让`Q x`操作转化为查询`x`的后继 - `x`的前趋 + 1.

`R`操作就直接用一个栈维护插入的数, 然后把栈顶`Remove`掉就完了.

### Code
```c++
/* Headers */
#include<algorithm>
#include<iostream>
#include<cstring>
#include<climits>
#include<cstdio>
#include<cctype>
#include<vector>
#include<cmath>
#include<queue>
#include<stack>
#include<map>
#define FOR(i,a,b,c) for(int i=(a);i<=(b);i+=(c))
#define ROF(i,a,b,c) for(int i=(a);i>=(b);i-=(c))
#define FORL(i,a,b,c) for(long long i=(a);i<=(b);i+=(c))
#define ROFL(i,a,b,c) for(long long i=(a);i>=(b);i-=(c))
#define FORR(i,a,b,c) for(register int i=(a);i<=(b);i+=(c))
#define ROFR(i,a,b,c) for(register int i=(a);i>=(b);i-=(c))
#define lowbit(x) x&(-x)
#define LeftChild(x) x<<1
#define RightChild(x) (x<<1)+1
#define RevEdge(x) x^1
#define FILE_IN(x) freopen(x,"r",stdin);
#define FILE_OUT(x) freopen(x,"w",stdout);
#define CLOSE_IN() fclose(stdin);
#define CLOSE_OUT() fclose(stdout);
#define IOS(x) std::ios::sync_with_stdio(x)
#define Dividing() printf("-----------------------------------\n");
namespace FastIO{
    const int BUFSIZE = 1 << 20;
    char ibuf[BUFSIZE],*is = ibuf,*its = ibuf;
    char obuf[BUFSIZE],*os = obuf,*ot = obuf + BUFSIZE;
    inline char getch(){
        if(is == its)
            its = (is = ibuf)+fread(ibuf,1,BUFSIZE,stdin);
        return (is == its)?EOF:*is++;
    }
    inline int getint(){
        int res = 0,neg = 0,ch = getch();
        while(!(isdigit(ch) || ch == '-') && ch != EOF)
            ch = getch();
        if(ch == '-'){
            neg = 1;ch = getch();
        }
        while(isdigit(ch)){
            res = (res << 3) + (res << 1)+ (ch - '0');
            ch = getch();
        }
        return neg?-res:res;
    }
    inline void flush(){
        fwrite(obuf,1,os - obuf,stdout);
        os = obuf;
    }
    inline void putch(char ch){
        *os++ = ch;
        if(os == ot)	flush();
    }
    inline void putint(int res){
        static char q[10];
        if(res == 0) putch('0');
        else if(res < 0){putch('-');res = -res;}
        int top = 0;
        while(res){
            q[top++] = res % 10 + '0';
            res /= 10;
        }
        while(top--)	putch(q[top]);
    }
    inline void space(bool x){
    	if(!x) putch('\n');
    	else putch(' ');
    }
}
inline void read(int &x){
    int rt = FastIO::getint();
    x = rt;
}
inline void print(int x,bool enter){
    FastIO::putint(x);
    FastIO::flush();
    FastIO::space(enter);
}
/* definitions */
const int MAXN = 5e4 + 10;
struct Tree{
	int ch[2], size, cnt, val, fa;
	#define Lef(x, k) Splay[Splay[x].ch[0]].k
	#define Rig(x, k) Splay[Splay[x].ch[1]].k
}Splay[MAXN];
int n, m, root, Node;
std::stack<int> room;
/* functions */
inline bool FIND(int x) {
	return (Splay[Splay[x].fa].ch[1] == x);
}
inline void pushUp(int k) {
	Splay[k].size = Lef(k, size) + Rig(k, size) + Splay[k].cnt;	
}
inline void Rotate(int x) {
	int fx = Splay[x].fa, fy = Splay[fx].fa;
	int chx = FIND(x), chy = Splay[x].ch[chx ^ 1];
	Splay[fx].ch[chx] = chy; Splay[chy].fa = fx;
	Splay[fy].ch[FIND(fx)] = x; Splay[x].fa = fy;
	Splay[x].ch[chx ^ 1] = fx; Splay[fx].fa = x;
	pushUp(fx); pushUp(x);
}
inline void splay(int x, int rt = 0) {
	while(Splay[x].fa != rt) {
		int fx = Splay[x].fa, fy = Splay[fx].fa;
		if(fy != rt) {
			if(FIND(x) == FIND(fx)) Rotate(fx);
			else Rotate(x);
		}
		Rotate(x);
	}
	if(!rt) root = x;
}
inline void Insert(int x) {
	int rt = root, p = 0;
	while(rt && Splay[rt].val != x) {
		p = rt;
		rt = Splay[rt].ch[x > Splay[rt].val];
	}
	if(rt) Splay[rt].cnt++;
	else {
		rt = ++Node;
		if(p) Splay[p].ch[x > Splay[p].val] = rt;
		Splay[rt].ch[0] = Splay[rt].ch[1] = 0;
		Splay[rt].fa = p; Splay[rt].val = x;
		Splay[rt].cnt = Splay[rt].cnt = 1;
	}
	splay(rt);
}
inline void LowerX(int x) {
	int rt = root;
	while(Splay[rt].ch[x > Splay[rt].val] && x != Splay[rt].val)
		rt = Splay[rt].ch[x > Splay[rt].val];
	splay(rt);
}
inline int Querypre(int x) {
	LowerX(x);
	if(Splay[root].val < x) return root;
	int rt = Splay[root].ch[0];
	while(Splay[rt].ch[1]) rt = Splay[rt].ch[1];
	return rt;
}
inline int Querysuf(int x) {
	LowerX(x);
	if(Splay[root].val > x) return root;
	int rt = Splay[root].ch[1];
	while(Splay[rt].ch[0]) rt = Splay[rt].ch[0];
	return rt;
}
inline void Remove(int x) {
	int last = Querypre(x), next = Querysuf(x);
	splay(last), splay(next, last);
	int del = Splay[next].ch[0];
	if(Splay[del].cnt > 1) {Splay[del].cnt--; splay(del);}
	else Splay[next].ch[0] = 0;
}
int main(int argc,char *argv[]){
	scanf("%d %d", &n, &m);
	Insert(0); Insert(n + 1);
	FOR(i, 1, m, 1) {
		char opt; std::cin>>opt;
		if(opt == 'D') {
			int x; scanf("%d", &x);
			Insert(x); room.push(x);
		}
		if(opt == 'R') {
			Remove(room.top()); 
			room.pop();
		}
		if(opt == 'Q') {
			int x; scanf("%d", &x);
			LowerX(x);
			if(Splay[root].val == x) puts("0");
			else {
				int last = Querypre(x), next = Querysuf(x);
				int ans = Splay[next].val - Splay[last].val - 1;
				printf("%d\n",ans);
			}
		}
	}
	return 0;
}
```

# THE END
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUxNDU0ODI3M119
-->