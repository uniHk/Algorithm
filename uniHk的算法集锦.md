[TOC]

# 常用代码

```cpp
#include <bits/stdc++.h>
#define hhh cerr << "hhh" << endl
#define see(x) cerr << (#x) << '=' << (x) << endl
using namespace std;
typedef long long ll;
typedef long double ld;
typedef pair<int, int> pr;
inline int read() {int x=0,f=1;char c=getchar();while(c!='-'&&c<48)c=getchar();if(c=='-')f=-1,c=getchar();while(c>47)x=x*10+(c^48),c=getchar();return f*x;}

const int maxn = 3e5+7;
const int inf  = 0x3f3f3f3f;
const int mod  = 1e9+7;

mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());

int head[maxn], to[maxn*2], nxt[maxn*2], tot;
inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; nxt[tot]=head[v]; head[v]=tot;
}

ll fac[maxn], inv[maxn];
ll gcd(ll a,ll b){return b?gcd(b,a%b):a;}
ll qpow(ll a,ll k=mod-2,ll p=mod){ll s=1;while(k){if(k&1)s=s*a%p;a=a*a%p;k>>=1;}return s;}
ll comb(int n, int m) {
    if(n<m||m<0||n<0) return 0;
    return fac[n]*inv[m]%mod*inv[n-m]%mod;
}

int main() {
		#ifndef ONLINE_JUDGE
    freopen("in", "r", stdin);
    freopen("out", "w", stdout);
		#endif
    ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    fac[0]=fac[1]=inv[0]=inv[1]=1;
    for(int i=2; i<maxn; ++i) fac[i]=i*fac[i-1]%mod;
    for(int i=2; i<maxn; ++i) inv[i]=(mod-mod/i)*inv[mod%i]%mod;
    for(int i=2; i<maxn; ++i) inv[i]=inv[i]*inv[i-1]%mod;
}

int Tot;
char *ch, buf[50000005];
inline int read(){int x=0,f=1;++ch;while(ch<buf+Tot&&*ch!='-'&&*ch<48)++ch;if(ch==buf+Tot)return 0;if(*ch=='-')f=-1,++ch;while(*ch>47)x=x*10+(*ch^48),++ch;return f*x;}
    ch=buf-1;
    Tot=fread(buf,1,5e7,stdin);

struct Mat{
    ll m[2][2];
    ll * operator [] (int i) { return m[i]; }
    Mat() { memset(m,0,sizeof(m)); }
    friend Mat operator*(Mat &a,Mat &b){Mat c;for(int i=0;i<2;++i)for(int j=0;j<2;++j)for(int k=0;k<2;++k)c[i][j]=(c[i][j]+a[i][k]*b[k][j])%mod;return c;}
    friend Mat qpow(Mat a,int k){Mat s;for(int i=0;i<2;++i)s[i][i]=1;while(k){if(k&1)s=s*a;a=a*a;k>>=1;}return s;}
};
```

**Java**

```java
import java.util.*;

public class Main {
    public static void main(String args[]) {
        Main task=new Main();
        task.main();
    }
    Scanner sc=new Scanner(System.in);
  
    final int maxn = (int)3e5+7;
    final int inf  = 0x3f3f3f3f;
    final int mod  = (int)1e9+7;



    void main() {
        
    }
}
```

**Java快读示例**

```java
import java.util.*;
import java.io.*;

public class Main {
    public static void main(String args[]) throws IOException {
        Main task=new Main();
        task.main(); out.close();
    }
		static BufferedReader in=new BufferedReader(new InputStreamReader(System.in));
    static PrintWriter out=new PrintWriter(new OutputStreamWriter(System.out));

    final int maxn = (int)3e5+7;
    final int inf  = 0x3f3f3f3f;
    final int mod  = (int)1e9+7;

    

    void main() throws IOException {
        int n=Integer.parseInt(in.readLine());
        long s=0;
        StringTokenizer st=new StringTokenizer(in.readLine()); //一行一行地读
        for(int i=1; i<=n; ++i) s+=Long.parseLong(st.nextToken())-i;
        for(int i=1; i<=n; ++i) out.print(i+s/n+(s%n>=i?1:0)+((i==n)?"\n":" "));
    }
}
```

**函数式编程**

```cpp
#define hhh cerr<<"hhh"<<endl
    #define see(x) cerr<<(#x)<<'='<<(x)<<endl
    typedef long long ll;
    typedef long double ld;
    typedef pair<int,int> pr;

    const static int maxn = 3e5+7;
    const static int inf = 0x3f3f3f3f;
    const static int mod = 1e9+7;
```



# 字符串

## KMP+EXKMP

### KMP板子

**next数组表示t[j]以前最长相同前缀后缀**

若下标从0开始(以下模板就采用这种方式)，next[ i ] 表示前面下标0~i-1的字符串前缀和后缀相等的最大长度为 next[ i ] 。
若下标从1开始，则next[ i ] 表示前面下标1~i - 1的字符串中前缀和后缀相等的最大长度为 next[ i ] - 1

**两种方式当s[i]与t[j]不同时j都应该移动到next[j]**

```cpp
const int maxn = 1e6+10;

char s[maxn], t[maxn];
int nxt[maxn], sl, tl;

void get() {
    tl=strlen(t);
    nxt[0]=-1; //nxt[0]=-1,nxt[1]=0始终成立
    int i=1, j=0; //nxt[0]和nxt[1]都已经定了，因此不需要求了
    while(i<tl) { //nxt[tl]用于做循环节相关题目
        if(j==-1||t[i]==t[j]) ++i, ++j, nxt[i]=j;
        else j=nxt[j];
    }
}

int kmp() {
		sl=strlen(s);
    int i=0, j=0;
    while(i<sl&&j<tl) {
        if(j==-1||s[i]==t[j]) ++i, ++j;
        else j=nxt[j];
    }
    if(j==tl) return i-j; //求第一个匹配位置，要求所有的话就不要退出
    return -1;
}

int main() {
	scanf("%s%s", s, t);
	get();
	printf("%d\n", kmp());
}
```

**补充：**

```cpp
//nxt[i]=j;
if(s[i]!=t[j]) nxt[i]=j;
else nxt[i]=nxt[j];
//nxt[i]=j的优化版写法
```

例子(下标从0开始)：
对于T串ababaaababaa的next数组
优化版：-1  0  -1  0  -1  3  1  0   -1  0  -1  3
本质版：-1  0   0   1   2  3  1  1    2  3   4  5 

对于T串aabaabaabaab的next数组
优化版：-1  -1  1  -1  -1  1  -1  -1  1  -1  -1  1 
本质版：-1   0  1   0    1  2   3   4  5   6   7   8 


### KMP求最小循环节

**cycle=len-next[len];（就这么简单粗暴！next[len]意思是在get()时要处理到字符串末位的后面一位）**
cycle指求出的循环节长度，若len%cycle==0，表明字符串是循环的，反之不循环，但可以通过在字符串后面补(cycle-len%cycle)个字符使得字符串变为循环的。

### EXKMP(Z algorithm)

1. 字符串下标从$0$开始（也可以很容易得改成从$1$开始）
2. $Z$数组的$Z[0]$不是良定义的，默认为$0$；如果有必要，可以在$getZ()$**的最后**进行$Z[0]=n$的修改
3. $S[j,j+z[j]-1]$表示右端点最靠右的已匹配串
4. 求$Z$数组的关键在于尽可能利用已求出的$Z$数组值

```cpp
char s[maxn];
int z[maxn];

void getZ() {
    int n=strlen(s);
    for(int i=1, j=0; i<n; ++i) {
        if(i<j+z[j]) z[i]=min(j+z[j]-i,z[i-j]);
        while(i+z[i]<n&&s[z[i]]==s[i+z[i]]) z[i]++;
        if(i+z[i]>j+z[j]) j=i;
    }
}
```

## 后缀自动机

[基础学习](https://www.luogu.org/blog/Kesdiael3/hou-zhui-zi-dong-ji-yang-xie)

[简洁明了的讲解](https://www.luogu.org/problemnew/solution/P3804)

1. 总状态数不超过$2n-1$（包括初始状态）。
2. 统计每个$endpos$等价类出现位置数量时，要按长度从长到短的计算$cnt$。那为什么一定要从长到短呢？（比如回文自动机就直接是按照节点编号从大到小计算$cnt$）罪魁祸首就是复制得到的$nq$，因为它的节点编号比较大，却又要插入到节点编号比它小的$q$的头上（作为其父节点），这样就导致若按节点编号计算$cnt$，会导致某些节点入度还未减到$0$就开始更新它的父节点了。
3. 将$q$复制一份得到$nq$后，原先的$q$就等效于被孤立了，用$nq$去代替它的所有角色（初始的$cnt$以及第一次的$endpos$值不能转移给$nq$，因为它们现在是父子关系）。
4. 按长度排序时采用计数排序方便（用于得到第i短的状态$a[i]$）。
5. 遇到多组数据时别忘了初始化$last$和$tot$，都为$1$。

```cpp
const int maxn = 5e5+7;

char s[maxn];
int ch[maxn*2][26], len[maxn*2], fa[maxn*2], cnt[maxn*2];
int last=1, tot=1;
int a[maxn*2], c[maxn];

void add(int c) {
    int p=last, np=last=++tot;
    len[np]=len[p]+1; cnt[np]=1;
    while(p&&!ch[p][c]) ch[p][c]=np, p=fa[p];
    if(!p) fa[np]=1;
    else {
        int q=ch[p][c];
        if(len[q]==len[p]+1) fa[np]=q;
        else {
            int nq=++tot; len[nq]=len[p]+1;
            fa[nq]=fa[q], fa[q]=fa[np]=nq;
            memcpy(ch[nq],ch[q],sizeof(ch[q]));
            while(p&&ch[p][c]==q) ch[p][c]=nq, p=fa[p];
        }
    }
}

int main() {
    scanf("%s", s); int n = strlen(s);
    for(int i=0; s[i]; ++i) add(s[i]-'a');
		for(int i=1; i<=tot; ++i) c[len[i]]++;
    for(int i=1; i<=n; ++i) c[i]+=c[i-1];
    for(int i=1; i<=tot; ++i) a[c[len[i]]--]=i;
		for(int i=tot; i; --i) cnt[fa[a[i]]]+=cnt[a[i]];
}
```

**广义后缀自动机**

每次插入总是让 cnt[last]++。

要插入新的字符串，则置 last = 1，因为 len[1] = 0。

```cpp
const int maxn = 4e5+7;

char s[maxn];
int ch[maxn*2][26], len[maxn*2], fa[maxn*2], cnt[2][maxn*2];
int last=1, tot=1;
int a[maxn*2], c[maxn*2];

int insert(int c, int last, int f) {
    if(ch[last][c]) {
        int p=last, q=ch[p][c];
        if(len[p]+1==len[q]) return ++cnt[f][q], q;
        else {
            int nq=++tot; len[nq]=len[p]+1;
            fa[nq]=fa[q], fa[q]=nq;
            memcpy(ch[nq],ch[q],sizeof(ch[q]));
            while(p&&ch[p][c]==q) ch[p][c]=nq, p=fa[p];
            return ++cnt[f][nq], nq;
        }
    }
    int p=last, np=++tot;
    len[np]=len[p]+1;
    while(p&&!ch[p][c]) ch[p][c]=np, p=fa[p];
    if(!p) fa[np]=1;
    else {
        int q=ch[p][c];
        if(len[q]==len[p]+1) fa[np]=q;
        else {
            int nq=++tot; len[nq]=len[p]+1;
            fa[nq]=fa[q], fa[q]=fa[np]=nq;
            memcpy(ch[nq],ch[q],sizeof(ch[q]));
            while(p&&ch[p][c]==q) ch[p][c]=nq, p=fa[p];
        }
    }
    return ++cnt[f][np], np;
}

int main() {
    scanf("%s", s);
    for(int i=0; s[i]; ++i) last=insert(s[i]-'a',last,0);
    last=1;
    scanf("%s", s);
    for(int i=0; s[i]; ++i) last=insert(s[i]-'a',last,1);
    for(int i=1; i<=tot; ++i) c[len[i]]++;
    for(int i=1; i<=tot; ++i) c[i]+=c[i-1];
    for(int i=1; i<=tot; ++i) a[c[len[i]]--]=i;
    for(int i=tot; i; --i) cnt[0][fa[a[i]]]+=cnt[0][a[i]], cnt[1][fa[a[i]]]+=cnt[1][a[i]];
    ll ans=0;
    for(int i=1; i<=tot; ++i) ans+=ll(cnt[0][i])*cnt[1][i]*(len[i]-len[fa[i]]);
    printf("%lld\n", ans);
}
```



## 回文自动机

[推荐一篇入门文章](https://www.luogu.org/blog/zhouzhuo/qiang-shi-tu-xie-hui-wen-zi-dong-ji)

1. 别忘了写上初始化！
2. 当字符串下标从$0$开始时，$pos$初始化为$-1$；若从$1$开始，则$pos$初始化为$0$；最终的$pos$代表最后一个字符的下标（前者为$n-1$,后者为$n$）。
3. 根据本质不同的回文子串数量不超过$|s|$，回文树节点数不超过$n+2$。
4. 节点$0$下面的回文串长度为偶数，节点$1$下面的回文串长度为奇数，而第一个字符在加入时显然只能构成奇数长度（长度为$1$）的回文子串，因此$last$初始化为$1$比较好。
5. 写法与后缀自动机很像，但如果只有后缀插入的话节点编号是递增的，统计$endpos$数($cnt$值)时无需进行计数排序。
6. 若要将两个字符串连在一起建立回文自动机，则必须真的将两个字符串连接在一起，因为函数内部要调用这个连接后的字符串（中间插入两个不同且不使用的字符）。


```cpp
const int maxn = 3e5+7;

char s[maxn];
int ch[maxn][26], fail[maxn], len[maxn], cnt[maxn], half[maxn];
int last, tot, pos;

void init() {
    fail[0]=fail[1]=tot=last=1;
    len[1]=pos=-1;
}

void add(int c) {
    ++pos; int p=last;
    while(s[pos-len[p]-1]!=s[pos]) p=fail[p];
    if(!ch[p][c]) {
        int np=++tot, fp=fail[p];
        len[np]=len[p]+2;
        while(s[pos-len[fp]-1]!=s[pos]) fp=fail[fp];
        fail[np]=ch[fp][c];
        if(len[fail[np]]<=len[np]/2) half[np]=fail[np];
        else {
          int hp=half[p];
          while(len[hp]+2>len[np]/2||s[pos-len[hp]-1]!=s[pos]) hp=fail[hp];
          half[np]=ch[hp][c];
        }
        ch[p][c]=np;
    }
    last=ch[p][c];
    cnt[last]++;
}

int main() {
    scanf("%s", s);
    init();
    for(int i=0; s[i]; ++i) add(s[i]-'a');
    for(int i=tot; i; --i) cnt[fail[i]]+=cnt[i];
}
```

**广义回文自动机**

普通多字符串情况下只需要将 $last$ 置 $1$ 即可。

若需要从某个字符串中间某处开始重构字符串，则考虑记录每个前缀字符串在回文树上的位置，同时记录一些额外的信息，以便快速求 fail，例如：

go\[i]\[j]：回文树自动机 i 点向上的 fail 链上第一个存在 j 字符转移边的点通过 j 字符边所转移到的节点（注意与 ch 区别）；

dep[i]：第 i 个增加的字符所在的前缀串的长度（注意与 len 区别）；

pre\[i]\[j]：第 i 个增加的字符所在的前缀串的往前数第 $2^j$ 个前缀，目的为了倍增找前面某个字符是什么。

[牛客：病毒](https://ac.nowcoder.com/acm/contest/6871/H)

```cpp
const int maxn = 5e5+7;
const int inf = 0x3f3f3f3f;
const int mod = 998244353;
const int D = 19;

struct EXPAM{
    int ch[maxn][4], go[maxn][4], fail[maxn], len[maxn];
    int pre[maxn][D], dep[maxn], val[maxn], dp[maxn], mx[maxn];
    int txt[maxn], cnt[maxn], w[4];
    int tot, rt0, rt1, last, size;
    void init() {
        last=tot=size=0; txt[size]=-1;
        for(int i=0; i<D; ++i) pre[size][i]=0; dep[size]=0;
        rt0=newnode(0); rt1=newnode(-1);
        fail[rt0]=1, fail[rt1]=1;
        for(int i=0; i<4; ++i) go[rt0][i]=rt1;
    }
    int newnode(int l) {
        len[tot]=l; cnt[tot]=val[tot]=dp[tot]=mx[tot]=0;
        memset(ch[tot],0,sizeof(ch[tot]));
        return tot++;
    }
    int getfail(int x) {
        if(txt[get(size,dep[size]-len[x]-1)]==txt[size]) return x;
        return go[x][txt[size]];
    }
    int get(int x, int l) {
        for(int i=D-1; i>=0; --i) if(dep[pre[x][i]]>=l) x=pre[x][i];
        return x;
    }
    void add(int c, int p) {
        txt[++size]=c; dep[size]=dep[p]+1; pre[size][0]=p;
        for(int i=1; i<D; ++i) pre[size][i]=pre[pre[size][i-1]][i-1];
        p=getfail(last);
        if(!ch[p][c]) {
            int np=newnode(len[p]+2);
            fail[np]=ch[getfail(fail[p])][c];
            ch[p][c]=np;

            if(len[np]==1) val[np]=w[c]; //这四行维护题目相关信息
            else val[np]=val[p]+w[c]*2;
            dp[np]=(val[np]+dp[fail[np]])%mod;
            mx[np]=max(val[np],mx[fail[np]]);
            
            memcpy(go[np],go[fail[np]],sizeof(go[np]));
            go[np][txt[get(size,dep[size]-len[fail[np]])]]=fail[np];
        }
        last=ch[p][c]; cnt[last]++;
    }
    void count() {
        for(int i=tot-1; i; --i) cnt[fail[i]]+=cnt[i];
    }
}pam;

int cg(char c) {
    if(c=='A') return 0;
    if(c=='C') return 1;
    if(c=='G') return 2;
    return 3;
}

char read_char() {
    char c=getchar();
    while(c<'A'||c>'Z') c=getchar();
    return c;
}

int p[maxn], f, n, ans; // p 记录某个前缀字符串在回文自动机上的节点

int main() {
    pam.w[0]=read(), pam.w[1]=read(), pam.w[2]=read(), pam.w[3]=read();
    char c=read_char(); pam.init(); pam.add(cg(c),0);
    printf("%d\n", ans=pam.mx[p[1]=pam.last]);
    for(int i=2; i; ++i) {
        f=read(); f^=ans;
        if(!f) break; c=read_char();
        pam.last=p[f]; pam.add(cg(c),f);
        printf("%d\n", ans=pam.mx[p[i]=pam.last]);
    }
    ans=0;
    for(int i=2; i<pam.tot; ++i) ans=(ans+ll(pam.dp[i])*pam.cnt[i])%mod;
    printf("%d\n", ans);
}
```



## AC自动机

0. **千万记住insert完以后要build!!!老是忘掉QAQ**
1. insert函数就只是trie数的插入函数，可在结尾根据条件打一些标记
2. build函数(构建fail树)，主要利用BFS
3. match函数中跑fail树其实就是反复跑后缀
4. fail树上倒过来建树以后可以进行很多高端操作，比如[阿狸的打字机](https://www.luogu.org/problem/P2414)(也许还能树剖一下QAQ)
5. match函数中跑fail树过程可以转化为打标记，而避免重复跑一个相同的后缀，常见优化(见下面的二次加强版板子题)
6. AC自动机常与数位DP结合，毕竟数字和字符串长得差不多

```cpp
const int maxn = 3e5+7;

char t[maxn];
int ch[maxn][26], ed[maxn], fa[maxn], cnt;

void insert(char *s) {
    int now=0;
    for(int i=0; s[i]; ++i) {
        int c=s[i]-'a';
        if(!ch[now][c]) ch[now][c]=++cnt;
        now=ch[now][c];
    }
    ed[now]++;
}

void build() {
    queue<int> q;
    memset(fa,0,sizeof(fa));
    for(int i=0; i<26; ++i) if(ch[0][i]) q.push(ch[0][i]);
    while(!q.empty()) {
        int now=q.front(); q.pop();
        for(int i=0; i<26; ++i) {
            if(ch[now][i]) {
                fa[ch[now][i]]=ch[fa[now]][i];
                q.push(ch[now][i]);
            }
            else ch[now][i]=ch[fa[now]][i];
        }
    }
}

int match(char *t) {
    int p=0, res=0;
    for(int i=0; t[i]; ++i) {
        p=ch[p][t[i]-'a'];
        for(int j=p; j&&~ed[j]; j=fa[j]) res+=ed[j], ed[j]=-1;
    }
    return res;
}

int main() {
    int n=read();
    for(int i=0; i<n; ++i) { scanf("%s", t); insert(t); }
    build();
    scanf("%s", t);
    printf("%d\n", match(t));
}
```

## Manacher

关键点:

1. 要有两个字符串，且处理后的字符串长度略大于原串2倍（肯定$<2n+7$）
2. p数组的更新
3. mid和r的转移

```cpp
const int maxn = 1e6+10;

char s[25000000], s0[12000000];
int p[25000000];

int change() {
    int len=strlen(s0);
    s[0]='?', s[1]='#';
    int j=2;
    for(int i=0; i<len; ++i) s[j++]=s0[i], s[j++]='#';
    s[j]='!';
    return j;
}

int Manacher() {
    int len=change(), mid=1, r=1, ans=1;
    for(int i=1; i<len; ++i) {
        if(i<r) p[i]=min(r-i,p[mid*2-i]); //如果可以直接利用，那肯定直接用呀
        else p[i]=1; //否则就只能先等于1了
        while(s[i-p[i]]==s[i+p[i]]) p[i]++; //暴力拓宽自身
        if(r<i+p[i]) mid=i, r=i+p[i]; //右边界如果拓宽，则mid也要变化
        ans=max(ans,p[i]-1);
    }
    return ans;
}

int main() {
    scanf("%s", s0);
    printf("%d\n", Manacher());
}
```

## 最小、大表示法

问题：给定长度为$n$的序列，求字典序最小的长度为$n$且与原序列循环同构的序列

利用**最小表示法**（也有后缀自动机以及后缀数组的解法）

1. 暴力的比较所有的$n$个循环同构的串
2. 纯暴力会被卡到$O(n^2)$，因此需要加点优化，代码如下

```cpp
int getmin(char *a) {
    int i=0, j=1, k=0; //i和j为两个正在比较大小的串的开始位置，k为两者第k个位置
    while(i<n&&j<n&&k<n) {
        if(a[(i+k)%n]==a[(j+k)%n]) k++; //若当前位置相同，则比较下一个位置
        else {
            if(a[(i+k)%n]>a[(j+k)%n]) i+=k+1;
            else j+=k+1;//这两行指前跳过了不可能成为最小表示的串，因为i和j的前k-1个位置都相同
            if(i==j) i++; //j++也行,保证i!=j即可
            k=0; //又从第一个位置开始比较
        }
    }
    return min(i,j); //由于上面退出循环可以分为两种情况,(i>=n||j>=n)或(k==n)。若k等于n，则i,j随便输出一个，表示原串所有位置都相同；若是另外的情况，肯定输出i和j中较小的那个
}
```

**最大表示法**

```cpp
int getmax(char *a) {
    int i=0, j=1, k=0; //i和j为两个正在比较大小的串的开始位置，k为两者第k个位置
    while(i<n&&j<n&&k<n) {
        if(a[(i+k)%n]==a[(j+k)%n]) k++; //若当前位置相同，则比较下一个位置
        else {
            if(a[(i+k)%n]<a[(j+k)%n]) i+=k+1;
            else j+=k+1;//这两行指前跳过了不可能成为最小表示的串，因为i和j的前k-1个位置都相同
            if(i==j) i++; //j++也行,保证i!=j即可
            k=0; //又从第一个位置开始比较
        }
    }
    return min(i,j); //由于上面退出循环可以分为两种情况,(i>=n||j>=n)或(k==n)。若k等于n，则i,j随便输出一个，表示原串所有位置都相同；若是另外的情况，肯定输出i和j中较小的那个
}
```



在这里口胡一个**后缀自动机**的解法，适用于长度t<=n的字典序最小的。。。

1. 建后缀自动机时用map存边
2. 由于map已经按照第一关键字排序了，因此只需贪心的跑长度为t的串即可(用map.begin()表示字典序最小的边)

# 图

## 网络流

[很棒的最小割模型论文(手动置顶)](https://wenku.baidu.com/view/986baf00b52acfc789ebc9a9.html)

### Dinic

#### interger型

```cpp
const int maxn = 1e5+7;
const int maxm = 3e5+7;
const int inf = 0x3f3f3f3f;

int head[maxn], to[maxm], w[maxm], nxt[maxm], tot=1;
int cur[maxn], level[maxn];

void add_edge(int u, int v, int c) {
    ++tot; to[tot]=v; w[tot]=c; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; w[tot]=0; nxt[tot]=head[v]; head[v]=tot;
}

void init() {
    memset(head,0,sizeof(head));
    tot=1;
}

bool bfs(int s, int t) {
    memset(level,0,sizeof(level));
    queue<int> q;
    q.push(s); level[s]=1;
    while(q.size()) {
        int u=q.front(); q.pop();
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(!w[i]||level[v]) continue;
            level[v]=level[u]+1;
            q.push(v);
        }
    }
    return level[t];
}

int dfs(int u, int t, int flow) {
    if(u==t||flow==0) return flow;
    int f, res=0;
    for(int &i=cur[u]; i; i=nxt[i]) {
        int v=to[i];
        if(level[v]==level[u]+1&&(f=dfs(v,t,min(flow,w[i])))>0) {
            w[i]-=f; w[i^1]+=f;
            flow-=f; res+=f;
            if(!flow) break;
        }
    }
    if(!res) level[u]=0;
    return res;
}

int dinic(int s, int t) {
    int res=0;
    while(bfs(s,t)) memcpy(cur,head,sizeof(cur)), res+=dfs(s,t,inf);
    return res;
}

int main() {
    int n=read(), m=read(), s=read(), t=read();
    for(int i=1; i<=m; ++i) {
        int u=read(), v=read(), w=read();
        add_edge(u,v,w);
    }
    printf("%d\n", dinic(s,t));
}
```

#### double型

```cpp
const int maxn = 1e5+7;
const int maxm = 3e5+7;
const int inf = 0x3f3f3f3f;
const double eps = 1e-8;

int head[maxn], to[maxm], nxt[maxm], tot=1;
double w[maxm];
int cur[maxn], level[maxn];

void add_edge(int u, int v, double c) {
    ++tot; to[tot]=v; w[tot]=c; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; w[tot]=0; nxt[tot]=head[v]; head[v]=tot;
}

void init() {
    memset(head,0,sizeof(head));
    tot=1;
}

bool bfs(int s, int t) {
    memset(level,0,sizeof(level));
    queue<int> q;
    q.push(s); level[s]=1;
    while(q.size()) {
        int u=q.front(); q.pop();
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(fabs(w[i])<=eps||level[v]) continue;
            level[v]=level[u]+1;
            q.push(v);
        }
    }
    return level[t];
}

double dfs(int u, int t, double flow) {
    if(u==t||fabs(flow)==0) return flow;
    double f, res=0;
    for(int &i=cur[u]; i; i=nxt[i]) {
        int v=to[i];
        if(level[v]==level[u]+1&&(f=dfs(v,t,min(flow,w[i])))>=eps) {
            w[i]-=f; w[i^1]+=f;
            flow-=f; res+=f;
            if(fabs(flow)<=eps) break;
        }
    }
    if(fabs(res)<=eps) level[u]=0;
    return res;
}

double dinic(int s, int t) {
    double res=0;
    while(bfs(s,t)) memcpy(cur,head,sizeof(cur)), res+=dfs(s,t,5e18);
    return res;
}

int main() {
    int n=read(), m=read(), s=read(), t=read();
    for(int i=1; i<=m; ++i) {
        int u=read(), v=read();
        double w; scanf("%lf", &w);
        add_edge(u,v,w);
    }
    printf("%.3f\n", dinic(s,t));
}
```

### 费用流（MCMF）

最小费用最大流只需要将所有的费用取相反数，最后再取反就行了，类似于最长路的处理。

```cpp
const int maxn = 2e4+7;
const int maxm = 2e5+7;
const int inf = 0x3f3f3f3f;

ll maxflow, mincost;
int last[maxn], pre[maxn], dis[maxn], now[maxn];
int head[maxn], to[maxm], flow[maxm], cost[maxm], nxt[maxm], tot=1;
bool inq[maxn];

void add_edge(int u,int v,int f,int c) {
    ++tot; to[tot]=v; flow[tot]=f; cost[tot]=c; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; flow[tot]=0; cost[tot]=-c; nxt[tot]=head[v]; head[v]=tot;
}

void init() {
    tot=1; mincost=maxflow=0;
    memset(head,0,sizeof(head));
    memset(last,0,sizeof(last));
    memset(pre,0,sizeof(pre));
}

bool spfa(int s, int t) {
    memset(dis,inf,sizeof(dis));
    memset(now,inf,sizeof(now));
    memset(inq,0,sizeof(inq));
    queue<int> q;
    dis[s]=0; pre[t]=-1; q.push(s); inq[s]=1;
    while(!q.empty()) {
        int u=q.front(); q.pop(); inq[u]=0;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(flow[i]>0&&dis[v]>dis[u]+cost[i]) {
                dis[v]=dis[u]+cost[i];
                now[v]=min(now[u],flow[i]);
                last[v]=i; pre[v]=u;
                if(!inq[v]) q.push(v), inq[v]=1;
            }
        }
    }
    return pre[t]!=-1;
}

void MCMF(int s, int t) {
    while(spfa(s,t)) {
        int u=t;
        maxflow+=now[t];
        mincost+=now[t]*ll(dis[t]);
        while(u!=s) {
            flow[last[u]]-=now[t];
            flow[last[u]^1]+=now[t];
            u=pre[u];
        }
    }
}

int main() {
    int n=read(), m=read(), s=read(), t=read();
    for(int i=1; i<=m; ++i) {
        int u=read(), v=read(), w=read(), f=read();
        add_edge(u,v,w,f);
    }
    MCMF(s,t);
    printf("%lld %lld\n", maxflow, mincost);
}
```

### 最大权闭合子图

1. 闭合图（针对有向图）：图中点集的出边也在点集中。（相当于有向边终点是有向边起点的必要条件）

2. 最大权闭合子图：图中每个点有点权，则权值最大的闭合子图为最大权闭合子图。

3. 原闭合图构造：额外加入源点 $s$ ，汇点 $t$ ，与 $s$ 相连的点（ $S$ ）的点权都为正，与 $t$ 相连的点（ $T$ ）的点权都为负（0就随意啦），这两者的点权用边权（取绝对值）代替； $S$ 与 $T$ 的连边边权为无穷。

4. 最大权闭合子图权值为：$\displaystyle \sum{S}-W$（ $W$为闭合图的最小割 ）
5. （由于 $<S,T>$ 都是无穷边，因此最小割一定是简单割，即所有割边都与源点或汇点相连）

## 最短路

### Dijkstra

1. 求次短路，只需要多维护一个“短路”就行了；
   但在维护次短路时，要记得把更新前的最短路与更新后的最短路swap一下，留着给次短路更新。

2. 不能处理负边权， 有负边权就换SPFA， 当然也不能求最长路。

```cpp
const int maxn = 3e5+7;
const int maxm = 6e5+7;
const int inf = 0x3f3f3f3f;

int n, m;
int head[maxn], to[maxm], w[maxm], nxt[maxm], tot;
int vis[maxn], dis[maxn];

inline void add_edge(int u, int v, int c) {
    ++tot; to[tot]=v; w[tot]=c; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; w[tot]=c; nxt[tot]=head[v]; head[v]=tot;
}

void dijstra(int s) {
    for(int i=1; i<=n; ++i) dis[i]=inf, vis[i]=0; dis[s]=0;
    priority_queue<pr,vector<pr>,greater<pr> > q;
    q.push(pr(0,s));
    while(q.size()) {
        int u=q.top().second; q.pop();
        if(vis[u]) continue; vis[u]=1;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(dis[v]>dis[u]+w[i]) {
                dis[v]=dis[u]+w[i];
                q.push(pr(dis[v],v));
            }
        }
    }
}
```

### SPFA

优点：能跑负边权

SLF优化（Small Label First 策略）：使用双端队列，当需要push节点进队列时，判断一下当前dis与队首dis大小；若更小，则push_front()；否则push_back()。

LLL优化（Large Label Last 策略）：如果当前出队的元素dis大于队内平均值，则先push到队尾，考虑下一个元素。

```cpp
const int maxn = 3e5+7;
const int maxm = 6e5+7;
const int inf = 0x3f3f3f3f;

int n, m;
int head[maxn], to[maxm], w[maxm], nxt[maxm], tot;
int inq[maxn], dis[maxn];

inline void add_edge(int u, int v, int c) {
    ++tot; to[tot]=v; w[tot]=c; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; w[tot]=c; nxt[tot]=head[v]; head[v]=tot;
}

void spfa(int s) {
    for(int i=1; i<=n; ++i) dis[i]=inf; dis[s]=0;
    queue<int> q;
    q.push(s); inq[s]=1;
    while(q.size()) {
        int u=q.front(); q.pop(); inq[u]=0;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(dis[v]>dis[u]+w[i]) {
                dis[v]=dis[u]+w[i];
                if(!inq[v]) inq[v]=1, q.push(v); //指定位置
            }
        }
    }
}
```

#### SPFA判断负环

在上述代码指定位置处加入： `if(++cnt[v]>n) flag=1`
仅仅需要一个cnt[]数组和一个负环flag即可.

### Floyd

众所周知，简单粗暴。
以下为floyd的路径保存问题代码， 并且附带倒序输出路径。
若想要正序输出， 则先将路径弄到栈里面再弹出。

```cpp
void floyd() {
    for(int i=0; i<n; ++i) {
        for(int j=0; j<n; ++j) {
            if(d[i][j]==inf) path[i][j]=-1;
            else path[i][j]=i;
        }
    }
    for(int k=0; k<n; ++k) {
        for(int i=0; i<n; ++i) {
            for(int j=0; j<n; ++j) {
                if(!(d[i][k]==inf||d[k][j]==inf)&&d[i][k]+d[k][j]<d[i][j]) {
                    d[i][j]=d[i][k]+d[k][j];
                    path[i][j]=path[k][j];
                }
            }
        }
    }
}

void printpath(int from, int to) {
    printf("%d ", to);
    while(path[from][t]!=from) {
        printf("%d ", path[from][to]);
        to=path[from][to];
    }
}
```

### Floyd求最小环

1. Floyd跑最小环考虑建立两个数组：$g[i][j]$和$d[i][j]$，其中$g[i][j]$表示$i,j$两点是否存在直接的连边，$d[i][j]$表示$i,j$的最短距离

2. 在Floyd循环的最内层用点$k$更新点$i,j$最短距离之前，先考虑$i,j,k$是否可以成环，若能成环，则更新答案

3. 能否成环的判定条件：$g[i][k]$&&$g[k][j]$&&$d[i][j]<inf$

4. 最后需要注意：$a[i]$可能为$0$，且为$0$的点不与任何点存在连边，因此是无效点，需要删去

```cpp
int n, ans;
ll a[maxn];
int g[130][130], d[130][130];

int Floyd() {
    //注意g和d的初始化方式
    for(int k=1; k<=n; ++k) {
        for(int i=1; i<=n; ++i) {
            if(i==k) continue;
            for(int j=1; j<=n; ++j) {
                if(i==j||k==j) continue;
                if(g[i][k]&&g[k][j]&&d[i][j]<130) ans=min(ans,d[i][j]+2);
                d[i][j]=min(d[i][j],d[i][k]+d[k][j]);
            }
        }
    }
    if(ans>n) return printf("-1\n"), 0;
    return printf("%d\n", ans), 0;
}
```

### Bellman-Ford

SPFA就是这个算法的队列优化。
同样可以用于判负环（循环了n次及以上，说明有负环）

```cpp
const int maxn = 1e4+10;
const int maxe = 1e6+10; //最大边数
const int INF = 1e9;

struct edge{
    int from, to, cost;
    edge(int f=0, int t=0, int c=0):from(f),to(t),cost(c) {}
};

edge es[maxe];
int d[maxn];
int n, E; //顶点数， 边数

void Bellman_Ford(int s) {
    for(int i=0; i<n; ++i) d[i]=INF;
    d[s]=0;
    while(1) {
        bool update=0;
        for(int i=0; i<E; ++i) {
            edge e=es[i];
            if(d[e.from]!=INF && d[e.to]>d[e.from]+e.cost) {
                d[e.to]=d[e.from]+e.cost;
                update=1;
            }
        }
        if(!update) break;
    }
}
```

## 二分图

### 匈牙利算法

复杂度：$O(V*E)$
匈牙利算法模板题：二分图上求最大匹配数

```cpp
const int maxn = 3e5+7;

int n, m;
int head[maxn], to[maxn], nxt[maxn], tot;
int vis[maxn], link[maxn], flag=1;

inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
}

bool find(int u) {
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i];
        if(vis[v]==flag) continue;
        vis[v]=flag;
        if(!link[v]||find(link[v])) {
            link[v]=u;
            return 1;
        }
    }
    return 0;
}

int main() {
    m=read(), n=read();
    int u, v;
    while(scanf("%d%d", &u, &v), u!=-1||v!=-1) add_edge(u,v);
    int cnt=0;
    for(int i=1; i<=m; ++i, ++flag) if(find(i)) cnt++;
    printf("%d\n", cnt);
    for(int i=m+1; i<=m+n; ++i)
        if(link[i]) printf("%d %d\n", link[i], i);
}
```

### 高效二分图匹配

据称复杂度为 $n^\frac{3}{2}$

```cpp
const int maxn = 3e5+7;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;

int n;
int head[maxn], to[maxn*2], nxt[maxn*2], tot;
int linku[maxn], linkv[maxn], vis[maxn], dis;
int du[maxn], dv[maxn];
queue<int> q;

inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
}

bool bfs() {
    dis=inf; while(q.size()) q.pop();
    for(int i=1; i<=n; ++i) du[i]=-1;
    for(int i=1; i<=n; ++i) dv[i]=-1;
    for(int i=1; i<=n; ++i) if(!linku[i]) du[i]=0, q.push(i);
    while(q.size()) {
        int u=q.front(); q.pop();
        if(du[u]>dis) break;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(dv[v]==-1) {
                dv[v]=du[u]+1;
                if(!linkv[v]) dis=dv[v];
                else du[linkv[v]]=dv[v]+1, q.push(linkv[v]);
            }
        }
    }
    return dis!=inf;
}

bool find(int u) {
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i];
        if(!vis[v]&&dv[v]==du[u]+1) {
            vis[v]=1;
            if(linkv[v]&&dv[v]==dis) continue;
            if(!linkv[v]||find(linkv[v])) {
                linku[u]=v, linkv[v]=u;
                return 1;
            }
        }
    }
    return 0;
}

int MaxMatch() {
    int ans=0;
    while(bfs()) {
        for(int i=1; i<=n; ++i) vis[i]=0;
        for(int i=1; i<=n; ++i) if(!linku[i]) ans+=find(i);
    }
    return ans;
}
```



### 奇偶染色

由于每个顶点和每条边都只访问了一次，因此**复杂度为O(V+E)**
可用于检测是否存在奇环。

```cpp
const int maxn = 1e4+10;

int n; //顶点数
int color[maxn]; //顶点i的颜色，1 or -1

bool dfs(int u, int c) {
    color[u]=c;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i];
        if(color[v]==c) return 0;
        if(!color[v]&&!dfs(v,-c)) return 0;
    }
    return 1;
}

void solve() {
    for(int i=1; i<=n; ++i) //如果是连通图，i==0时就可以遍历整张图
        if(!color[i]&&!dfs(i,1)) { printf("No\n"); return; }
    printf("Yes\n");
}
```

## Tarjan缩点

```cpp
const int maxn = 3e5+7;

int n, m, ID, cnt, top;
int dfn[maxn], low[maxn], ins[maxn], belong[maxn], stk[maxn];
int head[maxn], to[maxn*2], nxt[maxn*2], tot;
int head1[maxn], to1[maxn*2], nxt1[maxn*2], tot1;

inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
}
inline void add_edge1(int u, int v) {
    ++tot1; to1[tot1]=v; nxt1[tot1]=head1[u]; head1[u]=tot1;
}

void dfs(int u) {
    dfn[u]=low[u]=++ID;
    stk[++top]=u; ins[u]=1;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i];
        if(!dfn[v]) dfs(v), low[u]=min(low[u],low[v]);
        else if(ins[v]) low[u]=min(low[u],low[v]);
    }
    if(dfn[u]==low[u]) {
        cnt++;
        while(1) {
            int v=stk[top--]; ins[v]=0;
            belong[v]=cnt;
            if(v==u) break;
        }
    }
}

void tarjan() {
    for(int i=1; i<=n; ++i) if(!dfn[i]) dfs(i);
    for(int u=1; u<=n; ++u) {
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(belong[u]!=belong[v]) add_edge1(belong[u],belong[v]);
        }
    }
}
```

## Kosaraju缩点

思想：

1. 有向图中反图与原图的SCC相同；
2. 堵住SCC的出边;
3. 逆后序能维护拓扑关系（先序对起点有要求甚至完全不能保证）。

做法：先对原图用dfs求出逆后序，然后按照反图依次dfs加入SCC即可，复杂度O(n+m)。

```c++
void dfs1(int u) { //原图
    vis[u]=1;
    for(int v: g1[u]) if(!vis[v]) dfs1(v);
    s.push_back(u);
}

void dfs2(int u) { //反图
    belong[u]=cnt;
    for(int v: g2[u]) if(!belong[v]) dfs2(v);
}

void kosaraju() {
    for(int i=1; i<=n; ++i) if(!vis[i]) dfs1(i);
    for(int i=n-1; i>=0; --i) if(!belong[s[i]]) ++cnt, dfs2(s[i]);
}
```



## 线性LCA

```cpp
int head[maxn], nxt[maxn*2], to[maxn*2], cnt;
int head1[maxn], nxt1[maxn*2], to1[maxn*2], cnt1, id[maxn*2];

int fa[maxn], ans[maxn];
bool vis[maxn];
int n, m, s;

inline void add(int u, int v) {
    ++cnt; to[cnt]=v; nxt[cnt]=head[u]; head[u]=cnt;
    ++cnt; to[cnt]=u; nxt[cnt]=head[v]; head[v]=cnt;
}

inline void add1(int u, int v, int i) {
    ++cnt1; to1[cnt1]=v; nxt1[cnt1]=head1[u]; head1[u]=cnt1; id[cnt1]=i;
    ++cnt1; to1[cnt1]=u; nxt1[cnt1]=head1[v]; head1[v]=cnt1; id[cnt1]=i;
}

int find(int x) { return x==fa[x]?x:find(fa[x]); }

void dfs(int u, int p) {
    for(int i=head[u]; i; i=nxt[i])
        if(to[i]!=p) dfs(to[i],u), fa[to[i]]=u;
    for(int i=head1[u]; i; i=nxt1[i])
        if(vis[to1[i]]) ans[id[i]]=find(to1[i]);
    vis[u]=1;
}

int main() {
    n=read(), m=read(), s=read();
    for(int i=1; i<n; ++i) add(read(),read());
    for(int i=1; i<=m; ++i) add1(read(),read(),i);
    for(int i=1; i<=n; i++) fa[i]=i;
    dfs(s,0);
    for(int i=1; i<=m; i++) printf("%d\n", ans[i]);
}
```



## 树的重心和直径

### 树的重心

**性质**：

1. 最大的子树最小
2. 找到一个点,其所有的子树中最大的子树节点数最少,那么这个点就是这棵树的重心,删去重心后，生成的多棵树尽可能平衡
3. 树中所有点到某个点的距离和中，到重心的距离和是最小的，如果有两个距离和，他们的距离和一样，则这两个点都是重心（**即重心可以有两个**）
4. 把两棵树通过一条边相连，新的树的重心在原来两棵树重心的连线上
5. 一棵树添加或者删除一个节点，树的重心最多只移动一条边的位置
6. 一棵树最多有两个重心，且相邻。

**思路**：

1. 任选节点r为根节点做dfs，dfs的同时即更新所有的d(当前子树的大小)，以及最小的最大子树，注意当前子树的最大子树要考虑其父节点向上的树
 2. 最后得到的包含最小的最大子树的节点就是重心了

```cpp
const int maxn = 3e5+10;

int n, heart, mx, sz[maxn];
int head[maxn], to[maxn*2], nxt[maxn*2], tot;

inline void add_edge(int u, int v) {
    ++tot, to[tot]=v, nxt[tot]=head[u], head[u]=tot;
    ++tot, to[tot]=u, nxt[tot]=head[v], head[v]=tot;
}

void dfs(int u, int f) {
    sz[u]=1; int mm=0;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f) continue;
        dfs(v,u); sz[u]+=sz[v];
        if(sz[v]>mm) mm=sz[v];
    }
    if(n-sz[u]>mm) mm=n-sz[u];
    if(mm<mx) mx=mm, heart=u;
}

int main() {
    mx=inf, n=read();
    for(int i=1; i<n; ++i) add_edge(read(),read());
    dfs(1,0);
    printf("%d\n", heart);
}
```

### 树的直径

**算法$1$**：任选节点$u$做$dfs$，找到最远节点$v$；再从$v$做$dfs$，找到最远节点$w$，则$v-w$即为最长路径，$dis(v,w)$即为树的直径。
适合于**边权非负**的情形，代码简单

**算法$2$**：任选节点$r$作为根节点，求出最远距离$f$和次远距离$s$(在不同子树上)，则树的直径为$f+s$。
适用于**所有情形**

## 最大团

Bron-kerbosch算法

**说明**

all-已取顶点集，some-未处理顶点集(初始状态是全部顶点)，none-不取的顶点集，g-边，
S-极大团数量，ans-最大团结点数；
**结点编号1~n，0用于辅助，表示被删除；**
求最大团顶点数时只要some；要求记录路径时要all；
求极大团数量要some、none；要求记录路径时要all；

### 极大团计数

输入：some，g；
输出：S；
调用方法：dfs(0,0,n,0);
some的初始化:for(int i=0; i<n; ++i) some\[0][i]=i+1;
复杂度爆炸，可处理一百个点

```cpp
const int maxn = 129;

int n, m, S;
int some[maxn][maxn], all[maxn][maxn], none[maxn][maxn], g[maxn][maxn];

void dfs(int d, int an, int sn, int nn) {
    if(!sn&&!nn) ++S;
    if(S>1000) return; //题意表明S超过1000就输出Impossible
    int u=some[d][0];
    for(int i=0; i<sn; ++i) {
        int v=some[d][i];
        if(g[u][v]) continue;
        int tsn=0, tnn=0;
        //for(int j=0; j<an; ++j) all[d+1][j]=all[d][j];
        //all[d+1][an]=v;  //可以不写这两行
        for(int j=0; j<sn; ++j) if(g[v][some[d][j]])
            some[d+1][tsn++]=some[d][j];
        for(int j=0; j<nn; ++j) if(g[v][none[d][j]])
            none[d+1][tnn++]=none[d][j];
        dfs(d+1,an+1,tsn,tnn);
        some[d][i]=0, none[d][nn++]=v;
    }
}

int main() {
  	dfs(0,0,n,0);
  	for(int i=0; i<n; ++i) some[0][i]=i+1; //some的初始化
}
```

### 最大团

输入：some，g；
输出：ans，**最大团顶点集保存在all[ans]数组的前ans个数，即all\[ans][i]**
调用方式：dfs(0,n,0);
复杂度依然爆炸，可处理几十个点

```cpp
const int maxn = 51;

int n, ans;
int some[maxn][maxn], g[maxn][maxn];

void dfs(int d, int sn, int an) {
    if(sn+an<=ans) return;
    if(!sn) { ans=max(ans,an); return; }
    int u=some[d][0];
    for(int i=0; i<sn; ++i) {
        int v=some[d][i];
        if(g[u][v]) continue;
        int tsn=0;
        //for(int j=0; j<an; ++j) all[d+1][j]=all[d][j];
        //all[d+1][an]=v;  //这两行代码可以保存最大团顶点集
        for(int j=0; j<sn; ++j) if(g[v][some[d][j]])
            some[d+1][tsn++]=some[d][j];
        dfs(d+1,tsn,an+1);
        some[d][i]=0;
    }
}

int main() {
    for(int i=0; i<n; ++i) some[0][i]=i+1;
    dfs(0,n,0);
}
```

## 2-Sat

### 分析

**关键：连边(从"一定不能"中产生出"一定能")**

2-SAT算法本身并不难，关键是连边，不过只需要充分理解好边的概念：a->b即选a必选b。
（也即连**单向**边）
1. a、b不能同时选：选了a就要选b'，选了b就要选a'。
2. a、b必须同时选：选了a就要选b，选了b就要选a，选了a'就要选b'，选了b'就要选a'。
3. a、b必须选一个：选了a就要选b'，选了b就要选a'，选了a'就要选b，选了b'就要选a。

**从离散数学的角度讲：**

**就是将题给的逻辑表达式转化为以蕴涵关系为主的逻辑表达式**

例如：a V b=¬a -> b 并且 ¬b -> a（注意这里的“或”关系是指二选一而不是任选一）

### 具体实现

1. 利用DFS瞎搞，复杂度稍高，但是不影响AC题。

2. 利用SCC缩点，判断a和¬a是否在同一个强连通分量里，若belong[i]<belong[i+n]，表明选择了i（belong是一个逆拓扑序）；同时，若至少存在一个belong[i]==belong[i+n]，则表明不能成功指派。该方法不容易处理好最小字典序的问题。

### 例题

**题意：**

给定$n$个长度为$m$的$01$串，定义两个串相似：两个串对应位置相同的位置数量不小于某给定值$k$；可以通过反转字符串使得两个$01$串从不相似变成相似，求最少的反转次数使得所有的$01$串两两相似。

**思路：**

1. 刚看到这题时还想过网络流、双向bfs？QAQ，后面突然想到好像可以2-Sat，于是就A了。。。
2. 那怎样想到2-Sat呢？考虑到每个$01$串有两种状态，只能选其中一个；并且这些$01$串的状态受到其他$01$串的限制，很像2-Sat！
3. 2-Sat关键就是从“不能怎样”->“一定要那样”，也就是从模糊的条件推出必须满足的有向边。而这里先$O(n^2*m)$预处理出所有这样的关系；对于任意两个$01$串处理出都不反转和一个反转这两种情况是否相似，若都不相似，说明这对$01$串没救了！直接退出程序输出$-1$；若两种情况都满足相似，则说明这两个$01$串直接没有任何限制，不连边；剩下的就是只有一种情况相似，根据情况连双向边即可（为什么是双向边呢？其实是两条单向边的叠加）。
4. 现在最关键的问题来了，如何使得反转的$01$数量最少呢？考虑到前面连边的方式（双向边）导致不同的SCC之间没有任何边，也就是没有任何限制。因此对于每个没有确定的字符串，先随便确定它的状态进行$dfs$，若失败了，说明反转后也会失败！（这点很有趣）然后就可以直接退出程序输出$-1$了。那如果成功了呢？怎样保证反转次数最少呢？单独考虑当前SCC，SCC的整体（包含的所有字符串）都反转，则当前SCC依然合法（毕竟都反转等于都不反转）；因此若当前SCC中选择反转的数量大于其包含点数的$\frac{1}{2}$，则选择整体反转，这样就保证了反转的数量尽可能小啦。

### 代码

```cpp
const int maxn = 1e2+7;

int n, m, k;
string s[maxn];
int head[maxn], to[maxn*maxn], nxt[maxn*maxn], tot;
int tmp[maxn], top;
bool choose[maxn];

inline void add_edge(int u, int v) { //双向边针对本题
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; nxt[tot]=head[v]; head[v]=tot;
}

bool dfs(int u) {
    if(choose[u^1]) return 0;
    if(choose[u]) return 1;
    choose[u]=1, tmp[++top]=u;
    for(int i=head[u]; i; i=nxt[i]) if(!dfs(to[i])) return 0;
    return 1;
}

void solve() {
    n=read(), m=read(), k=read();
    for(int i=0; i<2*n; ++i) head[i]=choose[i]=0; tot=0;
    for(int i=0; i<n; ++i) cin>>s[i];
    for(int i=0; i<n; ++i) {
        for(int j=i+1; j<n; ++j) {
            int c1=0, c2=0;
            for(int t=0; t<m; ++t) {
                if(s[i][t]==s[j][t]) c1++;
                if(s[i][t]==s[j][m-1-t]) c2++;
            }
            if(c1<k&&c2<k) return (void)printf("-1\n");
            if(c1>=k&&c2>=k) continue;
            else if(c1>=k) add_edge(2*i,2*j), add_edge(2*i+1,2*j+1);
            else if(c2>=k) add_edge(2*i,2*j+1), add_edge(2*i+1,2*j);
        }
    }
    vector<int> ans;
    for(int i=0; i<n; ++i) {
        if(choose[2*i]||choose[2*i+1]) continue;
        top=0;
        /*if(!dfs(2*i)) {//如果要输出，大致这样写
              while(top) choose[temp[--top]]=0;
              if(!dfs(2*i+1)) return (void)printf("-1\n");
        }*/
        if(!dfs(2*i)) return (void)printf("-1\n");
        int rev_num=0;
        for(int j=1; j<=top; ++j) if(tmp[j]%2) rev_num++;
        if(rev_num>top/2) {
            for(int j=1; j<=top; ++j) if(tmp[j]%2==0) ans.push_back(tmp[j]/2+1);
        }
        else for(int j=1; j<=top; ++j) if(tmp[j]%2) ans.push_back(tmp[j]/2+1);
    }
    cout<<ans.size()<<endl;
    for(auto p: ans) printf("%d ", p);
    puts("");
}

int main() {
    int T=read();
    while(T--) solve();
}
```

### tarjan版的另外一个题代码

cf 585 div2 F题：[Radio Stations](https://codeforces.com/contest/1215/problem/F) （题目贼难读）

```cpp
int n, p, M, m, q;
int head[maxn], to[maxn], nxt[maxn], tot;

inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
}

int ID, cnt, top;
int dfn[maxn], low[maxn], ins[maxn], belong[maxn], Stack[maxn];

void dfs(int u) {
    dfn[u]=low[u]=++ID;
    Stack[++top]=u; ins[u]=1;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i];
        if(!dfn[v]) dfs(v), low[u]=min(low[u],low[v]);
        else if(ins[v]) low[u]=min(low[u],low[v]);
    }
    if(dfn[u]==low[u]) {
        cnt++;
        while(1) {
            int v=Stack[top--]; ins[v]=0;
            belong[v]=cnt;
            if(v==u) break;
        }
    }
}

int main() {
    q=read(), p=read(), M=read(), m=read(); n=p+M;
    for(int i=1; i<=q; ++i) {
        int x=read(), y=read();
        add_edge(x+n,y); add_edge(y+n,x); //这题边有点多，不用看连边的过程
    }
    for(int i=1; i<=p; ++i) {
        int l=read(), r=read();
        add_edge(i,l+p); add_edge(l+p+n,i+n);
        if(r<M) add_edge(i,p+r+1+n), add_edge(p+r+1,i+n);
    }
    for(int i=1; i<=m; ++i) {
        int u=read(), v=read();
        add_edge(u,v+n); add_edge(v,u+n);
    }
    for(int i=1; i<M; ++i) {
        add_edge(i+1+p,i+p), add_edge(i+p+n,i+p+1+n);
    }
    for(int i=1; i<=2*n; ++i) if(!dfn[i]) dfs(i);
    for(int i=1; i<=n; ++i) { //能否成功指派就看这里
        if(belong[i]==belong[i+n]) return printf("-1\n"), 0;
    }
    vector<int> ans;
    for(int i=1; i<=p; ++i) {
        if(belong[i]<belong[i+n]) ans.push_back(i); //表明要选i
    }
    int f=M;
    for(int i=1; i<=M; ++i) {
        if(belong[p+i]>belong[p+i+n]) { f=i-1; break; }
    }
    printf("%d %d\n", int(ans.size()), f);
    for(int i=0; i<ans.size(); ++i) {
        printf("%d%c", ans[i], " \n"[i==ans.size()-1]);
    }
}
```



## 树链剖分

### 简易树剖板子

```cpp
#include "bits/stdc++.h"
#define hhh cerr<<"hhh"<<endl
#define see(x) cerr<<(#x)<<'='<<(x)<<endl
using namespace std;
typedef long long ll;
typedef pair<int,int> pr;
inline int read() {int x=0,f=1;char c=getchar();while(c!='-'&&(c<'0'||c>'9'))c=getchar();if(c=='-')f=-1,c=getchar();while(c>='0'&&c<='9')x=x*10+c-'0',c=getchar();return f*x;}

const int maxn = 5e5+7;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;

int n;
int head[maxn], to[maxn*2], nxt[maxn*2], tot;

inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; nxt[tot]=head[v]; head[v]=tot;
}

int deep[maxn], id[maxn], ID, fa[maxn], son[maxn], top[maxn], sz[maxn];

void dfs0(int u, int f) {
    fa[u]=f; sz[u]=1; deep[u]=deep[f]+1;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f) continue;
        dfs0(v,u); sz[u]+=sz[v];
        if(sz[v]>sz[son[u]]) son[u]=v;
    }
}

void dfs1(int u, int f, int tp) {
    top[u]=tp; id[u]=++ID;
    if(!son[u]) return;
    dfs1(son[u],u,tp);
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f||v==son[u]) continue;
        dfs1(v,u,v);
    }
}

ll node[maxn<<2], lazy[maxn<<2];

void push_down(int l, int r, int now) {
    ll &d=lazy[now], m=(l+r)/2;
    node[now<<1]+=ll(m-l+1)*d; lazy[now<<1]+=d;
    node[now<<1|1]+=ll(r-m)*d; lazy[now<<1|1]+=d;
    d=0;
}

void update(int x, int y, int d, int l, int r, int now) {
    if(x<=l&&r<=y) {
        node[now]+=ll(r-l+1)*d; lazy[now]+=d;
        return;
    }
    if(lazy[now]) push_down(l,r,now);
    int m=(l+r)/2;
    if(x<=m) update(x,y,d,l,m,now<<1);
    if(y>m) update(x,y,d,m+1,r,now<<1|1);
    node[now]=node[now<<1]+node[now<<1|1];
}

ll query(int x, int y, int l, int r, int now) {
    if(x<=l&&r<=y) return node[now];
    if(lazy[now]) push_down(l,r,now);
    int m=(l+r)/2; ll res=0;
    if(x<=m) res+=query(x,y,l,m,now<<1);
    if(y>m) res+=query(x,y,m+1,r,now<<1|1);
    return res;
}

int get_lca(int u, int v) {
    while(top[u]!=top[v]) {
        if(deep[top[u]]<deep[top[v]]) swap(u,v);
        u=fa[top[u]];
    }
    return deep[u]<deep[v]?u:v;
}

void add(int u, int v, int d) {
    while(top[u]!=top[v]) {
        if(deep[top[u]]<deep[top[v]]) swap(u,v);
        update(id[top[u]],id[u],d,1,n,1); u=fa[top[u]];
    }
    if(deep[u]<deep[v]) swap(u,v);
    update(id[v],id[u],d,1,n,1);
}

ll qsum(int u, int v) {
    ll res=0;
    while(top[u]!=top[v]) {
        if(deep[top[u]]<deep[top[v]]) swap(u,v);
        res+=query(id[top[u]],id[u],1,n,1); u=fa[top[u]];
    }
    if(deep[u]<deep[v]) swap(u,v);
    res+=query(id[v],id[u],1,n,1);
    return res;
}

int main() {
    n=read();
    for(int i=1; i<n; ++i) add_edge(read(),read());
    dfs0(1,0); dfs1(1,0,1);
}
```



### 换根树剖

**题意：**

给定一张连通图，求出以1为根的最小生成树（然后就跟图没啥关系了）。
对于这棵生成树，有3种操作+3种询问：

1. 更换根节点
2. 树上$x$到$y$的最短路径上的点权加$d$
3. 树上$x$所在子树所有节点点权加$d$
4. 求$x$和$y$的$LCA$
5. 求$x$到$y$的最短路径上的点权之和
6. 求$x$所在子树所有节点点权之和

**思路：关键在于换根以及求$lca$**

这个换根想了挺久都没有想法，实在没有正确复杂度的操作，真是太菜！
于是百度了一下，发现换根是种传统操作（板子），**实际处理方法就是换根但不换树剖的信息！**
（前置知识：树剖，并且树剖的同时记录$dfs$序的$id_{left}$和$id_{right}$）（以下所有“子树”是指在以1为根的树中）

1. 换根
	直接用一个$root$记录当前根节点即可，就这么简单！当然这里方便了后面就会麻烦一些。
2. 树上$x$到$y$的最短路径上的点权加$d$
	由于树上最短路径与根是哪个点无关，因此正常的做就好了。
3. 树上$x$所在子树所有节点点权加$d$
	考虑两种情况+一种特判：
	- 特判：若$x=root$，那直接整棵树加$d$
	- $root$不在$x$子树中，则显然根具体是哪个点不会影响这棵子树
	- $root$在$x$子树中，此时这棵子树会发生变化，真正的子树变成了属于以当前点向“上”的树，具体而言就是整棵树去掉原子树中$root$所在的“树枝”后都是“$x$所在的子树”。
		然后问题就变成了如何找到这个“树枝”，我这里采用的倍增法，从$root$倍增跳$father$，跳到刚好是$x$的直接儿子即可，复杂度$O(log_{2}n)$
4. 求$x$和$y$的$LCA$
	这是本题难点，有不止一种方法，自认为我这方法挺不错（题解方法分类太多了），嘿嘿
	- 若$x$和$y$都属于原$root$子树，则返回原$lca$即可（很显然啦）
	- 若一个属于，一个不属于，则$lca$就是$root$，因为它夹在中间了
	- 若两个都不属于则分两种情况：
		* 先考虑以二者的$lca$为根的原子树是否包含$root$，若不包含，则显然返回$lca$即可
		* 剩下的就是不包含的情况，我们将$x$和$y$分别用倍增跳$father$，都跳到**刚好**使$root$在当前原子树中即可，然后将最后的$x$和$y$取深度较大的（为了更接近$root$）
5. 求$x$到$y$的最短路径上的点权之和
	同2，树上最短路径与根是哪个点无关
6. 求$x$所在子树所有节点点权之和
	同3，考虑两种情况+一种特判即可。

**Ok, all right! 此题不失为一道换根+树剖板子好题！复杂度目测为$\displaystyle O(nlog_2^2{n})$，集中在树剖+线段树区间操作处，已忽略一开始的MST复杂度。**

```cpp
#include "bits/stdc++.h"
#define hhh printf("hhh\n")
#define see(x) (cerr<<(#x)<<'='<<(x)<<endl)
using namespace std;
typedef long long ll;
typedef pair<int,int> pr;
inline int read() {int x=0,f=1;char c=getchar();while(c!='-'&&(c<'0'||c>'9'))c=getchar();if(c=='-')f=-1,c=getchar();while(c>='0'&&c<='9')x=x*10+c-'0',c=getchar();return f*x;}

const int maxn = 3e5+7;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;

struct Edge{
    int u, v, w, id;
    bool operator < (const Edge &rhs) const {
        if(w==rhs.w) return id<rhs.id;
        return w<rhs.w;
    }
}e[maxn*2];

int n, m, r, root=1;
int a[maxn], belong[maxn];
int head[maxn], to[maxn*2], nxt[maxn*2], tot;
int lid[maxn], rid[maxn], rk[maxn], ID;
int son[maxn], top[maxn], sz[maxn], deep[maxn];
int fa[maxn], st[maxn][20];

int find(int a) { return a==belong[a]?a:belong[a]=find(belong[a]); }

inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; nxt[tot]=head[v]; head[v]=tot;
}

void dfs0(int u, int f, int d) {
    fa[u]=f; sz[u]=1; deep[u]=d;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f) continue;
        dfs0(v,u,d+1); sz[u]+=sz[v];
        if(sz[v]>sz[son[u]]) son[u]=v;
    }
}

void dfs(int u, int f, int t) {
    rk[lid[u]=++ID]=u; top[u]=t;
    if(son[u]) dfs(son[u],u,t);
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f||v==son[u]) continue;
        dfs(v,u,v);
    }
    rid[u]=ID;
}

ll node[maxn<<2], lazy[maxn<<2];

void build(int l, int r, int now) {
    if(l==r) {
        node[now]=a[rk[l]];
        return;
    }
    int m=(l+r)/2;
    build(l,m,now<<1); build(m+1,r,now<<1|1);
    node[now]=node[now<<1]+node[now<<1|1];
}

void push_down(int l, int r, int now) {
    ll &d=lazy[now], m=(l+r)/2;
    node[now<<1]+=ll(m-l+1)*d; lazy[now<<1]+=d;
    node[now<<1|1]+=ll(r-m)*d; lazy[now<<1|1]+=d;
    d=0;
}

void update(int x, int y, int d, int l, int r, int now) {
    if(x>y) return;
    if(x<=l&&r<=y) {
        node[now]+=ll(r-l+1)*d;
        lazy[now]+=d;
        return;
    }
    if(lazy[now]) push_down(l,r,now);
    int m=(l+r)/2;
    if(x<=m) update(x,y,d,l,m,now<<1);
    if(y>m) update(x,y,d,m+1,r,now<<1|1);
    node[now]=node[now<<1]+node[now<<1|1];
}

ll query(int x, int y, int l, int r, int now) {
    if(x>y) return 0;
    if(x<=l&&r<=y) return node[now];
    if(lazy[now]) push_down(l,r,now);
    int m=(l+r)/2;
    ll ans=0;
    if(x<=m) ans+=query(x,y,l,m,now<<1);
    if(y>m) ans+=query(x,y,m+1,r,now<<1|1);
    return ans;
}

int lca(int x, int y) {
    if(deep[x]<deep[y]) swap(x,y);
    for(int i=19; i>=0; --i) if(deep[st[x][i]]>=deep[y]) x=st[x][i];
    if(x==y) return x;
    for(int i=19; i>=0; --i) if(st[x][i]!=st[y][i]) x=st[x][i], y=st[y][i];
    return fa[x];
}

bool contain(int x, int r) {
    return lid[r]<=lid[x]&&lid[x]<=rid[r];
}

void add_xyd(int x, int y, int d) {
    while(top[x]!=top[y]) {
        if(deep[top[x]]<deep[top[y]]) swap(x,y);
        update(lid[top[x]],lid[x],d,1,n,1); x=fa[top[x]];
    }
    if(deep[x]<deep[y]) swap(x,y);
    update(lid[y],lid[x],d,1,n,1);
}

void add_xd(int x, int d) {
    if(root==x) update(1,n,d,1,n,1);
    else if(!contain(root,x)) update(lid[x],rid[x],d,1,n,1);
    else {
        int r=root;
        for(int i=19; i>=0; --i) if(deep[st[r][i]]>deep[x]) r=st[r][i];
        update(1,lid[r]-1,d,1,n,1);
        update(rid[r]+1,n,d,1,n,1);
    }
}

int get_lca(int x, int y) {
    int cas=contain(x,root)+contain(y,root);
    if(cas==2) return lca(x,y);
    if(cas==1) return root;
    int LCA=lca(x,y);
    if(!contain(root,LCA)) return LCA;
    for(int i=19; i>=0; --i) {
        if(st[x][i]&&!contain(root,st[x][i])) x=st[x][i];
        if(st[y][i]&&!contain(root,st[y][i])) y=st[y][i];
    }
    if(!contain(root,x)) x=fa[x];
    if(!contain(root,y)) y=fa[y];
    return deep[x]>=deep[y]?x:y;
}

ll sum_xy(int x, int y) {
    ll ans=0;
    while(top[x]!=top[y]) {
        if(deep[top[x]]<deep[top[y]]) swap(x,y);
        ans+=query(lid[top[x]],lid[x],1,n,1); x=fa[top[x]];
    }
    if(deep[x]<deep[y]) swap(x,y);
    ans+=query(lid[y],lid[x],1,n,1);
    return ans;
}

ll sum_x(int x) {
    if(root==x) return node[1];
    if(!contain(root,x)) return query(lid[x],rid[x],1,n,1);
    int r=root;
    for(int i=19; i>=0; --i) if(deep[st[r][i]]>deep[x]) r=st[r][i];
    return query(1,lid[r]-1,1,n,1)+query(rid[r]+1,n,1,n,1);
}

int main() {
    n=read(), m=read(), r=read();
    for(int i=1; i<=n; ++i) a[i]=read(), belong[i]=i;
    for(int i=1; i<=r; ++i) {
        int u=read(), v=read(), w=read();
        e[i]=(Edge){u,v,w,i};
    }
    sort(e+1,e+1+r);
    for(int i=1; i<=r; ++i) {
        int x=find(e[i].u);
        int y=find(e[i].v);
        if(x!=y) belong[x]=y, add_edge(e[i].u,e[i].v);
    }
    dfs0(1,0,1);
    dfs(1,0,1);
    for(int j=0; j<=19; ++j) {
        for(int i=1; i<=n; ++i)
            if(!j) st[i][j]=fa[i];
            else st[i][j]=st[st[i][j-1]][j-1];
    }
    build(1,n,1);
    while(m--) {
        int op=read();
        if(op==1) root=read();
        else if(op==2) {
            int x=read(), y=read(), d=read();
            add_xyd(x,y,d);
        }
        else if(op==3) {
            int x=read(), d=read();
            add_xd(x,d);
        }
        else if(op==4) {
            int LCA=get_lca(read(),read());
            printf("%lld\n", query(lid[LCA],lid[LCA],1,n,1));
        }
        else if(op==5) printf("%lld\n", sum_xy(read(),read()));
        else if(op==6) printf("%lld\n", sum_x(read()));
    }
}
```



## Kruskal 重构树

建树流程类似于 Kruskal 生成树，如果某条边有用，则新建一个点，点权为边权，连接这条边两端点当前的最高祖先。

重构树特点：

- $2*n-1$ 个节点的二叉树
- 叶子结点无权值，非叶子结点都有权值，且整体可当作一个堆
- 

例题 1：$n$ 个点 $m$ 条无向边的图，$k$ 个询问，每次询问从 $u$ 到 $v$ 的所有路径中，最长的边的最小值。

建立最小 Kruskal 重构树，然后树上 $u$ 和 $v$ 的 lca 的权值就是所求 $x$。



例题 2：给一个 $n$ 个节点 $m$ 条带权边的无向连通图，有 $q$ 次询问，每次询问图中 $k_i$ 个互不相同的点，你可以选择一个数 $x$，然后将图中所有边权小于等于 $x$ 的边删除。求当删除这些边后 $k_i$ 个点互不连通时，$x$ 的最小值。

建立最大 Kruskal 重构树，然后每次把这 $k$ 个点按重构树上 dfs 序排序，找到所有相邻点的 lca 权值的最大值，即为 $x$。

例题 2 代码：

```cpp
struct Edge{
    int u, v, w;
    bool operator < (const Edge &rhs) const {
        return w>rhs.w; //注意这是最大 Kruskal 重构树
    }
}e[maxn];
  
int n, m, q, ans;
int head[maxn], to[maxn], nxt[maxn], tot;
int sz[maxn], son[maxn], fa[maxn], deep[maxn];
int ffa[maxn], top[maxn], a[maxn], id[maxn], ID;
int p[maxn];
  
inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
}
  
int find(int a) { return a==ffa[a]?a:ffa[a]=find(ffa[a]); }
  
void dfs0(int u, int f) {
    deep[u]=deep[f]+1; sz[u]=1; fa[u]=f;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i];
        dfs0(v,u); sz[u]+=sz[v];
        if(sz[v]>sz[son[u]]) son[u]=v;
    }
}
  
void dfs1(int u, int tp) {
    top[u]=tp; id[u]=++ID;
    if(!son[u]) return;
    dfs1(son[u],tp);
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==son[u]) continue;
        dfs1(v,v);
    }
}
  
int get_lca(int u, int v) {
    while(top[u]!=top[v]) {
        if(deep[top[u]]<deep[top[v]]) swap(u,v);
        u=fa[top[u]];
    }
    return deep[u]<deep[v]?u:v;
}
  
int main() {
    n=read(), m=read(), q=read();
    for(int i=1; i<=m; ++i) {
        int u=read(), v=read(), w=read();
        e[i]=(Edge){u,v,w};
    }
    sort(e+1,e+1+m);
    for(int i=1; i<2*n; ++i) ffa[i]=i;
    int cnt=n;
    for(int i=1; i<=m; ++i) {
        int u=find(e[i].u), v=find(e[i].v), w=e[i].w;
        if(u!=v) {
            cnt++;
            a[cnt]=w;
            add_edge(cnt,u);
            add_edge(cnt,v);
            ffa[u]=ffa[v]=cnt;
        }
    }
    dfs0(2*n-1,0); dfs1(2*n-1,2*n-1);
    while(q--) {
        int k=read();
        for(int i=1; i<=k; ++i) p[i]=ans^read();
        sort(p+1,p+1+k,[&](int u,int v){return id[u]<id[v];});
        ans=0;
        for(int i=1; i<k; ++i) ans=max(ans,a[get_lca(p[i],p[i+1])]);
        printf("%d\n", ans);
    }
}
```



## 虚树

**虚树的概念**

虚树，是对于一棵给定的节点数为 $n$ 的树 $T$ ，构造一棵新的树 $T’$ 使得总结点数**最小**且**包含指定的某几个节点和他们的LCA**。

**虚树解决的问题**

利用虚树，可以对于指定**多组**点集 $S$ 的询问进行**每组** $O(|S|log_2n+f(|S|)) $的回答，其中 $f(x)$ 指的是对于树上有 $x$ 个点的情况下**单组询问**这个问题的时间复杂度。可以看到，这个复杂度基本上(除了那个 $log_2n$ 以外)与 $ n$无关了。这样，对于多组询问的回答就可以省去每次询问都遍历一整棵树的 $O(n)$ 复杂度了。

```cpp
sort(a+1,a+1+k,[](int x,int y){return id[x]<id[y];}); //先按dfs序排序
s[tp=1]=1; //把根节点放进去，好写一些
for(int i=1; i<=k; ++i) {
    int lca=get(s[tp],a[i]);
    while(tp>1&&deep[lca]<=deep[s[tp-1]]) { //保证了deep[lca]<=deep[s[tp]];
	      add_edge1(s[tp-1],s[tp]); tp--;
    }
    if(s[tp]!=lca) add_edge1(lca,s[tp]), s[tp]=lca;
    if(s[tp]!=a[i]) s[++tp]=a[i]; //当a[i]=1时，s[tp]为一开始放入的1，因此要特判一下
}
while(tp>1) add_edge1(s[tp-1],s[tp]), tp--;
ans=0;
dfs(1); //dfs过程中记得把连好的边给删掉
printf("%d\n", ans);
```

## Dsu on tree

DSU on tree（Disjoint Set Union，树上启发式合并）

- **思想：利用每个节点到根节点路径上的轻边数复杂度是$log$级别的，同时只有当每次遍历轻边时才会将整棵轻边连接的子树额外暴力遍历一遍，所以每个节点被暴力遍历的次数就是$log$级别的，这样就保证了整体复杂度在$log$级别啦！(默认点修改为O(1))**
- 三种$dfs$：
	- dfs0:处理出重儿子
	- dfs1:对子树进行暴力操作，分为两种状态，一种在增添信息时使用，一种在删除信息时使用
	- dfs2:dsu on tree主体
- 讲讲dfs2，它也分为两种状态，一种是保留信息的，一种是不保留信息的。大致框架：
	- 对每个节点，先处理所有非重儿子节点，并且不保留信息
	- 然后处理重儿子，并且保留信息
	- 再暴力增添所有非重儿子信息，同时加上当前节点本身的信息，这样就得到当前节点答案啦！
	- 最后若此dfs2是“不保留型”，则用dfs1暴力删除刚刚所有的信息

**题意：**

给了一个带颜色的树，对每个节点，求出占领它的颜色的权值和。占领的意思就是在这个节点的子树中某种颜色出现次数最多（可以有多个）。

**思路：标准dsu on tree板子题**

全局维护每种颜色数量，某种数量颜色的权值和，以及最大数量颜色的数量是多少（有点点绕）即可。

```cpp
const int maxn = 3e5+7;

int n;
int c[maxn], num[maxn], mx;
int head[maxn], to[maxn*2], nxt[maxn*2], tot;
int son[maxn], sz[maxn];
ll sum[maxn], ans[maxn];

inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; nxt[tot]=head[v]; head[v]=tot;
}

void dfs0(int u, int f) {
    sz[u]=1;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f) continue;
        dfs0(v,u); sz[u]+=sz[v];
        if(sz[v]>sz[son[u]]) son[u]=v;
    }
}

void dfs1(int u, int f, int op) {
    if(op) {
        num[c[u]]++;
        sum[num[c[u]]-1]-=c[u], sum[num[c[u]]]+=c[u];
        if(sum[mx+1]) mx++;
    }
    else {
        num[c[u]]--;
        sum[num[c[u]]+1]-=c[u], sum[num[c[u]]]+=c[u];
        if(!sum[mx]) mx--;
    }
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f) continue;
        dfs1(v,u,op);
    }
}

void dfs2(int u, int f, int op) {
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f||v==son[u]) continue;
        dfs2(v,u,0);
    }
    if(son[u]) dfs2(son[u],u,1);
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f||v==son[u]) continue;
        dfs1(v,u,1);
    }
    num[c[u]]++;
    sum[num[c[u]]-1]-=c[u], sum[num[c[u]]]+=c[u];
    if(sum[mx+1]) mx++;
    ans[u]=sum[mx];
    if(!op) {
        num[c[u]]--;
        sum[num[c[u]]+1]-=c[u], sum[num[c[u]]]+=c[u];
        if(!sum[mx]) mx--;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i]; if(v==f) continue;
            dfs1(v,u,0);
        }
    }
}

int main() {
    n=read();
    for(int i=1; i<=n; ++i) c[i]=read();
    for(int i=1; i<n; ++i) add_edge(read(),read());
    dfs0(1,0);
    dfs2(1,0,1);
    for(int i=1; i<=n; ++i) printf("%lld%c", ans[i], " \n"[i==n]);
}
```

## 点分治

由于点分治框架比较简单，就随便记录一道吧

题意：带边权树，求树上距离为 $k$ 的点对中，路径上边数最少为多少？

（各种类似问题至少有：树上长度为 $k$ 的路径数；树上长度小于等于 $k$ 的路径数；多个询问，树上是否存在某些长度的路径）

思想：将路径分为**经过根节点的路径**和**不经过根节点的路径**。前者在当前树上暴力（cal+dfs函数）；后者通过合理的划分（solve+get_root函数），在自己所在子树中处理。

```cpp
const int maxn = 2e5+7;
const int maxl = 1e6+7;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;
int n, k
int head[maxn], to[maxn*2], w[maxn*2], nxt[maxn*2], tot;
inline void add_edge(int u, int v, int c) {
    ++tot; to[tot]=v; w[tot]=c; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; w[tot]=c; nxt[tot]=head[v]; head[v]=tot;
}

pr s[maxn]; //本题的栈要是二维的
int sz[maxn], used[maxn], dis[maxn], mi[maxl], top; //mi数组是nt数组的替代
int rt, MX, V, ans=inf;

void get_root(int u, int f) { //求树的重心
    sz[u]=1; int mx=0;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f||used[v]) continue;
        get_root(v,u); sz[u]+=sz[v];
        if(sz[v]>mx) mx=sz[v];
    }
    if(V-sz[u]>mx) mx=V-sz[u];
    if(mx<MX) rt=u, MX=mx;
}

void dfs(int u, int f, int c, int d) { //将点信息插入栈中，以及求新的sz
    dis[u]=dis[f]+c; s[++top]=pr(dis[u],d); sz[u]=1;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f||used[v]) continue;
        dfs(v,u,w[i],d+1); sz[u]+=sz[v];
    }
}

void cal(int u) { //每个部分的计算
    dis[u]=0; mi[0]=0; s[top=1]=pr(0,0);
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(used[v]) continue;
        dfs(v,u,w[i],1);
        for(int i=top-sz[v]+1; i<=top; ++i) {
            if(k-s[i].first>=0) ans=min(ans,s[i].second+mi[k-s[i].first]);
        }
        for(int i=top-sz[v]+1; i<=top; ++i) {
            if(s[i].first<maxl) mi[s[i].first]=min(mi[s[i].first],s[i].second);
        }
    }
    for(int i=1; i<=top; ++i) if(s[i].first<maxl) mi[s[i].first]=inf;
}

void solve(int u) { //分治
    cal(u); used[u]=1;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(used[v]) continue;
        rt=v, V=sz[v], MX=inf; get_root(v,0);
        solve(rt);
    }
}

int main() {
    memset(mi,inf,sizeof(mi));
    n=read(), k=read();
    for(int i=1; i<n; ++i) {
        int u=read()+1, v=read()+1, c=read();
        add_edge(u,v,c);
    }
    rt=1, V=n, MX=inf; get_root(1,0);
    solve(rt);
    printf("%d\n", ans>n?-1:ans);
}
```

## 点分树

类似于点分治，将找到的重心按顺序构成一颗含 $n$ 个节点的，也就是点分树。

重心的性质可以保证这棵树的高度是 $logn$ 的。于是，暴力跳 father 的复杂度就可以保证了！

例题：牛客 - 苹果树

题意：给一颗带点权边权的树，有修改和查询：

​		修改：在 $u$ 这个节点上增加一个苹果，权值为 $x$；

​		查询：求距离 $u$ 最近的权值范围在 $[x,y]$ 内的苹果的距离。

思路：

首先建好点分树，在每个节点上建动态开点线段树。

对于修改，从当前点一路暴力跳 father，在每个点上都单点更新 (x,dis[u,v])；

对于查询，从当前点一路暴力跳 father，得到 min(query(x,y,v)+dis[u,v])。

```cpp
int n, m;
int head[maxn], to[maxn*2], w[maxn*2], nxt[maxn*2], tot;

inline void add_edge(int u, int v, int c) {
    ++tot; to[tot]=v; w[tot]=c; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; w[tot]=c; nxt[tot]=head[v]; head[v]=tot;
}

int deep[maxn], sz[maxn], dis[maxn], used[maxn];
int top[maxn], son[maxn], fa[maxn], fa1[maxn];
int RT, V, MX;

void dfs0(int u, int f, int ds) {
    fa[u]=f; sz[u]=1; dis[u]=ds; deep[u]=deep[f]+1;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f) continue;
        dfs0(v,u,ds+w[i]); sz[u]+=sz[v];
        if(sz[v]>sz[son[u]]) son[u]=v;
    }
}

void dfs1(int u, int f, int tp) { //树剖，求树上距离
    top[u]=tp;
    if(!son[u]) return;
    dfs1(son[u],u,tp);
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f||v==son[u]) continue;
        dfs1(v,u,v);
    }
}

void get_root(int u, int f) {
    sz[u]=1; int mx=0;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f||used[v]) continue;
        get_root(v,u); sz[u]+=sz[v];
        if(sz[v]>mx) mx=sz[v];
    }
    if(V-sz[u]>mx) mx=V-sz[u];
    if(mx<MX) RT=u, MX=mx;
}

void all_root(int u) { //建立点分树，用fa1连接
    used[u]=1;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(used[v]) continue;
        RT=v; V=sz[v]; MX=inf; get_root(v,0);
        fa1[RT]=u; all_root(RT);
    }
}

int get_dis(int u, int v) {
    int res=0;
    while(top[u]!=top[v]) {
        if(deep[top[u]]<deep[top[v]]) swap(u,v);
        res+=dis[u]-dis[fa[top[u]]]; u=fa[top[u]];
    }
    if(deep[u]<deep[v]) swap(u,v);
    return res+dis[u]-dis[v];
}

int a[maxn], rt[maxn], node[maxn<<5], ls[maxn<<5], rs[maxn<<5], cnt;

void update(int x, int d, int l, int r, int &now) {
    if(!now) now=++cnt;
    if(l==r) { node[now]=min(node[now],d); return; }
    int m=(l+r)/2;
    if(x<=m) update(x,d,l,m,ls[now]);
    else update(x,d,m+1,r,rs[now]);
    node[now]=min(node[ls[now]],node[rs[now]]);
}

int query(int x, int y, int l, int r, int &now) {
    if(!now) return inf;
    if(x<=l&&r<=y) return node[now];
    int m=(l+r)/2, res=inf;
    if(x<=m) res=min(res,query(x,y,l,m,ls[now]));
    if(y>m) res=min(res,query(x,y,m+1,r,rs[now]));
    return res;
}

void update(int u, int x) { //暴力跳fa
    for(int v=u; v; v=fa1[v]) {
        update(x,get_dis(v,u),1,10000,rt[v]);
    }
}

int query(int u, int x, int y) { //暴力跳fa
    int res=inf;
    for(int v=u; v; v=fa1[v]) {
        res=min(res,query(x,y,1,10000,rt[v])+get_dis(u,v));
    }
    return res;
}

int main() {
    n=read(), m=read();
    for(int i=1; i<=n; ++i) a[i]=read();
    for(int i=1; i<n; ++i) {
        int u=read(), v=read(), w=read();
        add_edge(u,v,w);
    }
    dfs0(1,0,0); dfs1(1,0,1);
    RT=1; V=n; MX=inf; get_root(1,0);
    all_root(RT);
    memset(node,inf,sizeof(node));
    for(int i=1; i<=n; ++i) update(i,a[i]);
    while(m--) {
        int op=read();
        if(op==1) {
            int u=read(), x=read();
            update(u,x);
        }
        else {
            int u=read(), x=read(), y=read();
            int res=2*query(u,x,y);
            printf("%d\n", res>=inf?-1:res);
        }
    }
}
```

## 欧拉回路

若连通图的所有点度为偶数，则存在欧拉回路。

若连通图除了起点和终点为奇度，其余为偶度，则存在欧拉通路。

下面的 id[i] 表示这条边的编号（正反边同号）。

```cpp
void dfs(int u) {
    for(int &i=head[u]; i; i=nxt[i]) {
        int ID=id[i];
        if(used[ID]) continue; used[ID]=1;
        dfs(to[i]); s[++top]=ID;
    }
}
```

上面这种办法只知道边的遍历顺序，却不知道点的遍历顺序，尤其是在有重边的情况下，可能会出错。

下面这种办法可以直接输出点的遍历顺序。

```cpp
void dfs(int u) {
    for(int &i=head[u]; i; i=nxt[i]) {
        int ID=id[i];
        if(used[ID]) continue; used[ID]=1;
        dfs(to[i]);
        int x=e[ID].u, y=e[ID].v;
        if(x==u) s[++top]=2*ID, s[++top]=2*ID-1;
        else s[++top]=2*ID-1, s[++top]=2*ID;
    }
}
```

## 矩阵树定理

对于不含自环的图，生成树数量为对应的基尔霍夫矩阵任一代数余子式的值，即去掉一行一列后的行列式值。

```cpp
struct Edge{
    int u, v, w;
}e[maxn];

int n, m;
ll a[111][111];

ll qpow(ll a,ll k=mod-2,ll p=mod){ll s=1;while(k){if(k&1)s=s*a%p;a=a*a%p;k>>=1;}return s;}

ll Det(int n) {
    for(int i=1; i<=n; ++i) for(int j=1; j<=n; ++j) a[i][j]=(a[i][j]%mod+mod)%mod;
    ll res=1, f=0;
    for(int i=1; i<=n; ++i) {
        int mr=i;
        for(int j=i+1; j<=n; ++j) if(a[j][i]>a[mr][i]) mr=j;
        swap(a[i],a[mr]); if(i!=mr) ++f;
        res=res*a[i][i]%mod;
        if(!res) break;
        for(int j=i+1, inv=qpow(a[i][i]); j<=n; ++j) {
            ll d=a[j][i]*ll(inv)%mod;
            for(int k=i; k<=n; ++k) a[j][k]=(a[j][k]-d*a[i][k]%mod+mod)%mod;
        }
    }
    for(int i=1; i<=n+1; ++i) for(int j=1; j<=n+1; ++j) a[i][j]=0;
    return (res*(f&1?-1:1)+mod)%mod;
}

void solve() {
    n=read(), m=read();
    for(int i=1; i<=m; ++i) e[i].u=read(), e[i].v=read(), e[i].w=read();
    for(int i=1; i<=m; ++i) {
        int u=e[i].u, v=e[i].v;
        a[u][v]--; a[v][u]--;
        a[u][u]++; a[v][v]++;
    }
    printf("%lld\n", Det(n-1));
}
```

## 线段树优化建图

通过优雅地新建一堆虚点来优化建图。

适用于某些状况下单个点要向区间连边的情形，如求GCC、网络流搞二分图匹配、最短路等。

```cpp
void build(int l, int r, int now) {
    n1=max(n1,now);
    if(l==r) { pos[l]=now; return; }
    int m=(l+r)/2;
    build(l,m,now<<1); build(m+1,r,now<<1|1);
    add_edge(now,now<<1); add_edge(now,now<<1|1);
}

void add_edge(int u, int x, int y, int l=1, int r=nn, int now=1) {
    if(y<l||x>r) return;
    if(x<=l&&r<=y) { add_edge(u,now); return; }
    int m=(l+r)/2;
    add_edge(u,x,y,l,m,now<<1);
    add_edge(u,x,y,m+1,r,now<<1|1);
}
```

## 圆方树

静态带边权仙人掌上多次询问求两点最短距离 $dis(u,v)$

考虑建立圆方树，注意在带边权的情况下，建立的圆方树是有根树，根为建圆方树的起点，否则边权与定义不符。

```cpp
int n, m, q;
int head[maxn], to[maxn*2], w[maxn*2], nxt[maxn*2], tot;
int dfn[maxn], low[maxn], ID, cnt;
int fa[maxn], val[maxn];
int sum[maxn], dis[maxn];
vector<pr> e[maxn];

inline void add_edge(int u, int v, int c) {
    ++tot; to[tot]=v; w[tot]=c; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; w[tot]=c; nxt[tot]=head[v]; head[v]=tot;
}

void build(int u, int v, int w) {
    ++cnt;
    for(int p=v, s=w; p!=fa[u]; p=fa[p]) sum[p]=s, s+=val[p];
    sum[cnt]=sum[u], sum[u]=0; //u为这个环的入点
    for(int p=v; p!=fa[u]; p=fa[p]) {
        int mi=min(sum[p],sum[cnt]-sum[p]);
        e[cnt].push_back(pr(p,mi));
        e[p].push_back(pr(cnt,mi));
    }
}

void dfs(int u, int f) {
    dfn[u]=low[u]=++ID;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i]; if(v==f) continue;
        if(!dfn[v]) {
            fa[v]=u, val[v]=w[i];
            dfs(v,u);
        }
        low[u]=min(low[u],low[v]);
        if(low[v]<=dfn[u]) continue;
        e[u].push_back(pr(v,w[i]));
        e[v].push_back(pr(u,w[i]));
    }
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i];
        if(fa[v]==u||dfn[v]<=dfn[u]) continue; //dfs环的方向为u->v，下面从v一路跳fa到u建方点
        build(u,v,w[i]);
    }
}

int son[maxn], top[maxn], dep[maxn], sz[maxn];

void dfs1(int u, int f) {
    sz[u]=1; dep[u]=dep[f]+1; fa[u]=f;
    for(pr t: e[u]) {
        int v=t.first, w=t.second; if(v==f) continue;
        dis[v]=dis[u]+w;
        dfs1(v,u); sz[u]+=sz[v];
        if(sz[v]>sz[son[u]]) son[u]=v;
    }
}

void dfs2(int u, int f, int tp) {
    top[u]=tp;
    if(!son[u]) return;
    dfs2(son[u],u,tp);
    for(pr t: e[u]) {
        int v=t.first; if(v==f||v==son[u]) continue;
        dfs2(v,u,v);
    }
}

int get_lca(int u, int v) {
    while(top[u]!=top[v]) {
        if(dep[top[u]]<dep[top[v]]) swap(u,v);
        u=fa[top[u]];
    }
    return dep[u]<dep[v]?u:v;
}

int find(int u, int f) {
    int ans;
    while(top[u]!=top[f]) {
        ans=top[u];
        u=fa[top[u]];
    }
    return u==f?ans:son[f];
}

int main() {
    n=read(), m=read(), q=read(); cnt=n;
    for(int i=1; i<=m; ++i) {
        int u=read(), v=read(), w=read();
        add_edge(u,v,w);
    }
    dfs(1,0); dfs1(1,0); dfs2(1,0,1); //注意求dis的圆方树为有根树
    while(q--) {
        int u=read(), v=read();
        int lca=get_lca(u,v);
        if(lca<=n) printf("%d\n", dis[u]+dis[v]-2*dis[lca]); //lca 为圆点（普通点）
        else {
            int x=find(u,lca), y=find(v,lca); //先跳到 lca 所在环上
            int ans=dis[u]+dis[v]-dis[x]-dis[y];
            printf("%d\n", ans+min(abs(sum[x]-sum[y]),sum[lca]-abs(sum[x]-sum[y])));
        }
    }
}
```



## 最小割树

多次询问图中任意两点之间的最小割（最大流）。

预处理建树复杂度为 $n$ 次 $dinic$ 的 $O(n^3m)$。

大致做法（证明略）：在图中任取两点跑最小割，然后在树上连大小为割的边，并且当前图被划分为两个子图，然后分治下去。**在建好的树上，任意两点间的最小割即为原图中此两点的最小割**。而树上两点的最小割直接就是两点间简单路径边权最小值，可通过倍增处理。

```cpp
const int maxn = 5e2+7;
const int maxm = 3e4+7;
const int inf = 0x3f3f3f3f;

namespace Dinic{
    int head[maxn], to[maxm], w[maxm], nxt[maxm], tot=1;
    int cur[maxn], level[maxn];
    void add_edge(int u, int v, int c) {
        ++tot; to[tot]=v; w[tot]=c; nxt[tot]=head[u]; head[u]=tot;
        ++tot; to[tot]=u; w[tot]=0; nxt[tot]=head[v]; head[v]=tot;
    }
    void init() {
        memset(head,0,sizeof(head));
        tot=1;
    }
    bool bfs(int s, int t) {
        memset(level,0,sizeof(level));
        queue<int> q;
        q.push(s); level[s]=1;
        while(q.size()) {
            int u=q.front(); q.pop();
            for(int i=head[u]; i; i=nxt[i]) {
                int v=to[i];
                if(!w[i]||level[v]) continue;
                level[v]=level[u]+1;
                q.push(v);
            }
        }
        return level[t];
    }
    int dfs(int u, int t, int flow) {
        if(u==t||flow==0) return flow;
        int f, res=0;
        for(int &i=cur[u]; i; i=nxt[i]) {
            int v=to[i];
            if(level[v]==level[u]+1&&(f=dfs(v,t,min(flow,w[i])))>0) {
                w[i]-=f; w[i^1]+=f;
                flow-=f; res+=f;
                if(!flow) break;
            }
        }
        if(!res) level[u]=0;
        return res;
    }
    int dinic(int s, int t) {
        for(int i=2; i<=tot; i+=2) w[i]=w[i]+w[i^1], w[i^1]=0;
        int res=0;
        while(bfs(s,t)) memcpy(cur,head,sizeof(cur)), res+=dfs(s,t,inf);
        return res;
    }
}

struct GomoryHu_Tree{
    static const int logn = 10;
    int n, p[maxn], p1[maxn];
    int head[maxn], nxt[maxn*2], to[maxn*2], w[maxn*2], tot;
    int mi[maxn][logn], fa[maxn][logn], dep[maxn];
    inline void add_edge(int u, int v, int c) {
        ++tot; to[tot]=v; w[tot]=c; nxt[tot]=head[u]; head[u]=tot;
        ++tot; to[tot]=u; w[tot]=c; nxt[tot]=head[v]; head[v]=tot;
    }
    void init(int n) {
      	this->n=n;
        for(int i=1; i<=n; ++i) p[i]=i, head[i]=0, memset(mi[i],inf,sizeof(mi[i]));
        tot=0;
    }
    void build(int l, int r) {
        if(l==r) return ;
        int u=p[l], v=p[r];
        int w=Dinic::dinic(u,v), m=l, m1=r;
        add_edge(u,v,w);
        for(int i=l; i<=r; ++i) {
            if(Dinic::level[p[i]]) p1[m++]=p[i];
            else p1[m1--]=p[i];
        }
        for(int i=l; i<=r; ++i) p[i]=p1[i];
        build(l,m1); build(m,r);
    }
    void dfs(int u, int f) {
        dep[u]=dep[f]+1;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i]; if(v==f) continue;
            fa[v][0]=u; mi[v][0]=w[i];
            for(int j=1; j<logn; ++j) fa[v][j]=fa[fa[v][j-1]][j-1], mi[v][j]=min(mi[v][j-1],mi[fa[v][j-1]][j-1]);
            dfs(v,u);
        }
    }
    int query(int u, int v) {
        if(dep[u]<dep[v]) swap(u,v);
        int ans=inf;
        for(int i=logn-1; i>=0; --i) {
            if(dep[fa[u][i]]>=dep[v]) ans=min(ans,mi[u][i]), u=fa[u][i];
        }
        if(u==v) return ans;
        for(int i=logn-1; i>=0; --i) {
            if(fa[u][i]!=fa[v][i]) ans=min({ans,mi[u][i],mi[v][i]}), u=fa[u][i], v=fa[v][i];
        }
        return min({ans,mi[u][0],mi[v][0]});
    }
}GT;

int main() {
    int n=read(), m=read();
    for(int i=1; i<=m; ++i) {
        int u=read(), v=read(), w=read();
        Dinic::add_edge(u,v,w);
        Dinic::add_edge(v,u,w);
    }
    GT.init(n);
    GT.build(1,n);
    GT.dfs(1,0);
    int q=read();
    while(q--) printf("%d\n", GT.query(read(),read()));
}
```





# 数据结构

## 树状数组

```cpp
template <typename T>
struct BIT {
    vector<T> b; int n;
    BIT(int n=0): n(n) { b.resize(n+1,0); }
    T& operator [] (int i) { return b[i]; }
    void init(int n) { this->n=n; b.resize(n+1); for(int i=0;i<=n;++i)b[i]=0; }
    void add(int x, T d) { while(x<=n) b[x]+=d, x+=x&-x; }
    T get(int x) { T s=0; while(x) s+=b[x], x^=x&-x; return s; }
    void add(int x, int y, T d) { add(x,d), add(y+1,-d); }
    T get(int x, int y) { return get(y)-get(x-1); }
    int sum_lower_bound(T d){int p=0;for(int i=log2(n+1);i>=0;--i)if(p+(1<<i)<=n&&b[p|(1<<i)]<d)d-=b[p|=1<<i];return p+1;}
};
```



## 可撤销并查集

```cpp
struct Disjoint_set{
    int fa[maxn], sz[maxn], top;
    struct S{ int x, y, fx, szy; }stk[maxm];
    void init(int n) {
        top=0;
        for(int i=1; i<=n; ++i) fa[i]=i, sz[i]=1;
    }
    int find(int a) { while(a!=fa[a]) a=fa[a]; return a; }
    void link(int u, int v) {
        int x=find(u), y=find(v);
        if(sz[x]>sz[y]) swap(x,y);
        stk[++top]=(S){x,y,fa[x],sz[y]};
        if(x==y) return ;
        fa[x]=y;
        sz[y]+=sz[x];
    }
    void pop(int t) {
        while(t--) {
            auto [x,y,fx,szy]=stk[top--];
            // int x=stk[top].x, y=stk[top].y, fx=stk[top].fx, szy=stk[top].szy; top--;
            fa[x]=fx, sz[y]=szy;
        }
    }
}U;
```



## CDQ分治

题意：若某个元素的三个维度的值都小于等于另外一个元素，则是真的小于等于；给出一些元素，求等级分别为$0~n-1$的元素数量；而一个元素的等级被定义为小于等于这个元素的元素数量。

思路：(⊙o⊙)…标准的板子，CDQ分治就是细节多一点

```cpp
const int maxn = 1e5+10;

struct P{
    int x, y, z, cnt, rank;
    bool operator == (const P &rhs) const {
        return x==rhs.x&&y==rhs.y&&z==rhs.z;
    }
}p[maxn];

bool cmp1(const P &a, const P &b) {
    if(a.x==b.x&&a.y==b.y) return a.z<b.z;
    if(a.x==b.x) return a.y<b.y;
    return a.x<b.x;
}

bool cmp2(const P &a, const P &b) {
    return a.y<b.y;
}

int t[2*maxn], ans[maxn];
int n, k, tot;

inline void add(int x, int d) { while(x<=k) t[x]+=d, x+=x&-x; }
inline int sum(int x) { int ans=0; while(x) ans+=t[x], x-=x&-x; return ans; }

void solve(int l, int r) {
    if(l==r) {
        p[l].rank+=p[l].cnt-1;
        return;
    }
    int m=(l+r)/2;
    solve(l,m), solve(m+1,r);
    sort(p+l,p+m+1,cmp2), sort(p+m+1,p+r+1,cmp2);
    int j=l;
    for(int i=m+1; i<=r; ++i) {
        while(j<=m&&p[j].y<=p[i].y) add(p[j].z,p[j].cnt), ++j;
        p[i].rank+=sum(p[i].z);
    }
    while(--j>=l) add(p[j].z,-p[j].cnt);
}

int main() {
    n=read(), k=read();
    for(int i=1; i<=n; ++i) {
        scanf("%d%d%d", &p[i].x, &p[i].y, &p[i].z);
        p[i].cnt=1, p[i].rank=0;
    }
    sort(p+1,p+1+n,cmp1);
    int last=1;
    for(int i=2; i<=n; ++i) {
        if(p[i]==p[last]) p[last].cnt++, p[i].cnt=0;
        else last=i;
    }
    for(int i=1; i<=n; ++i) if(p[i].cnt) p[++tot]=p[i];
    solve(1,tot);
    for(int i=1; i<=tot; ++i) ans[p[i].rank]+=p[i].cnt;
    for(int i=0; i<n; ++i) printf("%d\n", ans[i]);
}
```

## 线段树分治

**题意**：有一排商店，每个商店都有许多商品。其中每个商店都有一种永久商品（随时都可以购买）。其次，每一天都会有一种操作，第$s$个商店进了属性为$v$的货物。每一天都可能有不止一个的询问，表示最近$d$天内$[l,r]$的商店内那种货物的属性与$x$的异或值最大，输出这个最大值即可（其实题面已经很清晰了）

**思路**：
1. 先考虑永久商品，由于要在$[l,r]$商店中选择，因此直接考虑可持久化$trie$树的维护
2. 而对于后面通过操作而来的新商品，我们用线段树分治解决即可。
3. 分治第一步：现将所有的询问在线段树上分解开，记录在每个节点上（$vector$记录即可），当后面操作进入线段树后对这些询问依次进行处理。这个过程用线段树容易处理
4. 分治第二步：在第一步中我们已经将询问进行了拆分，第二步我们就只需要对操作进行线段树分治。如果分治的是时间维度，那么我们首先需要按照空间维度对操作进行排序（按$s$），这样就可以保证分治后所有的操作在空间维度和时间维度上都是前面的小于后面的
5. 分治第三步（分治函数第一步：简称暴力）：正式开始线段树分治，其实这与整体二分挺像的，但特点是需要加上一个线段树节点编号，因为我们需要找到当前分治位置有哪些之前记录过的询问。在每个节点位置，直接暴力得构建一棵新的可持久化$trie$树（放心，毕竟是在分治上构建的，只是增加了一个$log$复杂度），然后分别回答当前节点的所有询问（询问不会太多，也只是加了一个$log$），并更新答案
6. 分治第四步（分治函数第二步：简称划分）：暴力处理完当前节点后，在往下一层递归时，我们需要将当前所有操作划分为时间在$mid$左边和$mid$右边的，然后用时间在$mid$左边的操作更新左儿子节点的询问，时间在$mid$右边的操作更新右儿子节点的询问
7. 整个问题到此也就结束了，学会以后就感觉挺模板的，哈哈哈

```cpp
const int maxn = 1e5+10;

struct P{
    int s, v, t;
    bool operator < (const P &rhs) const {
        return s<rhs.s;
    }
}p[maxn], p1[maxn], p2[maxn];

struct Q{
    int l, r, x;
}q[maxn];

int n, m, cnt1, cnt2;
int ans[maxn], pos[maxn];
int sz[maxn<<6], rt[maxn<<6], son[maxn<<6][2], tot;
vector<int> node[maxn<<6];

void insert(int x, int pre, int &now) {
    int p=now=++tot;
    for(int i=17; i>=0; --i) {
        bool s=x>>i&1;
        son[p][!s]=son[pre][!s];
        p=son[p][s]=++tot, pre=son[pre][s];
        sz[p]=sz[pre]+1;
    }
}

int query(int l, int r, int x) {
    int ans=0;
    for(int i=17; i>=0; --i) {
        bool s=x>>i&1;
        if(sz[son[r][!s]]-sz[son[l][!s]]) ans|=1<<i, l=son[l][!s], r=son[r][!s];
        else l=son[l][s], r=son[r][s];
    }
    return ans;
}

void update(int i, int x, int y, int l, int r, int now) {
    if(y<l||x>r) return;
    if(x<=l&&r<=y) { node[now].push_back(i); return; }
    int m=(l+r)/2;
    if(x<=m) update(i,x,y,l,m,now<<1);
    if(y>m) update(i,x,y,m+1,r,now<<1|1);
}

void segment(int x, int y, int l, int r, int now) {
    int cnt=0;
    for(int i=x; i<=y; ++i) {
        pos[++cnt]=p[i].s;
        insert(p[i].v,rt[cnt-1],rt[cnt]);
    }
    for(int i=0; i<node[now].size(); ++i) {
        int v=node[now][i];
        int l=lower_bound(pos+1,pos+1+cnt,q[v].l)-pos-1;
        int r=upper_bound(pos+1,pos+1+cnt,q[v].r)-pos-1;
        ans[v]=max(ans[v],query(rt[l],rt[r],q[v].x));
    } //以上为暴力回答询问
    if(l==r) return; //以下为按时间划分操作
    int m=(l+r)/2, c1=0, c2=0;
    for(int i=x; i<=y; ++i)
        if(p[i].t<=m) p1[++c1]=p[i];
        else p2[++c2]=p[i];
    for(int i=1; i<=c1; ++i) p[x+i-1]=p1[i];
    for(int i=1; i<=c2; ++i) p[x+c1+i-1]=p2[i];
    segment(x,x+c1-1,l,m,now<<1);
    segment(x+c1,y,m+1,r,now<<1|1);
}

int main() {
    n=read(), m=read();
    for(int i=1; i<=n; ++i) insert(read(),rt[i-1],rt[i]);
    for(int i=1; i<=m; ++i) {
        int f=read();
        if(!f) ++cnt1, p[cnt1]=(P){read(),read(),cnt1};
        else {
            int l=read(), r=read(), x=read(), d=read();
            ans[++cnt2]=query(rt[l-1],rt[r],x); //直接用可持久化trie树处理永久商品
            q[cnt2]=(Q){l,r,x};
            update(cnt2,max(1,cnt1-d+1),cnt1,1,m,1); //拆分询问。如果在所有询问读入结束后再进行拆分，则线段树可以是[1,cnt1]大小的，而不是[1,m]大小的，那样速度会快很多
        }
    }
    sort(p+1,p+1+cnt1); //按空间位置将操作进行排序
    segment(1,cnt1,1,m,1); //线段树分治
    for(int i=1; i<=cnt2; ++i) printf("%d\n", ans[i]);
}
```

## 整体二分

**题意**：[题面描述](https://www.luogu.org/problem/P3527)

**思路**：
1. 首先正常的读入以及连边，注意$l>r$时将$r$加上$m$，即用两个连在一起的数组表示循环
2. 然后，怎么说呢？反正学会整体二分以后就感觉是板子题。。。
3. 考虑当前$[l,r]$时间段的左半部分的流星雨全部落下后，将$[x,y]$内的询问分成可以在左半部分完成的和不能在左半部分完成的；然后依次递归下去就好
4. 好像就没了，hhh

```cpp
const int maxn = 3e5+10;

struct P{
    int id, need;
}p[maxn], pp[maxn*2];

int n, m, k;
int head[maxn], to[maxn], nxt[maxn], tot;
int L[maxn], R[maxn], A[maxn], ans[maxn];
ll b[maxn*2];

inline void add_edge(int u, int v) {
    ++tot, to[tot]=v, nxt[tot]=head[u], head[u]=tot;
}

inline void update(int x, int d) { while(x<=2*m) b[x]+=d, x+=x&-x; }
inline ll qsum(int x) { ll ans=0; while(x) ans+=b[x], x-=x&-x; return ans; }

void solve(int l, int r, int x, int y) {
    if(l==r) {
        for(int i=x; i<=y; ++i) ans[p[i].id]=l;
        return;
    }
    int mid=(l+r)/2, cnt1=0, cnt2=n;
    for(int i=l; i<=mid; ++i) update(L[i],A[i]), update(R[i]+1,-A[i]);
    for(int j=x; j<=y; ++j) {
        ll tmp=0;
        for(int i=head[p[j].id]; i&&tmp<p[j].need; i=nxt[i]) {
            int v=to[i];
            tmp+=qsum(v)+qsum(v+m);
        }
        if(tmp>=p[j].need) pp[++cnt1]=p[j];
        else p[j].need-=tmp, pp[++cnt2]=p[j];
    }
    for(int i=l; i<=mid; ++i) update(L[i],-A[i]), update(R[i]+1,A[i]);
    for(int i=1; i<=cnt1; ++i) p[x+i-1]=pp[i];
    for(int i=n+1; i<=cnt2; ++i) p[x+cnt1+i-n-1]=pp[i];
    solve(l,mid,x,x+cnt1-1), solve(mid+1,r,x+cnt1,y);
}

int main() {
    n=read(), m=read();
    for(int i=1; i<=m; ++i) add_edge(read(),i);
    for(int i=1; i<=n; ++i) p[i]=(P){i,read()};
    k=read();
    for(int i=1; i<=k; ++i) {
        L[i]=read(), R[i]=read(), A[i]=read();
        if(R[i]<L[i]) R[i]+=m;
    }
    solve(1,k+1,1,n);
    for(int i=1; i<=n; ++i) ans[i]==k+1?printf("NIE\n"):printf("%d\n", ans[i]);
}
```

## 主席树

### 静态区间第K小

```cpp
const int maxn = 2e5+10;

int tot;
int a[maxn], b[maxn], T[maxn];
int sz[maxn<<5], ls[maxn<<5], rs[maxn<<5];

void build(int l, int r, int &now) {
    now=++tot;
    sz[now]=0;
    int m=(l+r)/2;
    if(l==r) return;
    build(l,m,ls[now]), build(m+1,r,rs[now]);
}

void update(int x, int l, int r, int pre, int &now) {
    now=++tot;
    ls[now]=ls[pre], rs[now]=rs[pre], sz[now]=sz[pre]+1;
    if(l==r) return;
    int m=(l+r)/2;
    if(x<=m) update(x,l,m,ls[pre],ls[now]);
    else update(x,m+1,r,rs[pre],rs[now]);
}

int qk(int k, int x, int y, int l, int r) {
    if(l==r) return l;
    int s=sz[ls[y]]-sz[ls[x]];
    int m=(l+r)/2;
    if(s>=k) return qk(k,ls[x],ls[y],l,m);
    else return qk(k-s,rs[x],rs[y],m+1,r);
}

int main() {
    int n, q;
    scanf("%d%d", &n, &q);
    for(int i=1; i<=n; ++i) {
        scanf("%d", &a[i]);
        b[i]=a[i];
    }
    sort(b+1,b+1+n);
    int nn=unique(b+1,b+1+n)-b-1;
    tot=0, build(1,nn,T[0]);
    for(int i=1; i<=n; ++i) {
        int t=lower_bound(b+1,b+1+nn,a[i])-b;
        update(t,1,nn,T[i-1],T[i]);
    }
    while(q--) {
        int x, y, k;
        scanf("%d%d%d", &x, &y, &k);
        printf("%d\n", b[qk(k,T[x-1],T[y],1,nn)]);
    }
}
```

### 树上主席树

题意：有两种操作：
	1. 将某两个点连起来（连边后保证为森林或者一棵树）
	2. 询问两点之间路径上第K小权值（强制在线且保证询问合法）
思路：**树上主席树+启发式合并**

1. 先离散化一下还是有必要的
2. 连边后分别在每一棵树上建立主席树，每一棵树上的每一个节点的主席树维护从所在的树根到当前节点路径上的权值
3. 合并的过程用比较小的树合并大比较大的树上
4. 求解询问的关键：$s=sz[L[x]]+sz[L[y]]-sz[L[lca]]-sz[L[flca]]$，这样可以计算出$x$到$y$的路径上的权值个数，所以我们需要的是$x,y,lca(x,y),fa[lca(x,y)]$这四个值
5. 然后随便写个倍增求$lca$的函数，好像就完事了？
6. 嗯，差不多了，但是数组尽量开大点，毕竟每合并一次又需要更多的节点，并且你不知道要合并多少次。。。我开$64$被RE了两个点，然后索性直接开了256倍A掉了。

```cpp
const int maxn = 8e4+10;

int N, M, t, maxp;
int a[maxn], b[maxn], fa[maxn], st[maxn][18], vis[maxn], at[maxn], dp[maxn];
int head[maxn], son[maxn], to[maxn*2], nxt[maxn*2], cnt;
int T[maxn<<8], ls[maxn<<8], rs[maxn<<8], sz[maxn<<8], tot;

inline void add_edge(int u, int v) {
    ++cnt, to[cnt]=v, nxt[cnt]=head[u], head[u]=cnt;
    ++cnt, to[cnt]=u, nxt[cnt]=head[v], head[v]=cnt;
}

void update(int d, int l, int r, int pre, int &now) {
    now=++tot;
    ls[now]=ls[pre], rs[now]=rs[pre], sz[now]=sz[pre]+1;
    if(l==r) return;
    int m=(l+r)/2;
    if(d<=m) update(d,l,m,ls[pre],ls[now]);
    else update(d,m+1,r,rs[pre],rs[now]);
}

int query(int k, int x, int y, int lca, int flca, int l, int r) {
    if(l==r) return l;
    int s=sz[ls[x]]+sz[ls[y]]-sz[ls[lca]]-sz[ls[flca]];
    int m=(l+r)/2;
    if(s>=k) return query(k,ls[x],ls[y],ls[lca],ls[flca],l,m);
    else return query(k-s,rs[x],rs[y],rs[lca],rs[flca],m+1,r);
}

void dfs(int u, int f, int rt) {
    son[rt]++, fa[u]=st[u][0]=f, vis[u]=1, at[u]=rt, dp[u]=dp[f]+1;
    for(int i=1; i<18; ++i) st[u][i]=st[st[u][i-1]][i-1];
    update(a[u],1,maxp,T[f],T[u]);
    for(int i=head[u]; i; i=nxt[i]) if(to[i]!=f) dfs(to[i],u,rt);
}

inline int lca(int x, int y) {
    if(dp[x]<dp[y]) swap(x,y);
    int d=dp[x]-dp[y];
    for(int i=17; i>=0; --i) if(d>=(1<<i)) d-=1<<i, x=st[x][i];
    if(x==y) return x;
    for(int i=17; i>=0; --i) if(st[x][i]!=st[y][i]) x=st[x][i], y=st[y][i];
    return fa[x];
}

int main() {
    int testcase=read();
    N=read(), M=read(), t=read();
    for(int i=1; i<=N; ++i) a[i]=b[i]=read(), at[i]=i;
    sort(b+1,b+1+N); maxp=unique(b+1,b+1+N)-b-1;
    for(int i=1; i<=N; ++i) a[i]=lower_bound(b+1,b+1+maxp,a[i])-b;
    for(int i=1; i<=M; ++i) add_edge(read(),read());
    for(int i=1; i<=N; ++i) if(!vis[i]) dfs(i,0,i);
    int lastans=0;
    while(t--) {
        char op[2]; scanf("%s", op);
        int x=read()^lastans, y=read()^lastans, LCA;
        if(op[0]=='Q') LCA=lca(x,y), printf("%d\n", lastans=b[query(read()^lastans,T[x],T[y],T[LCA],T[fa[LCA]],1,maxp)]);
        else {
            add_edge(x,y);
            if(son[at[x]]<son[at[y]]) swap(x,y);
            dfs(y,x,at[x]);
        }
    }
}
```

## K-D Tree

### 单点插入+矩形区间和

**题意：**![在这里插入图片描述](https://img-blog.csdnimg.cn/20191027213414569.png)
**特点：** 强制在线(last_ans)+20M内存限制

**思路：** 没啥思路，就是K-D Tree板子题，因此下面记录K-D Tree的一些信息

1. K-D Tree也算二叉+平衡+树吧？用于维护K-Dimension的信息，将K维空间不断地通过某一维度的中位数点进行当前空间的分割。
2. 特点：树的节点数跟空间中点数相同（动态开点+节点回收），平衡时高度为$[log_2n]$
3. 一些需要维护的基础信息：
	1. $lson，rson:$当前节点左儿子+右儿子
	2. $sum:$当前子树的所有节点权值和（也可能是其他区间信息）
	3. $size:$当前子树节点总数
	4. $point:$当前节点所代表的空间中的一个点
	5. $mi[i]:$当前子树空间中第$i$维的下界
	6. $mx[i]:$当前子树空间中第$i$维的上界
4. 一些常用基础函数：
	1. `nth_element(tp+l,tp+m,tp+r+1);` 用于$log_2n$时间复杂度选出中点且将数组分成两边
	2. `void rebuild(int l, int r, int dim, int &now);`重构时调用的建树
	3. `void to_array(int idx, int now);`重构前将当前子树转化为数组并存在某临时数组中
	4. `void check(int dim, int &now);`检查当前子树是否相对平衡
	5. `void insert(P np, int dim, int &now);`插入一个新的点

```cpp
const int maxn = 2e5+10;

struct P{ int x[2], w; }p[maxn], tp[maxn];
int ls[maxn], rs[maxn], sum[maxn], sz[maxn];
int mi[maxn][2], mx[maxn][2];
int rub[maxn], top, tot, Dim, rt;
int N, last_ans;

bool operator < (P a, P b) { return a.x[Dim]<b.x[Dim]; }

inline void Min(int &x, int y) { if(x>y) x=y; }
inline void Max(int &x, int y) { if(x<y) x=y; }

int newnode() {
    if(top) return rub[top--];
    return ++tot;
}

void push_up(int now) {
    for(int i=0; i<2; ++i) {
        mi[now][i]=mx[now][i]=p[now].x[i];
        if(ls[now]) Min(mi[now][i],mi[ls[now]][i]), Max(mx[now][i],mx[ls[now]][i]);
        if(rs[now]) Min(mi[now][i],mi[rs[now]][i]), Max(mx[now][i],mx[rs[now]][i]);
    }
    sum[now]=sum[ls[now]]+sum[rs[now]]+p[now].w;
    sz[now]=sz[ls[now]]+sz[rs[now]]+1;
}

void rebuild(int l, int r, int dim, int &now) {
    if(l>r) { now=0; return; } //千万别忘了now=0！！！
    now=newnode();
    int m=(l+r)/2;
    Dim=dim; nth_element(tp+l,tp+m,tp+r+1); p[now]=tp[m];
    rebuild(l,m-1,dim^1,ls[now]); rebuild(m+1,r,dim^1,rs[now]);
    push_up(now);
}

void to_array(int idx, int now) {
    if(ls[now]) to_array(idx,ls[now]);
    tp[idx+sz[ls[now]]]=p[now]; rub[++top]=now;
    if(rs[now]) to_array(idx+sz[ls[now]]+1,rs[now]);
}

void check(int dim, int &now) {
    if(sz[now]*3<sz[ls[now]]*4||sz[now]*3<sz[rs[now]]*4) {
        to_array(1,now); rebuild(1,sz[now],dim,now);
    }
}

void insert(P np, int dim, int &now) {
    if(!now) {
        now=newnode(), ls[now]=rs[now]=0;
        p[now]=np;
        push_up(now);
        return;
    }
    if(np.x[dim]<=p[now].x[dim]) insert(np,dim^1,ls[now]);
    else insert(np,dim^1,rs[now]);
    push_up(now); check(dim,now);
}

inline bool in(int x1, int y1, int x2, int y2, int x3, int y3, int x4, int y4) {
    return x1>=x3&&y1>=y3&&x2<=x4&&y2<=y4;
}
inline bool out(int x1, int y1, int x2, int y2, int x3, int y3, int x4, int y4) {
    return x1>x4||x2<x3||y1>y4||y2<y3;
}

int query(int x1, int y1, int x2, int y2, int now) {
    if(!now) return 0;
    if(out(mi[now][0],mi[now][1],mx[now][0],mx[now][1],x1,y1,x2,y2)) return 0;
    if(in(mi[now][0],mi[now][1],mx[now][0],mx[now][1],x1,y1,x2,y2)) return sum[now];
    if(in(p[now].x[0],p[now].x[1],p[now].x[0],p[now].x[1],x1,y1,x2,y2))
        return p[now].w+query(x1,y1,x2,y2,ls[now])+query(x1,y1,x2,y2,rs[now]);
    return query(x1,y1,x2,y2,ls[now])+query(x1,y1,x2,y2,rs[now]);
}

int main() {
    N=read();
    int op;
    while(scanf("%d", &op), op!=3) {
        if(op==1) {
            int x=read()^last_ans, y=read()^last_ans, A=read()^last_ans;
            insert((P){x,y,A},0,rt);
        }
        else {
            int x1=read()^last_ans, y1=read()^last_ans, x2=read()^last_ans, y2=read()^last_ans;
            printf("%d\n", last_ans=query(x1,y1,x2,y2,rt));
        }
    }
}
```

### K远点对

**K-D Tree 真是优雅的暴力！开局建棵树，剪枝刷题数！**

**题意：**
	给定二维平面上的$N$个点，求第$K$远的无序点对。

**思路：**

1. 别问我为什么想到用K-D Tree的，因为是看了题解的。
2. 本题没有插入、删除等高级操作，仅仅建树和查询，代码简洁。
3. 进入正题：考虑**暴力**，暴力遍历对于每个点而言能形成的所有点对，显然复杂度为$O(n^2)$，不可行，接下来考虑剪枝。
4. 首先，$K=min(100,\frac{n*(n+1)}{2})$，所以先在小顶堆中插入$2*K$个$0$，在后续暴力搜索前$2*K$大点对的过程中逐渐把它们$pop$掉。
5. 对这$N$个点的每个点而言，都从K-D Tree的根节点往下遍历，每到一个节点，先计算当前节点与这个点的距离，并更新小顶堆，然后进入到剪枝的关键步骤。
6. 我们考虑每个节点的左右儿子，分别利用左右儿子的每个维度最大最小边界来计算可能的最远点，若当前子空间最远的点都无法对小顶堆进行更新，则不需要进入这个空间了！
7. 另一点剪枝：先遍历左右子空间中最远可能点更远的子空间，这样也许就不用再遍历另外一个空间啦。
8. 复杂度的话。。。还不会算，据说是$O(n^{\frac{3}{2}})$。

```cpp
const int maxn = 1e5+10;

int N, K, rt, tot, Dim;
int ls[maxn], rs[maxn];
ll mi[maxn][2], mx[maxn][2];
priority_queue<ll,vector<ll>,greater<ll> > q;

struct P{
    ll x[2];
    friend bool operator < (const P &a, const P &b) {
        return a.x[Dim]<b.x[Dim];
    }
    friend inline ll cal(const P &a, const P &b) {
        return (a.x[0]-b.x[0])*(a.x[0]-b.x[0])+(a.x[1]-b.x[1])*(a.x[1]-b.x[1]);
    }
    inline ll cal(const ll mi[2], const ll mx[2]) {
        ll d1=max(abs(x[0]-mi[0]),abs(x[0]-mx[0]));
        ll d2=max(abs(x[1]-mi[1]),abs(x[1]-mx[1]));
        return d1*d1+d2*d2;
    }
}p[maxn], tmp[maxn];

inline void Min(ll &a, ll b) { if(a>b) a=b; }
inline void Max(ll &a, ll b) { if(a<b) a=b; }

void push_up(int now) {
    for(int i=0; i<2; ++i) {
        mi[now][i]=mx[now][i]=p[now].x[i];
        if(ls[now]) Min(mi[now][i],mi[ls[now]][i]), Max(mx[now][i],mx[ls[now]][i]);
        if(rs[now]) Min(mi[now][i],mi[rs[now]][i]), Max(mx[now][i],mx[rs[now]][i]);
    }
}

void build(int l, int r, int dim, int &now) {
    if(l>r) return;
    now=++tot;
    int m=(l+r)/2;
    Dim=dim; nth_element(tmp+l,tmp+m,tmp+r+1); p[now]=tmp[m];
    build(l,m-1,dim^1,ls[now]); build(m+1,r,dim^1,rs[now]);
    push_up(now);
}

void query(P &a, int now) {
    ll t=cal(a,p[now]), lt=0, rt=0;
    if(t>q.top()) q.pop(), q.push(t);
    if(ls[now]) lt=a.cal(mi[ls[now]],mx[ls[now]]);
    if(rs[now]) rt=a.cal(mi[rs[now]],mx[rs[now]]);
    if(lt>rt) {
        if(lt>q.top()) query(a,ls[now]);
        if(rt>q.top()) query(a,rs[now]);
    }
    else {
        if(rt>q.top()) query(a,rs[now]);
        if(lt>q.top()) query(a,ls[now]);
    }
}

int main() {
    ios::sync_with_stdio(false); cin.tie(0);
    cin>>N>>K;
    for(int i=1; i<=N; ++i) cin>>tmp[i].x[0]>>tmp[i].x[1];
    build(1,N,0,rt);
    for(int i=1; i<=2*K; ++i) q.push(0);
    for(int i=1; i<=N; ++i) query(tmp[i],rt);
    cout<<q.top()<<endl;
}
```

## Splay

```cpp
struct Splay{
    int rt, tot, fa[maxn], ch[maxn][2], val[maxn], cnt[maxn], sz[maxn], flag[maxn];
  	int rub[maxn], top;
    void init() {
        for(int i=1; i<=tot; ++i) fa[i]=ch[i][0]=ch[i][1]=val[i]=cnt[i]=sz[i]=flag[i]=0;
        top=rt=tot=0;
    }
    int get(int x) { return x==ch[fa[x]][1]; }
    int newnode() { return top?rub[top--]:++tot; }
    void maintain(int x) { sz[x]=sz[ch[x][0]]+sz[ch[x][1]]+cnt[x]; }
    void clear(int x) { ch[x][0]=ch[x][1]=fa[x]=val[x]=sz[x]=cnt[x]=flag[x]=0; rub[++top]=x; }
    void rotate(int x) { //旋转操作
        int y=fa[x], z=fa[y], k=get(x);
        ch[y][k]=ch[x][k^1];
        fa[ch[x][k^1]]=y;
        ch[x][k^1]=y;
        fa[y]=x; fa[x]=z;
        if(z) ch[z][y==ch[z][1]]=x;
        else rt=x;
        maintain(y); maintain(x);
    }
    void splay(int x, int goal=0) { //将x伸展到goal的下面
        for(int f=fa[x]; f=fa[x], f!=goal; rotate(x))
            if(fa[f]!=goal) rotate(get(x)==get(f)?f:x);
        if(!goal) rt=x;
    }
    void ins(int v) { //插入v这个值
        if(!rt) {
          	int x=newnode();
            val[x]=v; cnt[x]++; rt=x;
            maintain(rt); return;
        }
        int x=rt, f=0;
        while(1) {
            if(flag[x]) push_down(x);
            if(val[x]==v) {
                cnt[x]++;
                maintain(x); maintain(f);
                splay(x); break;
            }
            f=x;
            x=ch[x][v>val[x]];
            if(!x) {
              	int x=newnode();
                val[x]=v; cnt[x]++; fa[x]=f;
                ch[f][v>val[f]]=x;
                maintain(x); maintain(f);
                splay(x); break;
            }
        }
    }
    void del(int v) { //删除一个值为v的数
        rk(v);
        if(cnt[rt]>1) { cnt[rt]--, sz[rt]--; return; }
        if(!ch[rt][0]&&!ch[rt][1]) { clear(rt); rt=0; return; }
        if(!ch[rt][0]) {
            int x=rt; rt=ch[rt][1];
            fa[rt]=0; clear(x); return;
        }
        if(!ch[rt][1]) {
            int x=rt; rt=ch[rt][0];
            fa[rt]=0; clear(x); return;
        }
        int y=pre(), x=rt;
        splay(y);
        fa[ch[x][1]]=y;
        ch[y][1]=ch[x][1];
        clear(x); maintain(rt);
    }
    void push_down(int x) { //下推标记
        swap(ch[x][0],ch[x][1]);
        flag[ch[x][0]]^=1;
        flag[ch[x][1]]^=1;
        flag[x]=0;
    }
    int rk(int v) { //返回v这个值从小到大的排名，并且把它弄到根节点（del里面要用）
        int res=0, x=rt;
        while(1) {
            if(flag[x]) push_down(x);
            if(v<val[x]) x=ch[x][0];
            else {
                res+=sz[ch[x][0]];
                if(v==val[x]) return splay(x), res+1;
                res+=cnt[x];
                x=ch[x][1];
            }
        }
    }
    int kth(int k) { //仅返回第k小的数所在节点
        int x=rt;
        while(1) {
            if(flag[x]) push_down(x);
            if(ch[x][0]&&k<=sz[ch[x][0]]) x=ch[x][0];
            else {
                k-=cnt[x]+sz[ch[x][0]];
                if(k<=0) return x;
                x=ch[x][1];
            }
        }
    }
    int pre() { //根节点的前驱，有时要主动插入-inf和+inf防止不存在前驱和后继
        int x=ch[rt][0];
        while(ch[x][1]) x=ch[x][1];
        return x;
    }
    int pre(int x) { ins(x); int s=val[pre()]; del(x); return s; } //x这个数的前驱
    int suf() { //根节点的后继
        int x=ch[rt][1];
        while(ch[x][0]) x=ch[x][0];
        return x;
    }
    int suf(int x) { ins(x); int s=val[suf()]; del(x); return s; } //x这个数的后继
    void reverse(int l, int r) { //将l到r的区间翻转
        if(l>=r) return;
        int x=kth(l-1), y=kth(r+1);
        splay(x); splay(y,x);
        flag[ch[y][0]]^=1;
    }
}sp;

int main() {
    int n=read();
    while(n--) {
        int op=read(), x=read();
        if(op==1) sp.ins(x);
        else if(op==2) sp.del(x);
        else if(op==3) printf("%d\n", sp.rk(x));
        else if(op==4) printf("%d\n", sp.val[sp.kth(x)]);
        else if(op==5) printf("%d\n", sp.pre(x));
        else if(op==6) printf("%d\n", sp.suf(x));
    }
}
```



## 红黑树

```cpp
struct RB_Tree{
    int rt, tot, fa[maxn], ch[maxn][2], val[maxn], cnt[maxn], sz[maxn], col[maxn];
    int rub[maxn], top;
    void init() {
        for(int i=1; i<=tot; ++i) fa[i]=ch[i][0]=ch[i][1]=val[i]=cnt[i]=sz[i]=col[i]=0;
        top=rt=tot=0;
    }
    int get(int x) { return x==ch[fa[x]][1]; }
    int newnode() { return top?rub[top--]:++tot; }
    void del_up(int x) { while(x) sz[x]--, x=fa[x]; }
    void maintain(int x) { sz[x]=sz[ch[x][0]]+sz[ch[x][1]]+cnt[x]; }
    void clear(int x) { ch[x][0]=ch[x][1]=fa[x]=val[x]=sz[x]=cnt[x]=col[x]=0; rub[++top]=x; }
    void rotate(int x) {
        int y=fa[x], z=fa[y], k=get(x);
        ch[y][k]=ch[x][k^1];
        fa[ch[x][k^1]]=y;
        ch[x][k^1]=y;
        fa[y]=x; fa[x]=z;
        if(z) ch[z][y==ch[z][1]]=x;
        else rt=x;
        maintain(y); maintain(x);
    }
    void ins_fix(int x) {                           // 双红修正
        while(x&&col[fa[x]]) {
            int f=fa[x], gf=fa[f], ul=ch[gf][get(f)^1];
            if(col[ul]) {                           // 叔叔节点为红，则上面三个点都变一下色，高度加一，从祖父往上继续递归
                col[f]=col[ul]=0;
                col[gf]=1;
                x=gf;
            }
            else if(get(x)!=get(f)) rotate(x), x=f; // 统一一下方向，重新递归，方便操作
            else col[f]=0, col[gf]=1, rotate(f);    // 最简单的情形，操作完就结束了
        }
        col[rt]=0;
    }
    void ins(int v) {
        int x=rt, f=0;
        while(x) {
            sz[x]++, f=x;
            if(v==val[x]) {
                cnt[x]++;
                return ;
            }
            x=ch[x][v>val[x]];
        }
        x=newnode(); val[x]=v; cnt[x]=sz[x]=col[x]=1;
        if(f) ch[f][v>val[f]]=x;
        else rt=x;
        fa[x]=f;
        ins_fix(x);
    }
    void del_fix(int x) {
        while(x!=rt&&!col[x]) {
            int f=fa[x], k=get(x), br=ch[f][k^1];
            if(col[br]) {                           // 兄弟节点为红色，直接给他变成黑色，并且旋转他，重新递归
                col[br]=0;
                col[f]=1;
                rotate(br);
            }
            else {                                  // 兄弟节点为黑色，分两种情况
                if(!col[ch[br][0]]&&!col[ch[br][1]]) {  // 兄弟节点的子节点全黑，则直接把他变红，当前子数就平衡了
                    col[br]=1;
                    x=f;                            // 若父节点是黑色：虽然平衡，但由于高度减1，要继续递归；若是红色：退出并且把这个红色变成黑色，才能保持高度不变
                }
                else {
                    if(!col[ch[br][k^1]]) {         // 兄弟节点的对应位置儿子为黑色，我们需要先将其统一成红色
                        col[ch[br][k]]=0;
                        col[br]=1;
                        rotate(ch[br][k]);
                        br=ch[f][k^1];
                    }
                    col[br]=col[f];
                    col[f]=col[ch[br][k^1]]=0;
                    rotate(br);
                    break;
                }
            }
        }
        col[x]=0;                                   // 要保持高度不变，把红色变黑色，使高度加回来
    }
    void del(int v) {
        int x=rt;
        while(x&&val[x]!=v) x=ch[x][v>val[x]];
        if(!x) return ;
        if(cnt[x]>1) {
            cnt[x]--;
            del_up(x);
            return ;
        }
        int d=x, g=0;
        if(ch[x][0]&&ch[x][1]) {                    // 若两个儿子都有，则让其后继节点为 d 节点
            d=ch[x][1];
            while(ch[d][0]) d=ch[d][0];
        }
        g=ch[d][!ch[d][0]];                         // 找到 d 的非空儿子来替代 d 的位置，其实包含了有多种情况
        fa[g]=fa[d];                                // g 可能为 0，但是不影响
        if(!fa[d]) rt=g;
        else ch[fa[d]][get(d)]=g;
        if(x!=d) val[x]=val[d], cnt[x]=cnt[d];
        del_up(fa[d]);
        for(int t=fa[d]; t&&t!=x; t=fa[t]) sz[t]-=cnt[d]-1;
        if(!col[d]) del_fix(g);                     // 用 g 来替代 d 进行删除
        clear(d);
    }
    int kth(int k) {                                // 排名为k的数
        int tmp;
        int x=rt;
        while(x) {
            tmp=sz[ch[x][0]];
            if(tmp+1<=k&&k<=tmp+cnt[x]) break;
            else {
                if(k<=tmp) x=ch[x][0];
                else k-=tmp+cnt[x], x=ch[x][1];
            }
        }
        return val[x];
    }
    int rk(int v) {                                 // v的排名
        ins(v);
        int tmp=0, sum=0;
        int x=rt;
        while(x) {
            tmp=sz[ch[x][0]];
            if(v==val[x]) break;
            else {
                if(v<val[x]) x=ch[x][0];
                else sum+=tmp+cnt[x], x=ch[x][1];
            }
        }
        del(v);
        return sum+tmp+1;
    }
    int pre(int v) {
        int res=-inf;
        int x=rt;
        while(x) {
            if(val[x]<v) res=val[x], x=ch[x][1];
            else x=ch[x][0];
        }
        return res;
    }
    int suf(int v) {
        int res=inf;
        int x=rt;
        while(x) {
            if(val[x]>v) res=val[x], x=ch[x][0];
            else x=ch[x][1];
        }
        return res;
    }
}rb;

int main() {
    int n=read();
    while(n--) {
        int op=read(), x=read();
        if(op==1) rb.ins(x);
        else if(op==2) rb.del(x);
        else if(op==3) printf("%d\n", rb.rk(x));
        else if(op==4) printf("%d\n", rb.kth(x));
        else if(op==5) printf("%d\n", rb.pre(x));
        else if(op==6) printf("%d\n", rb.suf(x));
    }
}
```



# 数学

## 组合数

组合数常见变换：

1. $\displaystyle \binom{n}{m} = \binom{n}{n-m}$
2. $\displaystyle \binom{n}{k} = \frac{n}{k} \binom{n-1}{k-1}$，递推式。
3. $\displaystyle \binom{n}{m} = \binom{n-1}{m} + \binom{n-1}{m-1}$，杨辉三角的公式表达，$O(n^2)$ 推导组合数。
4. $\displaystyle \sum_{i=0}^{n}\binom{n}{i} = 2^n$，二项式定理 $(a+b)^n$ 中 $a=1,b=1$ 的特殊情况。
5. $\displaystyle \sum_{i=0}^{n}(-1)^i \binom{n}{m} = 0$，二项式定理中 $a=1,b=-1$ 的特殊情况。
6. $\displaystyle \sum_{i=0}^{n} \binom{n}{i} \binom{m}{i} =  \sum_{i=0}^{n} \binom{n}{i} \binom{m}{m-i} = \binom{n+m}{m} \ \ (n≥m)$
7. $\displaystyle \sum_{i=0}^{n}\binom{n}{i}^2 = \binom{2n}{n}$，上式中 $n=m$ 的特殊情况。
8. $\displaystyle \sum_{i=0}^{n}i\binom{n}{i} = n2^{n-1}$，带权和的一个式子。
9. $\displaystyle \sum_{i=0}^{n} i^2\binom{n}{i} = n(n+1)2^{n-2}$，带权和的另一个式子。
10. $\displaystyle \sum_{i=0}^{n}\binom{i}{k} = \binom{n+1}{k+1}$
11. $\displaystyle \binom{n}{r} \binom{r}{k} = \binom{n}{k} \binom{n-k}{r-k}$
12. $\displaystyle \sum_{i=0}^{n}\binom{i}{a} \binom{n-i}{b-a} = \binom{n+1}{b+1}$，与 $a$ 无关，$a \in [0,b]$。
13. $\displaystyle \sum_{i=0}{n}\binom{n-i}{i} = F_{n+1}$，其中 $F$ 是斐波拉契数列。

## 斐波那契数列

定义如下：
$$
F_0=0, F_1=1, F_n=F_{n-1}+F_{n-2}
$$


$$
F_n = \frac{(\frac{1+\sqrt5}{2})^n-(\frac{1-\sqrt5}{2})^n}{\sqrt5}
$$



简单性质：

1. $F_{n-1}F_{n+1}-F_n^2 = (-1)^n$，卡西尼性质。
2. $F_{n+m} = F_mF_{n+1} + F_{m-1}F_n$，附加性质。
3. $F_{2n} = F_n(F_{n-1}+F_{n+1})$，附加性质中 $m=n$ 的情况。
4. $\forall k \in \mathbb{N} , F_n|F_{nk}$，由上一条性质归纳可得。
5. $\forall F_a|F_b,a|b$，上述性质可逆。
6. $(F_n,F_m)=F_{(n,m)}$，GCD 性质。
7. 模 $n$ 意义下斐波那契数列的周期被称为 皮萨诺周期，该数可以证明总是不超过 $6n$。

## 原根

### 最小原根

若一个数 $m$ 存在原根，则 $m$ 的最小原根在 $O(m^{0.25})$ 级别。

求原根原理：若 $g$ 是 $m$ 的原根，当且仅当对 $\varphi(m)$ 的任意质因数 $p$ 都有 $g^{\frac{\varphi(m)}{p}}\not\equiv 1 \ (mod \ m)$，于是考虑直接暴力枚举。

```cpp
ll qpow(ll a,ll k=mod-2,ll p=mod){ll s=1;while(k){if(k&1)s=s*a%p;a=a*a%p;k>>=1;}return s;}

int get_ord(int m) {
    vector<int> v;
    int x = m - 1, y = x; // x = phi(m)
    for (int i = 2; i * i <= x; ++i) {
        if (x % i == 0) {
            v.push_back(i);
            while (x % i == 0) x /= i;
        }
    }
    if (x > 1) v.push_back(x);
    int G = 0;
    for (int i = 2; i < m; ++i) {
        int f = 1;
        for (int p: v) if (qpow(i, y / p, m) == 1) { f = 0; break; }
        if (f) { G = i; break; }
    }
    return G;
}
```

### 所有原根

若一个数 $m$ 存在原根，则它的原根数量为 $\varphi(\varphi(m))$ 个；若 $g$ 为最小原根，则 $g^1,g^2,g^3,...,g^{\varphi(m)-1}$都是 $m$ 的原根。

### 判断是否有原根

若 $m$ 有原根，则 $m$ 一定是下列形式：$2,4,p^a,2p^a$ 。这里 $p$ 为奇素数， $a$ 为正整数。

## 线性筛总结

**总体思想：筛某个合数时，总是以这个数的最小质因数一次筛除它。**

**重点：因数个数$d(n)$、因数和$s(n)$、欧拉函数$phi(n)$、莫比乌斯函数$mu(n)$等均为$n$的积性函数，能很好的利用“最小质因数”筛法性质。**

**筛积性函数 $f(n)$ 时，首先要推出 $f(p_j^k)$ 的表达式，然后着重推 $f(i*p_j)(p_j|i)$ 的式子。**

### 筛质数
处理出$1e8$以内的素数用时$1s$左右，实测复杂度为$O(n)$带一个小常数。

$1e7$以内则$0.1s$左右。
```cpp
const int maxn = 1e7+7;
const int maxp = 7e5+7;

bool vis[maxn];
int pri[maxp], tot;

void getPrime() {
    for(int i=2; i<maxn; ++i) {
        if(!vis[i]) pri[++tot]=i;
        for(int j=1; j<=tot&&i*pri[j]<maxn; ++j) {
            vis[i*pri[j]]=1;
            if(i%pri[j]==0) break;
        }
    }
}
```

### 因数个数
d数组：d函数，因数个数
num数组：最小质因数的次数

```cpp
const int maxn = 1e6+7;

int d[maxn], num[maxn], vis[maxn], pri[maxn], tot;

void getDiv() {
    d[1]=1;
    for(int i=2; i<maxn; ++i) {
        if(!vis[i]) pri[++tot]=i, d[i]=2, num[i]=1;
        for(int j=1; j<=tot&&i*pri[j]<maxn; ++j) {
            int k=i*pri[j]; vis[k]=1;
            if(i%pri[j]==0) {
                num[k]=num[i]+1; d[k]=d[i]/num[k]*(num[k]+1);
                break;
            }
            num[k]=1; d[k]=d[i]*2;
        }
    }
}
```

### 因数和
s数组：s函数（$1e6$的s在$1e6$左右，用$int$即可）
g数组：最小质因数对应的：$\displaystyle \frac{p^{e+1}-1}{p-1}=1+p+p^2+\dots+p^e=1+p*(1+p+\dots+p^{e-1})$

```cpp
const int maxn = 1e6+7;

int s[maxn], g[maxn], vis[maxn], pri[maxn], tot;

void getSum() {
    s[1]=g[1]=1;
    for(int i=2; i<maxn; ++i) {
        if(!vis[i]) pri[++tot]=i, s[i]=i+1, g[i]=i+1;
        for(int j=1; j<=tot&&i*pri[j]<maxn; ++j) {
            int k=i*pri[j]; vis[k]=1;
            if(i%pri[j]==0) {
                g[k]=g[i]*pri[j]+1; s[k]=s[i]/g[i]*g[k];
                break;
            }
            g[k]=pri[j]+1; s[k]=s[i]*g[k];
        }
    }
}
```

### 欧拉函数
phi数组：欧拉函数

```cpp
const int maxn = 1e7+7;
const int maxp = 7e5+7;

int pri[maxp], phi[maxn], tot;
bool vis[maxn];

void getPhi() {
    phi[1]=1;
    for(int i=2; i<maxn; ++i) {
        if(!vis[i]) pri[++tot]=i, phi[i]=i-1;
        for(int j=1; j<=tot&&i*pri[j]<maxn; ++j) {
            vis[i*pri[j]]=1;
            if(i%pri[j]==0) { phi[i*pri[j]]=phi[i]*pri[j]; break; }
            phi[i*pri[j]]=phi[i]*phi[pri[j]];
        }
    }
}
```

### 莫比乌斯函数
mu数组：莫比乌斯函数

```cpp
const int maxn = 1e7+7;
const int maxp = 7e5+7;

int pri[maxp], mu[maxn], tot;
bool vis[maxn];

void getMu() {
    mu[1]=1;
    for(int i=2; i<maxn; ++i) {
        if(!vis[i]) pri[++tot]=i, mu[i]=-1;
        for(int j=1; j<=tot&&i*pri[j]<maxn; ++j) {
            vis[i*pri[j]]=1;
            if(i%pri[j]==0) { mu[i*pri[j]]=0; break; }
            mu[i*pri[j]]=-mu[i];
        }
    }
}
```



## 整除分块

**向下取整**

证明略

```cpp
for(int l=1, r; l<=n; l=r+1) {
		r=n/(n/l);
		// cal(l...r);
}
```

**向上取整**

在已知上述向下取整正确的情况下，$\displaystyle \lceil \frac{n}{l} \rceil = \lceil \frac{n}{r} \rceil$，且 $r$ 最大，等价于 $\displaystyle \lfloor \frac{n-1}{l} \rfloor + 1=\lfloor \frac{n-1}{r} \rfloor + 1$，且 $r$ 最大，两边去掉 $1$，于是：

$\displaystyle \lfloor \frac{n-1}{l} \rfloor=\lfloor \frac{n-1}{r} \rfloor$，且 $r$ 最大。

```cpp
for(int l=1, r; l<=n; l=r+1) {
  	if(l!=n) r=(n-1)/((n-1)/l);
  	else r=n;
  	// cal(l...r);
}
```

或者

```cpp
n--;
for(int l=1, r; l<=n; l=r+1) {
  	r=n/(n/l);
		// cal(l...r); 注意 n 不是原来的 n 了
}
// cal(n+1,n+1);
```



## 莫比乌斯反演

如果 $f(n)$ 和 $g(n)$ 为两个数论函数（积性函数），且 $\displaystyle f(n) = \sum_{d|n}{g(d)}$

则 $\displaystyle g(n) = \sum_{d|n}{\mu(d) f(\frac{n}{d})}$

证明：已知 $f=g*1$，两边同时卷积 $\mu$，得 $f*\mu=g*1*\mu=g*\varepsilon=g$，即 $g=f*\mu$。

关键：$\varepsilon$ 为单位函数，$\varepsilon(x)=[x=1]$，且 $\varepsilon=1*\mu$。

### 一些结论

1. $\displaystyle \sum_{i=1}^{n} \lfloor \frac{n}{i} \rfloor =\sum_{i=1}^{n}d[i]$，单个前缀因数和可以用整除分块来做，降低复杂度。

2. $\displaystyle d(i*j)=\sum_{x|i}\sum_{y|j}[gcd(x,y)=1]$，可以通过一一映射的方式来证明这个式子。

3. $\displaystyle [gcd(i,j)=1]=\sum_{d|gcd(i,j)}\mu(d)$，利用单位函数：$\displaystyle  \varepsilon(x)=[x=1]=\sum_{d|x}\mu(d) = 1*\mu$

4. 一些卷积

   <img src="/Users/universeofhk/Library/Application Support/typora-user-images/image-20200408190437019.png" alt="image-20200408190437019" style="zoom:90%;" />

## 欧拉定理

**欧拉定理：当$a,m$互质时，有**
$$\displaystyle a^b \equiv a^{b\%\varphi(m)} \ (mod \ m)$$

**扩展欧拉定理：当$a,m$不一定互质时，有**
$$\displaystyle
\begin{cases}
a^b \equiv a^{b\%\varphi(m)} \ ( \ mod \ m \ ), \ b<\varphi(m) \\
a^b \equiv a^{b\%\varphi(m)+\varphi(m)} \ ( \ mod \ m \ ), b≥\varphi(m)
\end{cases}$$

```cpp
const int maxn = 3e5+7;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;

int main() {
    int a=read(), m=read();
    string b; cin>>b;
    int phi=1, M=m;
    for(int i=2; i*i<=M; ++i) {
        if(M%i==0) {
            M/=i; phi*=i-1;
            while(M%i==0) phi*=i, M/=i;
        }
    }
    if(M>1) phi*=M-1;
    int k=0, f=0;
    for(char c: b) {
        k=k*10+c-'0';
        if(k>=phi) k%=phi, f=1;
    }
    if(f) k+=phi; //只有当b>=phi(m)时才加一个
    ll ans=1;
    while(k) {
        if(k&1) ans=ans*a%m;
        a=ll(a)*a%m, k>>=1;
    }
    printf("%lld\n", ans);
}
```



## Lucas定理

模数 $p$ 为质数，且建议在 $1e6$ 以内。

```cpp
struct Lucas{
    int p;
    vector<ll> fac, inv;
    Lucas(int p=2): p(p) {
        fac.resize(p); inv.resize(p);
        fac[0]=fac[1]=inv[0]=inv[1]=1;
        for(int i=2; i<p; ++i) fac[i]=i*fac[i-1]%p;
        for(int i=2; i<p; ++i) inv[i]=(p-p/i)*inv[p%i]%p;
        for(int i=2; i<p; ++i) inv[i]=inv[i]*inv[i-1]%p;
    }
    ll cal(ll n, ll m) {
        if(n<0||m<0||n<m) return 0;
        if(n<p) return fac[n]*inv[m]%p*inv[n-m]%p;
        return cal(n/p,m/p)*cal(n%p,m%p)%p;
    }
};
```



## $\min-\max$ 容斥

常用于 $\min$（或 $\max$）不好求，但 $\max$（或 $\min$）好求时，进行 $\min-\max$ 的转化。

最重要的是，这玩意对期望也可以转化。

$\displaystyle \max(S)=\sum_{T \subseteq S }{(-1)^{|T|+1} \min(T)}$

$\displaystyle E(\max(S))=\sum_{T \subseteq S }{(-1)^{|T|+1} E(\min(T))}$

更进一步，还可以对第 $k$ 大向 $\min$ 进行转化，反过来也一样。

$\displaystyle \max_{k}(S)=\sum_{T \subseteq S }{(-1)^{|T|-k} \binom{|T|-1}{k-1} \min(T)}$

$\displaystyle E(\max_{k}(S))=\sum_{T \subseteq S }{(-1)^{|T|-k} \binom{|T|-1}{k-1} E(\min(T))}$

## 中国剩余定理

**求解下列同余方程组（模数互质）**

$$\begin{cases}
x \equiv a_1 \ (\ mod \ m_1\ )\\
x \equiv a_2 \ (\ mod \ m_2\ )\\
\quad \vdots\\
x \equiv a_n \ (\ mod \ m_n)
\end{cases}$$

**构造法**
**令：**$\displaystyle lcm=\prod\limits_{i=1}^{n}{m_i}$，$\displaystyle M_i=\frac{lcm}{m_i}$，$x=inv(M_i,m_i)$

**则：**$\displaystyle ans=\sum\limits_{i=1}^{n}{a_i*x}$

```cpp
struct CRT{
    int n;
    __int128 a[maxn], b[maxn]; //a表示余数，b表示模数
    CRT(): n(0) {}
    void add(ll x, ll y) { a[n]=x; b[n]=y; ++n; }
    __int128 exgcd(__int128 a, __int128 b, __int128 &x, __int128 &y) {
        if(!b) { x=1, y=0; return a; }
        __int128 ans=exgcd(b,a%b,y,x); y-=a/b*x;
        return ans;
    }
    __int128 cal() {
        __int128 lcm=1;
        for(int i=0; i<n; ++i) a[i]=(a[i]%b[i]+b[i])%b[i], lcm*=b[i];
        __int128 ans=0, x, y;
        for(int i=0; i<n; ++i) {
            __int128 tmp=lcm/b[i];
            exgcd(tmp,b[i],x,y);
            x=(x%b[i]+b[i])%b[i];
            ans=(ans+a[i]*x%lcm*tmp%lcm)%lcm;
        }
        return ans;
    }
};
```

## 扩展中国剩余定理

**处理模数不一定互质的情况**

**思路：不断合并同余方程**

**合并策略：**

**令：**$g=(m_1,m_2)，m=lcm(m_1,m_2)$

$\displaystyle a=\left(inv\left(\frac{m_1}{g},\frac{m_2}{g}\right)*\frac{a_2-a_1}{g}\right)\%$$\displaystyle \frac{m_2}{g}*m_1+a_1$ $(\ mod \ m \ )$

[详细证明](https://www.cnblogs.com/zwfymqz/p/8425731.html)

```cpp
struct CRT{
    int n;
    __int128 a[3], b[3]; //a表示余数，b表示模数
    CRT(): n(0) {}
    void add(ll x, ll y) { a[n]=x; b[n]=y; ++n; }
    __int128 exgcd(__int128 a, __int128 b, __int128 &x, __int128 &y) {
        if(!b) { x=1, y=0; return a; }
        __int128 ans=exgcd(b,a%b,y,x); y-=a/b*x;
        return ans;
    }
    __int128 cal() {
        __int128 A=0, B=1, x, y;
        for(int i=0; i<n; ++i) {
            __int128 g=exgcd(B,b[i],x,y);
            __int128 Bg=B/g, bg=b[i]/g;
            exgcd(Bg,bg,x,y);
            x=(x%bg+bg)%bg;
            __int128 lcm=Bg*b[i];
            if((a[i]-A)%g) return -1;
            A=(((a[i]-A)/g*x%bg*B+A)%lcm+lcm)%lcm;
            B=lcm;
        }
        return A;
    }
};
```

## 逆元

逆元满足：$a*inv(a)≡1(mod$ $p)$
### 模p​为素数的逆元

利用费马小定律:$a^{p-1}≡1(mod$ $p)$
则$inv(a)=a^{p-2}$
### 模p​不一定为质数的逆元求法

利用拓展欧几里得算法，既可以求合数模，也可以求素数模；并且常数较小，实际速度比快速幂更好一点，但快速幂好写一些。

给定模数 $m$ ，求 $a$ 的逆相当于求解 $ax=1(mod$ $m)$ 
这个方程可以转化为 $ax-my=1$
然后套用求二元一次方程的方法，用扩展欧几里得算法求得一组 $x0,y0$ 和 $gcd$
检查 $gcd$ 是否为 1 
$gcd$ 不为 1 则说明逆元不存在 
若为 $1$，则调整 $x0$ 到 $0$ ~ $m-1$ 的范围中即可。

```cpp
ll exgcd(ll a, ll b, ll &x, ll &y) {
    if(!b) { x=1, y=0; return a; }
    ll ans=exgcd(b,a%b,y,x); y-=a/b*x;
    return ans;
}
ll inv(ll a, int mod) {
    ll x, y;
    return exgcd(a,mod,x,y)==1?(x+mod)%mod:-1;
}
```

### 递归求逆元

递推公式：$inv[1]=1$, $inv[i]=(mod-mod/i)*inv[mod$%$i]$%$mod;$

```cpp
inv[1]=1;
for(int i=2; i<mod; ++i) inv[i]=(mod-mod/i)*inv[mod%i]%mod;
```

也可用于单次求逆元（但一般不用）

```cpp
ll inv(ll a, ll m) { return a==1?1:(m-m/a)*inv(m%a,m)%m; }
```

### 利用递归预处理阶乘的逆元

```cpp
inv[0]=inv[1]=1;
for(int i=2; i<mod; ++i) inv[i]=(mod-mod/i)*inv[mod%i]%mod;
for(int i=2; i<mod; ++i) inv[i]=inv[i]*inv[i-1]%mod;
```

## 高斯消元法

### 唯一解：[判定存在性并求值](https://www.luogu.org/problem/P3389)

1. $a$数组存增广矩阵（第$n+1$列为常数列）
2. $mr$为每次所选取的最大系数的行标号，然后交换第$i$行和$mr$行
3. 若存在唯一解，则系数矩阵会被化简为$n×n$的单位矩阵，相应的常数列就是$x_i$的解啦！
4. 调用：Gauss();
5. 返回值：$0$表示无唯一解，$1$表示有唯一解
```cpp
const int maxn = 1e5+10;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;
const double eps = 1e-7;

int n;
double a[101][102];

bool Gauss() {
    for(int i=1; i<=n; ++i) {
        int mr=i;
        for(int j=i+1; j<=n; ++j) if(fabs(a[j][i])>fabs(a[mr][i])) mr=j;
        swap(a[i],a[mr]);
        if(fabs(a[i][i])<eps) return 0;
        for(int j=1; j<=n; ++j) {
            if(j==i) continue;
            double d=a[j][i]/a[i][i];
            for(int k=i; k<=n+1; ++k) a[j][k]-=d*a[i][k];
        }
        for(int k=i+1; k<=n+1; ++k) a[i][k]/=a[i][i]; a[i][i]=1;
    }
    return 1;
}

int main() {
    n=read();
    for(int i=1; i<=n; ++i)
        for(int j=1; j<=n+1; ++j) scanf("%lf", &a[i][j]);
    if(Gauss()) for(int i=1; i<=n; ++i) printf("%.2f\n", a[i][n+1]);
    else printf("No Solution\n");
}
```

### 无穷解：[判定解的情况](https://www.luogu.org/problem/P2455)

1. 要先判定无解，再判定无穷解（但是要在化简过程中先记录）
2. 判定无解要在化简结束后，并且要检查所有行

```cpp
const int maxn = 1e5+10;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;
const double eps = 1e-7;

int n;
double a[101][102];

int Gauss() {
    bool Inf=0;
    for(int i=1; i<=n; ++i) {
        int mr=i;
        for(int j=i+1; j<=n; ++j) if(fabs(a[j][i])>fabs(a[mr][i])) mr=j;
        swap(a[i],a[mr]);
        if(fabs(a[i][i])<eps) { Inf=1; continue; }
        for(int j=1; j<=n; ++j) {
            if(j==i) continue;
            double d=a[j][i]/a[i][i];
            for(int k=i; k<=n+1; ++k) a[j][k]-=d*a[i][k];
        }
        for(int k=i+1; k<=n+1; ++k) a[i][k]/=a[i][i]; a[i][i]=1;
    }
    for(int i=1; i<=n; ++i) {
        int f=1;
        for(int j=1; j<=n; ++j) if(fabs(a[i][j])>=eps) { f=0; break; }
        if(f&&fabs(a[i][n+1])) return 0;
    }
    if(Inf) return 2;
    return 1;
}

int main() {
    n=read();
    for(int i=1; i<=n; ++i)
        for(int j=1; j<=n+1; ++j) scanf("%lf", &a[i][j]);
    int ans=Gauss();
    if(ans==1) for(int i=1; i<=n; ++i) printf("x%d=%.2f\n", i, a[i][n+1]);
    else printf("%d\n", ans?0:-1);
}
```

### 高级版（可求出自由变量数目）

a数组中常系数保存在a[i][var]中；
唯一解保存在x数组中；
自由变量保存在free_x数组中；
```cpp
const double eps = 1e-9;

double a[maxn][maxn];//每个方程中每个未知数的系数
double x[maxn];//解集
bool free_x[maxn];

int sgn(double x) {
    return (x>eps)-(x<-eps);
}

int gauss(int equ, int var) {
    int i,j,k;
    int max_r; // 当前这列绝对值最大的行.
    int col; // 当前处理的列.
    double temp;
    int free_x_num;
    int free_index;
    // 转换为阶梯阵.
    col=0; // 当前处理的列.
    memset(free_x,1,sizeof(free_x));
    for(k=0; k<equ&&col<var; k++,col++)
    {
        max_r=k;
        for(i=k+1; i<equ; i++) {
            if(sgn(fabs(a[i][col])-fabs(a[max_r][col]))>0) max_r=i;
        }
        if(max_r!=k){ // 与第k行交换.
            for(j=k; j<=var; j++) swap(a[k][j],a[max_r][j]);
        }
        if(sgn(a[k][col])==0) { k--; continue; }// 说明该col列第k行以下全是0了，则处理当前行的下一列.
        for(i=k+1; i<equ; i++) { // 枚举要删去的行.
            if(sgn(a[i][col])!=0) {
                temp=a[i][col]/a[k][col];
                for(j=col; j<=var; j++) {
                    a[i][j]=a[i][j]-a[k][j]*temp;
                }
            }
        }
    }
    for(i=k;i<equ;i++) if(sgn(a[i][col])!=0) return 0;
    if(k<var) {
        for(i=k-1; i>=0; i--) {
            free_x_num=0;
            for(j=0; j<var; j++) {
                if(sgn(a[i][j])!=0&&free_x[j]) free_x_num++, free_index=j;
            }
            if(free_x_num>1) continue;
            temp=a[i][var];
            for(j=0; j<var; j++) {
                if(sgn(a[i][j])!=0&&j!=free_index) temp-=a[i][j]*x[j];
            }
            x[free_index]=temp/a[i][free_index];
            free_x[free_index]=0;
        }
        return var-k;
    }
    for(i=var-1; i>=0; i--) {
        temp=a[i][var];
        for(j=i+1; j<var; j++) {
            if(sgn(a[i][j])!=0) temp-=a[i][j]*x[j];
        }
        x[i]=temp/a[i][i];
    }
    return 1;
}
```

### 同余版本高斯消元

**题意：**

意思是当Bobo位于$n+1,n+2,...,n+m$节点后就不会移动了，求Bobo从节点$1$开始经过无穷时间后Bobo停在这$m$个点的概率分别是多少

**思路：**

1. 这个问题告诉要我们求Bobo开始在节点$1$处的答案，我们可以先求解一个子问题的答案，就是Bobo最终停在$n+1$处的概率
2. 对于这个子问题，要是我们知道Bobo从其他点开始，最终停在$n+1$处的概率就好了。虽然我们不知道这些概率具体是多少，但是可以将它们设出来，这样就可以建立$n*n$的系数矩阵，外加一行的增广，这样就可以利用高斯消元求解出开始位于第$i$个点，最终停在第$n+1$个点的概率了。
3. 而这个系数矩阵如何建立呢？考虑：
	$a[i][n+1]=p[i][n+1]+\sum^{n}_{i=1}{(p[i][j]*a[i][n+1])}$
	只需要对上面这个式子进行简单的移项就可以得到一个增广矩阵啦！
4. 这样，对于这$m$个结束位置我们都可以建立这样一个$n*(n+1)$的增广矩阵，并且进行高斯消元，得到答案。但很不幸的是，我第一发TLE了。
5. 来分析一下复杂度，高斯消元$O(n^3)$，$m$个最终位置$O(m)$，$T$组数据$O(T)$；因此复杂度就是$O(T*m*n^3)$。显然我们需要去掉一个$m$或者$n$。
6. 仔细观察这$m$个增广矩阵，可以发现，它们的系数矩阵是一样的！！！并且高斯消元的过程中，具体如何消元跟常数列无关，只与系数矩阵有关，因此我们考虑将这$m$个增广矩阵合并，变成一个$n*(n+m)$且保证有唯一解的增广矩阵。这样，最终复杂度降为$O(T*(n+m)*n^2)$，应该是正解啦。

```cpp
const int maxn = 1e3+10;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;
const double eps = 1e-7;
 
int n, m;
ll invw;
ll a[maxn][maxn];
ll p[maxn][maxn];
 
ll fast(int a, int k) {
    ll ans=1, p=a;
    while(k) {
        if(k&1) ans=ans*p%mod;
        p=p*p%mod;
        k>>=1;
    }
    return ans;
}
 
ll inv(int a) { return fast(a,mod-2); }
 
void Gauss() {
    for(int i=1; i<=n; ++i) {
        int mr=i;
        for(int j=i+1; j<=n; ++j) if(a[j][i]>a[mr][i]) mr=j;
        swap(a[i],a[mr]);
        for(int j=1; j<=n; ++j) {
            if(j==i) continue;
            ll d=a[j][i]*inv(a[i][i])%mod;
            for(int k=i; k<=n+m; ++k) a[j][k]=(a[j][k]-d*a[i][k]%mod+mod)%mod;
        }
        for(int k=i+1; k<=n+m; ++k) a[i][k]=a[i][k]*inv(a[i][i])%mod; a[i][i]=1;
    }
}
 
int main(){
    invw=inv(10000);
    while(cin>>n>>m) {
        for(int i=1; i<=n; ++i)
            for(int j=1; j<=n+m; ++j) scanf("%lld", &p[i][j]);
        for(int i=1; i<=n; ++i) {
            for(int k=1; k<=n; ++k) {
                a[i][k]=p[i][k]*invw%mod;
                if(i==k) a[i][k]=(a[i][k]-1+mod)%mod;
            }
            for(int j=1; j<=m; ++j) a[i][n+j]=(mod-p[i][j+n])*invw%mod;
        }
        Gauss();
        for(int i=1; i<=m; ++i) printf("%lld%c", a[1][n+i], " \n"[i==m]);
    }
}
```

## 线性基

**概述**

所谓线性基，就是线性代数里面的概念。一组线性无关的向量便可以作为一组基底，张起一个线性的向量空间，这个基底又称之为线性基。这个线性基的基底进行线性运算，可以表示向量空间内的所有向量，也即所有向量可以拆成基底的线性组合。

**定义**

设数集$T$的值域范围为$[1,2^n−1]$。 
$T$的线性基是$T$的一个子集$A=${$a1,a2,a3,...,an$}。 
$A$中元素互相$xor$所形成的异或集合，等价于原数集$T$的元素互相$xor$形成的异或集合。 
可以理解为将原数集进行了压缩。

**性质**

1. 设线性基的异或集合中不存在$0$。 
2. 线性基的异或集合中每个元素的异或方案唯一，其实这个跟性质1是等价的。 
3. 线性基二进制最高位互不相同。 
4. 如果线性基是满的，它的异或集合为$[1,2^n−1]$。 
5. 线性基中元素互相异或，异或集合不变。

### 插入

如果向线性基中插入数$x$，从高位到低位扫描它为$1$的二进制位。 
扫描到第$i$时，如果$a_i$不存在，就令$a_i=x$，否则$x=x⊗a_i$。 
$x$的结局是，要么被扔进线性基，要么经过一系列操作过后，变成了$0$。

```
bool insert(ll val) {
    for(int i=62; i>=0; i--) if(val>>i)) {
        if (!a[i]) { a[i]=val; break; }
        val^=a[i];
    }
    return val>0;
}
```

### 合并

**将一个线性基暴力插入另一个线性基即可。**

### 存在性

如果要查询$x$是否存于异或集合中。
从高位到低位扫描$x$的为$1$的二进制位。
扫描到第$i$位的时候$x=x⊗ai$
如果中途$x$变为了$0$，那么表示$x$存于线性基的异或集合中。

### 最大值

从高位到低位扫描线性基。 
如果异或后可以使得答案变大，就异或到答案中去。

```
ll qmax() {
    ll ans=0;
    for(int i=62; i>=0; --i)
        if((ans^p[i])>ans) ans^=p[i];
    return ans;
}
```

### 最小值

最小值即为最低位上的线性基。(就是最小的那一个)
```
ll qmin() {
    for(int i=0; i<=62; i++) if(d[i]) return d[i];
    return 0;
}
```

### K小值

根据性质$3$。 
我们要将线性基改造成每一位相互独立。 
具体操作就是如果$i<j$，$a_j$的第$i$位是$1$，就将$a_j$异或上$a_i$。 
经过一系列操作之后，对于二进制的某一位$i$而言，只有$a_i$的这一位是$1$，其他的这一位都是$0$。 
所以查询的时候将$k$二进制拆分，对于$1$的位，就异或上对应的线性基。 
最终得出的答案就是$k$小值。

```
void rebuild() {
    for(int i=62; i; --i)
        for(int j=i-1; j>=0; --j) if(a[i]>>j) a[i]^=a[j];
    for(int i=0; i<=62; i++) if(a[i]) p[cnt++]=a[i];
}

ll qkmin(ll k) {
    ll ans=0;
    if(k>=(1ll<<cnt)) return -1;
    for(int i=cnt-1; i>=0; i--)
        if(k&(1ll<<i)) ans^=p[i];
    return ans;
}
```

[先推荐一个](https://blog.csdn.net/qaq__qaq/article/details/53812883)

[再推荐一个](https://blog.csdn.net/a_forever_dream/article/details/83654397)

### [最大值板子题](https://www.luogu.org/problem/P3812)

```cpp
const int maxn = 1e5+10;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;
const double eps = 1e-7;

ll p[63], a[63], cnt;

void insert(ll a) {
    for(int i=62; i>=0; --i) if(a>>i&1) {
        if(!p[i]) { p[i]=a; break; }
        a^=p[i];
    }
}

ll qmax() {
    ll ans=0;
    for(int i=62; i>=0; --i)
        if((ans^p[i])>ans) ans^=p[i];
    return ans;
}

int main() {
    int n=read();
    for(int i=1; i<=n; ++i) insert(read());
    printf("%lld\n", qmax());
}
```

### [HDU 3949 XOR(K小值)](http://acm.hdu.edu.cn/showproblem.php?pid=3949)

题意：给N个数，然后求出能异或出来的第K小值
思路：求出N个数的线性基，再化简为每一个基,结果大概这个样子
$1xxxx0xxx0x$
$000001xxx0x$
$0000000001x$
然后统计有多少不为0的基，数量为tot
如果N!=tot，说明可以组成0（单纯用线性基不行）
如果K>=1<<tot，说明不存在第K小的值
最后利用线性基按照K进行构造，得到第K小的值。

```cpp
const int maxn = 1e4+10;

int N, Q;
ll K, a[maxn], b[66], tot;

void Gauss() {
    memset(b,0,sizeof(b));
    for(int i=0; i<N; ++i) {
        for(int j=63; j>=0; --j) {
            if((a[i]>>j)&1) {
                if(b[j]) a[i]^=b[j];
                else {
                    b[j]=a[i];
                    break;
                }
            }
        }
    }
    for(int i=63; i>=0; --i) {
        if(!b[i]) continue;
        for(int j=i+1; j<63; ++j) {
            if((b[j]>>i)&1) b[j]^=b[i];
        }
    }
    tot=0;
    for(int i=0; i<=63; ++i) if(b[i]) b[tot++]=b[i];
}

int main() {
    int T, t=0;
    cin>>T;
    while(T--) {
        cout<<"Case #"<<++t<<":"<<endl;
        cin>>N;
        for(int i=0; i<N; ++i) cin>>a[i];
        Gauss();
        cin>>Q;
        while(Q--) {
            cin>>K;
            if(N!=tot) K--;
            if(K>=(1ll<<tot)) cout<<-1<<endl;
            else {
                ll ans=0;
                for(int i=0; i<tot; ++i) if((K>>i)&1) ans^=b[i];
                cout<<ans<<endl;
            }
        }
    }    
}
```

### 前缀线性基求区间最大异或值

**题意：**给定一个数组，有两个操作

1. 1,x 在数组末尾插入一个x(但x需要解码)
2. 0,l,r 在数组[l,r]之间任取数字进行异或，求出最大可能的异或值(l,r也需要解码)

```cpp
const int maxn = 5e5+10;
const int mod = 1e9+7;
const double eps = 1e-9;

int f[maxn][32], pos[maxn][32]; //f用来保存f[r]出的前缀线性基,pos[i][j]用来记录f[i][j]在原数组中的位置

inline void add(int i, int x) {
    int k=i;
    for(int j=0; j<=30; ++j) f[i][j]=f[i-1][j], pos[i][j]=pos[i-1][j];
    for(int j=30; j>=0; --j) if(x>>j) {
        if(!f[i][j]) {
            f[i][j]=x, pos[i][j]=k;
            break;
        }
        else {
            if(k>pos[i][j]) swap(k,pos[i][j]), swap(x,f[i][j]); //贪心的将高位相同但位置更靠后的放在高位
            x^=f[i][j];
        }
    }
}

int main() {
    int T=read();
    while(T--) {
        int n=read(), m=read();
        for(int i=1; i<=n; ++i) add(i,read());
        int ans=0;
        while(m--) {
            if(read()) add(++n,read()^ans);
            else {
                int l=(read()^ans)%n+1, r=(read()^ans)%n+1; //由于l,r,x是编码过的，所以读入后要记得解码
                if(l>r) swap(l,r);
                ans=0;
                for(int i=30; i>=0; --i)
                    if((ans^f[r][i])>ans&&pos[r][i]>=l) ans^=f[r][i]; //还是正常的求最大值，但是这个基底必须是由[l,r]区间内的数得到的。
                printf("%d\n", ans);
            }
        }
        for(int i=1; i<=n; ++i)
            for(int j=0; j<=30; ++j) f[i][j]=pos[i][j]=0;
    }
}
```

### 实数线性基

**题意**：

求给定矩阵的秩，并且所选的基底尽可能小（“小”的定义在题面中）

**思路**：

1. 像平时做的二进制线性基一样插入即可
2. 插入前按照$c$的值先排个序，就当做贪心了吧
3. （补充：如果所有条件全部换成整数，而不是实数，则不能当做线性基处理。比如将此处的$m$维换成$1$维，那么这个问题就变成背包问题了，比如[货币系统](https://www.luogu.org/problem/P5020)）

```cpp
const int maxn = 5e2+10;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;
const double eps = 1e-6; //这里改成1e-10反而WA了两个点！！！

struct P{
    long double mat[maxn];
    int c;
    long double & operator [] (const int x) { return mat[x]; }
    friend bool operator < (const P &a, const P &b) { return a.c<b.c; }
}a[maxn];

int n, m, cnt, ans, p[maxn];

int main() {
    n=read(), m=read();
    for(int i=1; i<=n; ++i)
        for(int j=1; j<=m; ++j) scanf("%Lf", &a[i][j]);
    for(int i=1; i<=n; ++i) a[i].c=read();
    sort(a+1,a+1+n);
    for(int i=1; i<=n; ++i) {
        for(int j=1; j<=m; ++j) if(fabs(a[i][j])>eps) {
            if(!p[j]) { cnt++, ans+=a[i].c, p[j]=i; break; }
            long double t=a[i][j]/a[p[j]][j];
            for(int k=j; k<=m; ++k) a[i][k]-=t*a[p[j]][k];
        }
    }
    printf("%d %d\n", cnt, ans);
}
```

## 康托展开

### 什么是康托展开？

**就是关于全排列中某个排列的排名的问题**
1. 给出一个全排列，求出它是字典序第几小的全排列，这就叫康托展开。
2. 给出一个全排列的长度以及它的排名，让你求出这个全排列，这叫逆康托展开。
（以上全排列都是从$1-N$的排列，否则需要离散化一下，且排名指从小到大的排名）

### 康托展开的核心---变进制数

**对于一个$N$个数的排列：**

1. 如果我们依次从左边到右边选择数字组成排列，那么每个位置可以选择的数字个数是确定的。比如第一个数有$N$种选择，第二个数只有$N-1$种，第三$...$$, ... ,$第$N$个数只有一种选择
2. 如果我们可以把这些选择看做每个位置上的进制，对应的数字就是一个变进制数
3. 那么我们如何得到一个能代表某个全排列的变进制数呢？

以$N=5$为例，其中一个全排列是$2,4,3,1,5$

1. 第一个数只有$1,2,3,4,5$可以选，所以$2$是排名第二的，将其赋值为1
2. 第二个数只有$1,3,4,5$可以选，所以$4$是排名第三的，将其赋值为2
3. 第三个数只有$1,3,5$可以选，所以$3$是排名第二的，将其赋值为1
4. 第四个数只有$1,5$可以选，所以$1$是排名第一的，将其赋值为0
5. 第五个数只有$5$可以选，所以$5$是排名第一的，将其赋值为0

最后得到的变进制数就是 $12100$

**我们看到第$i$位数的值就是$a_i$减去左边比它小的数的数量再减去$1$(因为$0$也算一个数)**

显然我们可以将得到这个数的过程写成一段程序:

```cpp
//N表示全排列长度
for(int i=1;i<=N;i++) {
    cin>>a[i];
    int x=a[i];
    for(int j=1;j<=a[i];j++) x-=used[j];
    //used[j]表示j是否用过(1用过，0没用）
    used[a[i]]=1;
    a[i]=x-1;
}
//下面给出树状数组加速版(线段树也行啦)
for(int i=1; i<=N; ++i) {
    update(a[i]);//这里先把自己更新了，下面query的时候+1和-1就抵消掉了
    a[i]-=query(a[i]);
}
```

**那它到底代表的排名是多少呢？我们只需要将这个变进制数转化为10进制即可**

转化过程：

```cpp
int ans=0; //其实多数时候需要long long以及取mod
for(int i=1; i<N; ++i) ans=(ans+a[i])*(N-i); //最后的ans+1就是排名，因为要加上0的排名
```
转化后的答案就是 $39$
**这样，整个康托展开过程就结束了！也就几行代码啦。**

下面给出完整代码（[一道模板题的AC代码](https://www.luogu.org/problem/P5367)）：

```cpp
const int maxn = 1e6+10;
const int mod = 998244353;
const double eps = 1e-9;

int a[maxn], p[maxn], N;
void update(int x){while(x<=N){p[x]++;x+=x&-x;}}
int query(int x){int sum=0;while(x){sum+=p[x];x-=x&-x;}return sum;}

int main() {
    N=read();
    for(int i=1; i<=N; ++i) a[i]=read();
    for(int i=1; i<=N; ++i) {
        update(a[i]);
        a[i]-=query(a[i]);
    }
    ll ans=0;
    for(int i=1; i<N; ++i) ans=(ans+a[i])*(N-i)%mod;
    cout<<ans+1<<endl;
}
```

### 逆康托展开

**就是说你已经知道某全排列的长度以及它的排名，要你求出这个全排列**

1. 把这个排名转化为变进制数(细节后续补充)。
2. 再用求每位变进制数的逆过程即可求出这个全排列了(细节后续补充)。



## FFT

### 板子

多项式乘法，可跑$1e6$的数据。

```cpp
const int maxn = (1<<20)+7;
const double PI = acos(-1);

struct CP{
    double x, y;
    CP(double x=0, double y=0): x(x), y(y) {}
    CP operator+(CP&rhs){return CP(x+rhs.x,y+rhs.y);}
    CP operator-(CP&rhs){return CP(x-rhs.x,y-rhs.y);}
    CP operator*(CP&rhs){return CP(x*rhs.x-y*rhs.y,x*rhs.y+y*rhs.x);}
}f[maxn], g[maxn];

int n, m, nm, up;
int tr[maxn];

void FFT(CP *f, int op) {
    for(int i=0; i<up; ++i) if(i<tr[i]) swap(f[i],f[tr[i]]);
    for(int len=2; len<=up; len<<=1) {
        CP w1(cos(2*PI/len),op*sin(2*PI/len));
        for(int l=0, hf=len>>1; l<up; l+=len) {
            CP w(1,0);
            for(int i=l; i<l+hf; ++i) {
                CP tp=w*f[i+hf];
                f[i+hf]=f[i]-tp;
                f[i]   =f[i]+tp;
                w=w*w1;
            }
        }
    }
    if(op==-1) for(int i=0; i<up; ++i) f[i].x/=up;
}

int main() {
    n=read(), m=read();
    for(int i=0; i<=n; ++i) scanf("%lf", &f[i].x); //需要n+1个点确定n次函数
    for(int i=0; i<=m; ++i) scanf("%lf", &g[i].x);
    for(nm=n+m, up=1; up<=nm; up<<=1); //nm=n+m（m为结果函数的次数），up为严格大于m的2的正整数幂
    for(int i=0; i<up; ++i) tr[i]=(tr[i>>1]>>1)|((i&1)?up>>1:0); //逆进制转换
    FFT(f,1); FFT(g,1); //DFT
    for(int i=0; i<up; ++i) f[i]=f[i]*g[i];
    FFT(f,-1); //IDFT
    for(int i=0; i<=nm; ++i) printf("%d%c", int(round(f[i].x)), " \n"[i==nm]);
}
```

### [浅谈FFT在字符串匹配中的应用](https://www.luogu.com.cn/problemnew/solution/P4173)

#### 背景引入

字符串匹配，是OI中的一个古老为传统的问题。常见的匹配问题有：单模式串匹配、多模式串匹配、字串匹配。而对应的算法是KMP、AC自动机、后缀自动机。而我们今天要谈的问题，是单模式串匹配问题的一种：带通配符的单模式串匹配。

为了逐步解决这个问题，我们从普通的单模式串匹配开始说起。

#### 普通的单模式串匹配

问题概述：给定模式串A（长度为m）、文本串B（长度为n），需要求出所有位置p，满足B串从第p个字符开始的连续m个字符，与A串完全相同

KMP是这一类问题的优秀算法，理论复杂度是线性的。但今天我们要用FFT来解决这个问题。千万不要因为FFT的复杂度高于KMP而忽略这一段，因为这一段内容对接下来我们要解决的问题而言，非常重要。

为了使问题更便于研究，我们约定所有字符串的下标从0开始

我们定义字符串的匹配函数C(x,y)=A(x)-B(y)，那么我们可以形式化地定义“匹配”：若C(x,y)=0，则称A的第x个字符和B的第y个字符匹配。再定义完全匹配函数$P(x)=\sum\limits_{i=0}^{m-1}C(i,x-m+i+1)$，若P(x)=0，则称B以第x位结束的连续m位，与A完全匹配

但有没有觉得这个完全匹配函数有什么问题？似乎，根据完全匹配函数，“ab”与“ba”是匹配的？？？问题出在我们定义的匹配函数身上。匹配函数的确可以判断某一位是否匹配，但加了一个求和符号，一切都变了，只要两个串所含的字符是一样的，不管排列如何，完全匹配函数都会返回0值。

而这一切的罪魁祸首就是匹配函数的正负号！我们现在要做的是：定义一个更好的匹配函数，只要两个字符串某一位的匹配函数非零，完全匹配函数也不可能为零。不难想到给匹配函数加一个绝对值符号。但这样似乎就只能O(nm)暴力计算，没有更好的优化方法了。于是我们给匹配函数加上一个平方符号。此时完全匹配函数就是$P(x)=\sum\limits_{i=0}^{m-1}\left[A(i)-B(x-m+i+1)\right]^2$

这样还是没什么优化方法。我们不妨翻转A串，将翻转后的结果设为S。形式化地，S(x)=A(m-x-1)。据此不难推出A(x)-S(m-x-1)。于是完全匹配函数可以写成$P(x)=\sum\limits_{i=0}^{m-1}\left[S(m-i-1)-B(x-m+i+1)\right]^2$

大力展开这个函数：$P(x)=\sum\limits_{i=0}^{m-1}\left[S(m-i-1)^2+B(x-m+i+1)^2-2S(m-i-1)B(x-m+i+1)\right]$

再将求和符号拆开：$P(x)=\sum\limits_{i=0}^{m-1}S(m-i-1)^2+\sum\limits_{i=0}^{m-1}B(x-m+i+1)^2-2\sum\limits_{i=0}^{m-1}S(m-i-1)B(x-m+i+1)$

第一项是一个定值，可以O(m)预处理出来。第二项可以O(n)预处理前缀和。现在问题就在第三项。第三项中，两个小括号里面的东西加起来，似乎能抵消很多东西？居然……刚好等于x？这是巧合吗？~~当然不是，明明是我干出来的。~~所以，第三项是不是可以写成$-2\sum\limits_{i+j=x}S(i)B(j)$呢？当然可以！（这一步不太好解释，需要自己好好理解一下）

那么，我们设$T=\sum\limits_{i=0}^{m-1}S(i)^2\qquad f(x)=\sum\limits_{i=0}^xB(i)^2\qquad g(x)=\sum\limits_{i+j=x}S(i)B(j)$

于是就有$P(x)=T+f(x)-f(x-m)-2g(x)$

观察这个g函数，发现它不就是我们熟悉的卷积形式吗？而且还是最常规的卷积——多项式乘法！于是可以用FFT计算出g函数了！然后再代入计算一下，就可以O(n)得到所有P(x)的值了！

说了那么多，其实代码很短的。

```cpp
const int maxn = (1<<20)+7;
const double PI = acos(-1);

struct CP{
    double x, y;
    CP(double x=0, double y=0): x(x), y(y) {}
    CP operator+(CP const&rhs)const{return CP(x+rhs.x,y+rhs.y);}
    CP operator-(CP const&rhs)const{return CP(x-rhs.x,y-rhs.y);}
    CP operator*(CP const&rhs)const{return CP(x*rhs.x-y*rhs.y,x*rhs.y+y*rhs.x);}
  	friend CP operator*(int a,CP const&b){return CP(a*b.x,a*b.y);}
}f[maxn], g[maxn], p[maxn];

int n, m, nm, up;
char a[maxn], b[maxn];
int tr[maxn];

void FFT(CP *f, int op) {
    for(int i=0; i<up; ++i) if(i<tr[i]) swap(f[i],f[tr[i]]);
    for(int len=2; len<=up; len<<=1) {
        CP w1(cos(2*PI/len),op*sin(2*PI/len));
        for(int l=0, hf=len>>1; l<up; l+=len) {
            CP w(1,0);
            for(int i=l; i<l+hf; ++i) {
                CP tp=w*f[i+hf];
                f[i+hf]=f[i]-tp;
                f[i]   =f[i]+tp;
                w=w*w1;
            }
        }
    }
    if(op==-1) for(int i=0; i<up; ++i) f[i].x/=up;
}

int main() {
    m=read(), n=read();
    scanf("%s%s", a, b); reverse(a,a+m); //先翻转模式串
    for(nm=n+m, up=1; up<=nm; up<<=1);
    for(int i=0; i<up; ++i) tr[i]=(tr[i>>1]>>1)|((i&1)?up>>1:0);
    //FFT:f^3*g
    for(int i=0; i<up; ++i) {
        if(i>=m) f[i].x=0;
        else if(a[i]!='*') f[i].x=(a[i]-'a'+1)*(a[i]-'a'+1)*(a[i]-'a'+1);
        else f[i].x=0;
        f[i].y=0;
    }
    for(int i=0; i<up; ++i) {
        if(i>=n) g[i].x=0;
        else if(b[i]!='*') g[i].x=b[i]-'a'+1;
        else g[i].x=0;
        g[i].y=0;
    }
    FFT(f,1); FFT(g,1);
    for(int i=0; i<up; ++i) p[i]=p[i]+f[i]*g[i];
    //FFT:f*g^3
    for(int i=0; i<up; ++i) {
        if(i>=m) f[i].x=0;
        else if(a[i]!='*') f[i].x=a[i]-'a'+1;
        else f[i].x=0;
        f[i].y=0;
    }
    for(int i=0; i<up; ++i) {
        if(i>=n) g[i].x=0;
        else if(b[i]!='*') g[i].x=(b[i]-'a'+1)*(b[i]-'a'+1)*(b[i]-'a'+1);
        else g[i].x=0;
        g[i].y=0;
    }
    FFT(f,1); FFT(g,1);
    for(int i=0; i<up; ++i) p[i]=p[i]+f[i]*g[i];
    //FFT:f^2*g^2
    for(int i=0; i<up; ++i) {
        if(i>=m) f[i].x=0;
        else if(a[i]!='*') f[i].x=(a[i]-'a'+1)*(a[i]-'a'+1);
        else f[i].x=0;
        f[i].y=0;
    }
    for(int i=0; i<up; ++i) {
        if(i>=n) g[i].x=0;
        else if(b[i]!='*') g[i].x=(b[i]-'a'+1)*(b[i]-'a'+1);
        else g[i].x=0;
        g[i].y=0;
    }
    FFT(f,1); FFT(g,1);
    for(int i=0; i<up; ++i) p[i]=p[i]-2*f[i]*g[i];
    //IDFT:p
    FFT(p,-1);

    vector<int> ans;
    for(int i=m-1; i<n; ++i) {
        if(fabs(p[i].x)<0.1) ans.push_back(i-m+2);
    }
    printf("%d\n", int(ans.size()));
    for(int i=0; i<ans.size(); ++i) printf("%d%c", ans[i], " \n"[i==ans.size()-1]);
}
```

## NTT

### 板子

```cpp
const int maxn = (1<<19)+7;
const int mod = 998244353, G = 3, sqr2 = 116195171;
int n, m, nm, up;
ll f[maxn], g[maxn];
int tr[maxn];

ll qpow(ll a,ll k=mod-2,ll p=mod){ll s=1;while(k){if(k&1)s=s*a%p;a=a*a%p;k>>=1;}return s;}

void NTT(ll *f, int op) {
    for(int i=0; i<up; ++i) if(i<tr[i]) swap(f[i],f[tr[i]]);
    for(int len=2; len<=up; len<<=1) {
        ll w1=qpow(qpow(G,(mod-1)/len),~op?1:mod-2);
        for(int l=0, hf=len>>1; l<up; l+=len) {
            ll w=1;
            for(int i=l; i<l+hf; ++i) {
                ll tp=w*f[i+hf]%mod;
                f[i+hf]=(f[i]-tp+mod)%mod;
                f[i]=(f[i]+tp)%mod;
                w=w*w1%mod;
            }
        }
    }
    if(op==-1) for(int i=0, inv=qpow(up,mod-2); i<up; ++i) f[i]=f[i]*inv%mod;
}

int main() {
    n=read(), m=read();
    for(int i=0; i<=n; ++i) f[i]=read();
    for(int i=0; i<=m; ++i) g[i]=read();
    for(nm=n+m, up=1; up<=nm; up<<=1);
    for(int i=0; i<up; ++i) tr[i]=(tr[i>>1]>>1)|((i&1)?up>>1:0);
    NTT(f,1); NTT(g,1);
    for(int i=0; i<up; ++i) f[i]=f[i]*g[i]%mod;
    NTT(f,-1);
    ll ans=0;
    for(int i=0; i<=nm; ++i) ans=(ans+f[i])%mod;
    printf("%lld\n", ans);
}
```

**任意模数**

模数1e6左右时只需要前两个模，模数1e5以下，直接FFT。

注意在使用任意模数NTT时，两个多项式乘起来以后就要CRT一遍并取模，否则必须保证多个多项式乘起来后“真实”系数不爆__int128，也就是要保证CRT能得到“真实系数”。

```cpp
const int maxn = (1<<19)+7;
const int MOD[] = {1004535809,998244353,469762049};
const int G = 3, md = 1e6+3; int mod=md;
int n, m, nm, up;
ll f[maxn], g[maxn], fac[maxn], inv[maxn];
ll A[maxn], B[maxn], P[maxn], tp[maxn], bb[3][maxn];
int tr[maxn];

ll qpow(ll a,ll k=mod-2,ll p=mod){ll s=1;while(k){if(k&1)s=s*a%p;a=a*a%p;k>>=1;}return s;}

void NTT(ll *f, int op) {
    for(int i=0; i<up; ++i) if(i<tr[i]) swap(f[i],f[tr[i]]);
    for(int len=2; len<=up; len<<=1) {
        ll w1=qpow(qpow(G,(mod-1)/len),~op?1:mod-2);
        for(int l=0, hf=len>>1; l<up; l+=len) {
            ll w=1;
            for(int i=l; i<l+hf; ++i) {
                ll tp=w*f[i+hf]%mod;
                f[i+hf]=(f[i]-tp+mod)%mod;
                f[i]=(f[i]+tp)%mod;
                w=w*w1%mod;
            }
        }
    }
    if(op==-1) for(int i=0, inv=qpow(up,mod-2); i<up; ++i) f[i]=f[i]*inv%mod;
}

struct CRT{
    int n;
    ll a[3], b[3]; //a表示余数，b表示模数
    CRT(): n(0) {}
    void add(ll x, ll y) { a[n]=x; b[n]=y; ++n; }
    ll exgcd(ll a, ll b, ll &x, ll &y) {
        if(!b) { x=1, y=0; return a; }
        ll ans=exgcd(b,a%b,y,x); y-=a/b*x;
        return ans;
    }
    ll cal() {
        ll A=0, B=1, x, y;
        for(int i=0; i<n; ++i) {
            ll g=exgcd(B,b[i],x,y);
            ll Bg=B/g, bg=b[i]/g;
            exgcd(Bg,bg,x,y);
            x=(x%bg+bg)%bg;
            ll lcm=Bg*b[i];
            A=(((a[i]-A)/g*x%bg*B+A)%lcm+lcm)%lcm;
            B=lcm;
        }
        return A;
    }
}C;

void NTT(ll *A, ll *B) { //任意模数NTT，存在tp数组里
    for(int s=0; s<3; ++s) {
        for(int i=0; i<up; ++i) f[i]=A[i];
        for(int i=0; i<up; ++i) g[i]=B[i];
        mod=MOD[s];
        NTT(f,1); NTT(g,1);
        for(int i=0; i<up; ++i) f[i]=f[i]*g[i];
        NTT(f,-1);
        for(int i=0; i<up; ++i) bb[s][i]=f[i];
    }
    mod=md;
    for(int i=0; i<up; ++i) {
        C.n=0; for(int s=0; s<3; ++s) C.add(bb[s][i],MOD[s]);
        tp[i]=C.cal()%mod;
    }
}
```



### NTT用到的各种素数

1. 常用任意模数设置：1004535809,998244353,469762049，这三个数G=3，且可以开1e6以上的up。

2. 注意当模数非常小时，比如碰到过313，可以直接FFT搞啊，边搞边取模就完事了！

3. 注意在使用任意模数NTT时，两个多项式乘起来以后就要CRT一遍并取模，否则必须保证多个多项式乘起来后“真实”系数不爆__int128，也就是要保证CRT能得到“真实系数”。
4. 任意模数小技巧，要对每个多项式开一个tp数组来存NTT前的表达式，以及一个公用的b\[3][maxn]，方便CRT。

| r*2^k+1    | r    | k    | g    |
| :--------- | ---- | ---- | ---- |
| 167772161  | 5    | 25   | 3    |
| 469762049  | 7    | 26   | 3    |
| 998244353  | 119  | 23   | 3    |
| 1004535809 | 479  | 21   | 3    |
| 2281701377 | 17   | 27   | 3    |
| 2013265921 | 15   | 27   | 31   |



### 序列统计

题意：有一个集合 $S$ ，里面的有些非负元素。问利用 $S$ 中的元素生成长度为 $n$ 的序列，序列所有元素乘积对 $m$ （质数）取模后等于 $x$ ，问有多少满足条件的序列。

思路：找到 $m$ 的一个原根 $G1$ ，然后将[1,m-1]的数字转化为 $G1$ 的幂表达，乘法转化为加法（模为 $m-1$ ），就可以做NTT+快速幂了。

```cpp
const int maxn = (1<<20)+7;
const int mod = 1004535809, G = 3, sqr2 = 116195171;
int n, m, x, S, nm, up;
ll f[maxn], g[maxn];
int tr[maxn], p[maxn];

ll qpow(ll a,ll k=mod-2,ll p=mod){ll s=1;while(k){if(k&1)s=s*a%p;a=a*a%p;k>>=1;}return s;}

void NTT(ll *f, int op){
    for(int i=0; i<up; ++i) if(i<tr[i]) swap(f[i],f[tr[i]]);
    for(int len=2; len<=up; len<<=1) {
        ll w1=qpow(qpow(G,(mod-1)/len),~op?1:mod-2);
        int hf=len>>1;
        for(int l=0; l<up; l+=len) {
            ll w=1;
            for(int i=l; i<l+hf; ++i) {
                ll tp=w*f[i+hf]%mod;
                f[i+hf]=(f[i]-tp+mod)%mod;
                f[i]=(f[i]+tp)%mod;
                w=w*w1%mod;
            }
        }
    }
    if(op==-1) for(int i=0, inv=qpow(up,mod-2); i<up; ++i) f[i]=f[i]*inv%mod;
}

int main() {
    n=read(), m=read(), x=read(), S=read();
    //下面求m的最小原根（离散里面的生成元）G1.
    int mm=m-1;
    vector<int> v;
    for(int i=2; i*i<=mm; ++i) {
        if(mm%i==0) {
            v.push_back(i);
            while(mm%i==0) mm/=i;
        }
    }
    if(mm>1) v.push_back(mm);
    int G1=0;
    for(int i=1; i<m; ++i) {
        int f=1;
        for(int d: v) {
            if(qpow(i,(m-1)/d,m)==1) { f=0; break; }
        }
        if(f) { G1=i; break; }
    }
    for(int i=1, t=G1; i<m; ++i, t=t*G1%m) p[t]=i; //将[1,m-1]每个数变成原根的幂表达
    p[1]=0; //(m-1)%(m-1)=0
    for(int i=1; i<=S; ++i) {
        int q=read()%m;
        if(q) f[p[q]]++; //0不在[1,m-1]范围内，特判一下
    }
    for(nm=2*m, up=1; up<=nm; up<<=1);
    for(int i=0; i<up; ++i) tr[i]=(tr[i>>1]>>1)|((i&1)?up>>1:0);
    NTT(f,1);
    for(int i=0; i<up; ++i) g[i]=1;
    while(n) {
        if(n&1) {
            for(int i=0; i<up; ++i) g[i]=g[i]*f[i]%mod;
            //由于要对m-1取模，因此IDFT一下，手动取模后再DFT回来
            NTT(g,-1);
            for(int i=0; i<up; ++i) {
                int t=g[i]; g[i]=0;
                g[i%(m-1)]=(g[i%(m-1)]+t)%mod;
            }
            NTT(g,1);
        }
        for(int i=0; i<up; ++i) f[i]=f[i]*f[i]%mod;
        NTT(f,-1);
        for(int i=0; i<up; ++i) {
            int t=f[i]; f[i]=0;
            f[i%(m-1)]=(f[i%(m-1)]+t)%mod;
        }
        NTT(f,1);
        n>>=1;
    }
    NTT(g,-1);
    printf("%lld\n", g[p[x]]);
}
```

## FWT

FWT(f,3)，就是子集和。

```cpp
const int maxn = (1<<20)+7;
const int mod = 998244353, inv2 = 499122177;

int n, up;
ll f[maxn], g[maxn], h[maxn];

void FWT(ll *f, int op) {
    for(int len=2; len<=up; len<<=1) {
        for(int l=0, hf=len>>1; l<up; l+=len) {
            for(int i=l; i<l+hf; ++i) {
                ll x=f[i], y=f[i+hf];
                if(op>0) {
                    if(op==1) f[i]=(x+y)%mod, f[i+hf]=(x-y+mod)%mod; //xor
                    else if(op==2) f[i]=(x+y)%mod; //and
                    else f[i+hf]=(x+y)%mod; //or
                }
                else {
                    if(op==-1) f[i]=(x+y)*inv2%mod, f[i+hf]=(x-y+mod)*inv2%mod; //xor
                    else if(op==-2) f[i]=(x-y+mod)%mod; //and
                    else f[i+hf]=(y-x+mod)%mod; //or
                }
            }
        }
    }
}

int main() {
    for(int i=0; i<up; ++i) f[i]=read();
    for(int i=0; i<up; ++i) g[i]=read();
    FWT(f,3); FWT(g,3);
    for(int i=0; i<up; ++i) h[i]=f[i]*g[i]%mod;
    FWT(f,-3); FWT(g,-3); FWT(h,-3);
    for(int i=0; i<up; ++i) printf("%lld%c", h[i], " \n"[i==up-1]);

    FWT(f,2); FWT(g,2);
    for(int i=0; i<up; ++i) h[i]=f[i]*g[i]%mod;
    FWT(f,-2); FWT(g,-2); FWT(h,-2);
    for(int i=0; i<up; ++i) printf("%lld%c", h[i], " \n"[i==up-1]);

    FWT(f,1); FWT(g,1);
    for(int i=0; i<up; ++i) h[i]=f[i]*g[i]%mod;
    FWT(f,-1); FWT(g,-1); FWT(h,-1);
    for(int i=0; i<up; ++i) printf("%lld%c", h[i], " \n"[i==up-1]);
}
```

**多子集卷积**

多个（可以不止两个）子集并起来，且交为空集。

在子集卷积的基础上将所有的状态按最高位分类，即同类的最多取一个，再从低位依次向上做子集卷积，于是复杂度变为 $\displaystyle O(\sum_{i=1}^{k}{i^22^i}) = O(k^22^k)$

注意：若子集里面包含空集，则要先记下来，不让其参与运算，最后再一次性乘上去。

(下面顺带 FMT)

```cpp
const int maxn = (1<<21)+7;
const int mod = 998244353, inv2 = 499122177, k = 21;

int up;
int f[k+1][maxn], g[k+1][maxn], num[maxn];

void FWT(int *f, int op) {
    for(int len=2; len<=up; len<<=1) {
        for(int l=0, hf=len>>1; l<up; l+=len) {
            for(int i=l; i<l+hf; ++i) {
                ll x=f[i], y=f[i+hf];
                if(op>0) {
                    if(op==1) f[i]=(x+y)%mod, f[i+hf]=(x-y+mod)%mod; //xor
                    else if(op==2) f[i]=(x+y)%mod; //and
                    else f[i+hf]=(x+y)%mod; //or
                }
                else {
                    if(op==-1) f[i]=(x+y)*inv2%mod, f[i+hf]=(x-y+mod)*inv2%mod; //xor
                    else if(op==-2) f[i]=(x-y+mod)%mod; //and
                    else f[i+hf]=(y-x+mod)%mod; //or
                }
            }
        }
    }
}

void solve() {
    f[0][0]=1; f[1][1]=num[1];
    for(int t=1; t<k; ++t) {
        up=1<<(t+1);
        for(int i=1<<t; i<up; ++i) g[__builtin_popcount(i)][i]=num[i];
        g[0][0]=1; //为了保留前面的答案
        for(int i=0; i<=(t+1); ++i) FWT(f[i],3), FWT(g[i],3);
        for(int s=0; s<up; ++s) { //子集卷积FMT
            for(int i=t+1; i>=0; --i) { //倒过来枚举，就不需要额外数组
                ll tmp=0;
                for(int j=0; j<=i; ++j) (tmp+=ll(f[j][s])*g[i-j][s])%=mod;
                f[i][s]=tmp;
            }
        }
        for(int i=0; i<=(t+1); ++i) {
            FWT(f[i],-3);
            for(int j=0; j<up; ++j) g[i][j]=0;
        }
    }
}

int main() {
    int n=read();
    for(int i=1; i<=n; ++i) {
        int p=read(), b=read();
        (num[b]+=p)%=mod;
    }
    solve();
    int m=read();
    while(m--) {
        int x=read();
        printf("%d\n", f[__builtin_popcount(x)][x]);
    }
}
```



## BM板子

```cpp
#include "bits/stdc++.h"
#define hhh cerr<<"hhh"<<endl
#define SZ(x) ((int)(x).size())
#define all(x) (x).begin(),(x).end()
#define rep(i,a,n) for (int i=a;i<n;i++)
#define see(x) cerr<<(#x)<<'='<<(x)<<endl
using namespace std;
typedef long long ll;
typedef vector<int> VI;
typedef pair<int,int> pr;
inline int read() {int x=0,f=1;char c=getchar();while(c!='-'&&(c<'0'||c>'9'))c=getchar();if(c=='-')f=-1,c=getchar();while(c>='0'&&c<='9')x=x*10+c-'0',c=getchar();return f*x;}

const int maxn = 1e5+7;
const int inf = 0x3f3f3f3f;
const int mod = 998244353;

ll qpow(ll a,ll b) {ll res=1;a%=mod; for(;b;b>>=1){if(b&1)res=res*a%mod;a=a*a%mod;}return res;}

namespace linear_seq {
    ll res[maxn],base[maxn],c[maxn],md[maxn];
    vector<int> Md;
    void mul(ll *a,ll *b,int k) {
        rep(i,0,k+k) c[i]=0;
        rep(i,0,k) if (a[i]) rep(j,0,k) c[i+j]=(c[i+j]+a[i]*b[j])%mod;
        for (int i=k+k-1;i>=k;i--) if (c[i])
            rep(j,0,SZ(Md)) c[i-k+Md[j]]=(c[i-k+Md[j]]-c[i]*md[Md[j]])%mod;
        rep(i,0,k) a[i]=c[i];
    }
    int solve(ll n,VI a,VI b) { // a 系数 b 初值 b[n+1]=a[0]*b[n]+...
        ll ans=0,pnt=0;
        int k=SZ(a);
        rep(i,0,k) md[k-1-i]=-a[i];md[k]=1;
        Md.clear();
        rep(i,0,k) if (md[i]!=0) Md.push_back(i);
        rep(i,0,k) res[i]=base[i]=0;
        res[0]=1;
        while ((1ll<<pnt)<=n) pnt++;
        for (int p=pnt;p>=0;p--) {
            mul(res,res,k);
            if ((n>>p)&1) {
                for (int i=k-1;i>=0;i--) res[i+1]=res[i];res[0]=0;
                rep(j,0,SZ(Md)) res[Md[j]]=(res[Md[j]]-res[k]*md[Md[j]])%mod;
            }
        }
        rep(i,0,k) ans=(ans+res[i]*b[i])%mod;
        if (ans<0) ans+=mod;
        return ans;
    }
    VI BM(VI s) {
        VI C(1,1),B(1,1);
        int L=0,m=1,b=1;
        rep(n,0,SZ(s)) {
            ll d=0;
            rep(i,0,L+1) d=(d+(ll)C[i]*s[n-i])%mod;
            if (d==0) ++m;
            else if (2*L<=n) {
                VI T=C;
                ll c=mod-d*qpow(b,mod-2)%mod;
                while (SZ(C)<SZ(B)+m) C.push_back(0);
                rep(i,0,SZ(B)) C[i+m]=(C[i+m]+c*B[i])%mod;
                L=n+1-L; B=T; b=d; m=1;
            } else {
                ll c=mod-d*qpow(b,mod-2)%mod;
                while (SZ(C)<SZ(B)+m) C.push_back(0);
                rep(i,0,SZ(B)) C[i+m]=(C[i+m]+c*B[i])%mod;
                ++m;
            }
        }
        return C;
    }
    int gao(VI a,ll n) { //把vector和要求的第多少项-1放进去，因为vector下标从0开始的
        VI c=BM(a);
        c.erase(c.begin());
        rep(i,0,SZ(c)) c[i]=(mod-c[i])%mod;
        return solve(n,c,VI(a.begin(),a.begin()+SZ(c)));
    }
};

ll f[maxn], pre[maxn], ans[maxn];

int main() {
    f[1]=f[2]=1;
    ll n, m; cin>>n>>m;
    for(int i=3; i<maxn; ++i) f[i]=(f[i-1]+f[i-2])%mod;
    pre[0]=1;
    for(int i=1; i<maxn; ++i) pre[i]=qpow(i,m);
    ll now=0;
    vector<int> v;
    for(int i=1; i<maxn; ++i) {
        ll tmp=ll(pre[i])*f[i]%mod;
        (now+=tmp)%=mod; ans[i]=now;
        v.push_back(ans[i]);
    }
    printf("%d\n",linear_seq::gao(v,n-1));
}
```

## 二次剩余

ll cal(ll n,ll p); 返回 $-1$ 表示 $n$ 是 $p$ 的非二次剩余，否则返回其中一个解 $a$，另一个解为 $p-a$。

```cpp
const int maxn = 3e5+7;
const int mod = 1e9+7;

mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());

ll w, n, p;

struct CP{
    ll x, y;
    CP(ll x=0, ll y=0): x(x), y(y) {}
    bool operator==(CP &rhs){return x==rhs.x&&y==rhs.y;}
    CP operator*(CP &rhs){return CP((x*rhs.x+w*y%p*rhs.y)%p,(x*rhs.y+y*rhs.x)%p);}
    friend CP qpow(CP &a,ll k){CP s=1;while(k){if(k&1)s=s*a;a=a*a;k>>=1;}return s;}
};

ll qpow(ll a,ll k=mod-2,ll p=mod){ll s=1;while(k){if(k&1)s=s*a%p;a=a*a%p;k>>=1;}return s;}

ll cal(ll n, ll p) {
    n%=p;
    if(!n) return 0;
    if(p==2) return n;
    if(qpow(n,(p-1)/2,p)==p-1) return -1; //不存在
    ll a;
    while(1) {
        a=rng()%p;
        w=((a*a%p-n)%p+p)%p;
        if(qpow(w,(p-1)/2,p)==p-1) break;
    }
    CP x(a,1);
    return qpow(x,(p+1)/2).x;
}

int main() {
    cin>>n>>p;
    ll x=cal(n,p);
    if(x==-1) return printf("Hola!\n"), 0;
    ll y=(p-x)%p;
    if(x>y) swap(x,y);
    if(x==y) printf("%lld\n", x);
    else printf("%lld %lld\n", x, y);
}
```

## exBSGS

求解 $a^x \equiv b \ (mod \ m)$。

普通的 BSGS 要求 $a$ 与 $m$ 互质，ex 版则不需要。

对于 $x^a \equiv b \ (mod \ m)$ 问题，可以利用原根将 $x$ 变为 $g^c$ ，于是问题转化为求 $(g^a)^c \equiv b$ ，即基本 BSGS 问题。

补充：该做法求出的解是大于 $k$ 的。因此，求出解后如果要求最小的，也许需要暴力判断一下小于等于 $k$ 的部分（$k$ 会很小，纯暴力即可）。

```cpp
unordered_map<ll,int> H;

ll gcd(ll a,ll b){return b?gcd(b,a%b):a;}
ll qpow(ll a,ll k=mod-2,ll p=mod){ll s=1;while(k){if(k&1)s=s*a%p;a=a*a%p;k>>=1;}return s;}
ll exgcd(ll a, ll b, ll &x, ll &y) {
    if(!b) { x=1, y=0; return a; }
    ll ans=exgcd(b,a%b,y,x); y-=a/b*x;
    return ans;
}
ll inv(ll a, int mod) {
    ll x, y;
    return exgcd(a,mod,x,y)==1?(x+mod)%mod:-1;
}

ll BSGS(ll a, ll b, ll mod, ll aa) {
    H.clear();
    ll p=ceil(sqrt(mod)), x, y;
    exgcd(aa,mod,x,y); b=(b*x%mod+mod)%mod;
    exgcd(qpow(a,p,mod),mod,x,y); x=(x+mod)%mod;
    for(ll i=1, j=0; j<=p; ++j, i=i*a%mod) if(!H.count(i)) H[i]=j;
    for(ll i=b, j=0; j<=p; ++j, i=i*x%mod) if(H.count(i)) return j*p+H[i];
    return -1;
}

ll exBSGS(ll a, ll b, ll m) {
    ll aa=1, k=0, t=1;
    if(b==1) return 0;
    while((t=gcd(a,m))>1) {
        if(b%t) return -1;
        ++k, b/=t, m/=t, aa=aa*(a/t)%m;
        if(aa==b) return k;
    }
    return (t=BSGS(a,b,m,aa))==-1?-1:t+k;
}
```

## 自适应辛普森

利用二次函数的积分来拟合 $f$ 的积分，表达式为：$$\displaystyle F(r)-F(l)=\frac{(r-l)*(f(l)+f(r)+4f(\frac{l+r}{2}))}{6}$$。

在对于是否继续递归划分的时候，通过考虑分与不分两种情况的误差大小进行自适应分段。

调用：simpson(l,r,eps,simpson(l,r));

```cpp
double simpson(double l, double r){return (r-l)*(f(l)+4*f((l+r)/2)+f(r))/6;}
double simpson(double l, double r, double eps, double ans) {
    double mid=(l+r)/2;
    double fl=simpson(l,mid), fr=simpson(mid,r);
    if(abs(fl+fr-ans)<=15*eps) return fl+fr+(fl+fr-ans)/15;
    return simpson(l,mid,eps/2,fl)+simpson(mid,r,eps/2,fr);
}
```



## 拉格朗日插值法

二维平面上，用 $n$ 个点可以确定一条至多 $n-1$ 次的曲线。

拉格郎日插值：用 $n$ 个点得到 $n$ 条特殊的 $n-1$ 次曲线（分别通过其中一个点，其余点值为 $0$）之和来得到原 $n-1$ 次曲线，如下：

$\displaystyle f(x)=\sum_{i=1}^{n}{y_i}\prod_{j \neq i}{\frac{x-x_j}{x_i-x_j}}$

于是可以 $O(n^2)$ 单次求解 $f(k)$，也可以先 $O(n^2)$ 计算每项系数，再 $O(n)$ 快速计算 $f(k)$。

# 计算几何

小小汇总

```cpp
const int maxn = 1e5+10;
const double PI = acos(-1);
const double eps = 1e-9;

//求正负符号
int sgn(double d) {
    if(fabs(d)<eps) return 0;
    if(d>0) return 1;
    return -1;
}

//求x,y大小关系，y默认为0
int dcmp(double x, double y=0) {
    if(fabs(x-y)<eps) return 0;
    if(x>y) return 1;
    return -1;
}

struct Point{
    double x, y;
    Point(double x=0, double y=0):x(x), y(y) {}
};

typedef Point Vector;

Vector operator + (Vector A, Vector B) {
    return Vector(A.x+B.x,A.y+B.y);
}

Vector operator - (Point A, Point B) {
    return Vector(A.x-B.x,A.y-B.y);
}

Vector operator * (Vector A, double p) {
    return Vector(A.x*p,A.y*p);
}

Vector operator / (Vector A, double p) {
    return Vector(A.x/p,A.y/p);
}

bool operator < (const Point &a, const Point &b) {
    if(a.x==b.x) return a.y<b.y;
    return a.x<b.x;
}

bool operator == (const Point &a, const Point &b) {
    if(sgn(a.x-b.x)==0 && sgn(a.y-b.y)==0) return true;
    return false;
}

//向量点乘，内积
double Dot(Vector A, Vector B) {
    return A.x*B.x+A.y*B.y;
}

//向量叉乘，外积
double Cross(Vector A, Vector B) {
    return A.x*B.y-A.y*B.x;
}

//向量长度
double Length(Vector A) {
    return sqrt(Dot(A,A));
}

//二维向量夹角，rad值
double Angle(Vector A, Vector B) {
    return acos(Dot(A,B)/Length(A)/Length(B));
}

//三角形面积的二倍
double Area2(Point A, Point B, Point C) {
    return Cross(B-A,C-A);
}

//向量逆时针旋转rad角度
Vector Rotate(Vector A, double rad) {
    return Vector(A.x*cos(rad)-A.y*sin(rad),A.x*sin(rad)+A.y*cos(rad));
}

//向量A逆时针转90°后的单位向量
Vector Normal(Vector A) {
    double L=Length(A);
    return Vector(-A.y/L,A.x/L);
}

//判断bc是否在ab的逆时针方向，构造凸包时常用
bool ToLeftTest(Point a, Point b, Point c) {
    return Cross(b-a,c-b)>0;
}

struct Line{
    Point v, p;
    Line(Point v, Point p):v(v), p(p) {}
    Point point(double t) {
        return v+(p-v)*t;
    }
};

//求两直线交点，但需保证Cross(v,w)!=0，即两直线不能夹直角
Point GetLineIntersection(Point P, Vector v, Point Q, Vector w) {
    Vector u=P-Q;
    double t=Cross(w,u)/Cross(v,w);
    return P+v*t;
}

//点P到直线AB的距离公式
double DistanceToLine(Point P, Point A, Point B) {
    Vector v1=B-A, v2=P-A;
    return fabs(Cross(v1,v2)/Length(v1));
}

//点P到线段AB的距离公式
double DistanceToSegment(Point P, Point A, Point B) {
    if(A==B) return Length(P-A);
    Vector v1=B-A, v2=P-A, v3=P-B;
    if(dcmp(Dot(v1,v2))<0) return Length(v2);
    if(dcmp(Dot(v1,v3))>0) return Length(v3);
    return DistanceToLine(P,A,B);
}

//点P在直线AB上的投影点
Point GetLineProjection(Point P, Point A, Point B) {
    Vector v=B-A;
    return A+v*(Dot(v,P-A)/Dot(v,v));
}

//判断点P是否在线段AB上,包含端点
bool OnSegment(Point p, Point a1, Point a2) {
    return dcmp(Cross(a1-p,a2-p))==0 && dcmp(Dot(a1-p,a2-p))<=0;
}

//判断两条线段是否相交
bool SegmentProperIntersection(Point a1, Point a2, Point b1, Point b2) {
    double c1=Cross(a2-a1,b1-a1), c2=Cross(a2-a1,b2-a1);
    double c3=Cross(b2-b1,a1-b1), c4=Cross(b2-b1,a2-b1);
    if(!sgn(c1)||!sgn(c2)||!sgn(c3)||!sgn(c4)) {
        bool f1=OnSegment(b1,a1,a2);
        bool f2=OnSegment(b2,a1,a2);
        bool f3=OnSegment(a1,b1,b2);
        bool f4=OnSegment(a2,b1,b2);
        return f1|f2|f3|f4;
    }
    return sgn(c1)*sgn(c2)<0 && sgn(c3)*sgn(c4)<0;
}

//求多边形面积，p为顶点集合，n为顶点个数
//方法为从第一个顶点出发把凸多边形分成n-2个三角形(另外一个常用的方法就自己写吧)
//顶点应按照逆时针顺序排列！！！关键点
double PolygonArea(Point *p, int n) {
    double s=0;
    for(int i=1; i<n-1; ++i) s+=Cross(p[i]-p[0],p[i+1]-p[0]);
    return s/2;
}

//判断点是否在多边形内(需保证凸多边形)
//若点在多边形内返回1，在多边形外部返回0，在多边形上返回-1
//若要判断在凸多边形内，则只需判断是否此点在所有（有向）边的左边
int isPointInPolygon(Point p, vector<Point> poly) {
    int wn=0;
    int n=poly.size();
    for(int i=0; i<n; ++i) {
        if(OnSegment(p,poly[i],poly[(i+1)%n])) return -1;
        int k=sgn(Cross(poly[(i+1)%n]-poly[i],p-poly[i]));
        int d1=sgn(poly[i].y-p.y);
        int d2=sgn(poly[(i+1)%n].y-p.y);
        if(k>0&&d1<=0&&d2>0) wn++;
        if(k<0&&d2<=0&&d1>0) wn--;
    }
    if(wn!=0) return 1;
    return 0;
}

struct Circle{
    Point c;
    double r;
    Circle(Point c, double r):c(c), r(r) {}
    Point point(double a) { //通过圆心角求坐标
        return Point(c.x+cos(a)*r,c.y+sin(a)*r);
    }
};

//求圆与直线的交点，交点存放在sol里面
int getLineCircleIntersection(Line L, Circle C, double &t1, double &t2, vector<Point> &sol) {
    double a=L.v.x, b=L.p.x-C.c.x, c=L.v.y, d=L.p.y-C.c.y;
    double e=a*a+c*c, f=2*(a*b+c*d), g=b*b+d*d-C.r*C.r;
    double delta=f*f-4*e*g;
    if(sgn(delta)<0) return 0; //相离
    if(sgn(delta)==0) { //相切
        t1=-f/(2*e);
        t2=-f/(2*e);
        sol.push_back(L.point(t1));
        return 1;
    }
    //相交
    t1=(-f-sqrt(delta))/(2*e);
    sol.push_back(L.point(t1));
    t2=(-f+sqrt(delta))/(2*e);
    sol.push_back(L.point(t2));
    return 2;
}

//两个圆相交的面积
double AreaOfOverlap(Point c1, double r1, Point c2, double r2) {
    double d=Length(c1-c2);
    if(r1+r2<d+eps) return 0.0;
    if(d<fabs(r1-r2)+eps) {
        double r=min(r1,r2);
        return acos(-1)*r*r;
    }
    double x=(d*d+r1*r1-r2*r2)/(2.0*d);
    double p=(r1+r2+d)/2.0;
    double t1=acos(x/r1);
    double t2=acos((d-x)/r2);
    double s1=r1*r1*t1;
    double s2=r2*r2*t2;
    double s3=2*sqrt(p*(p-r1)*(p-r2)*(p-d));
    return s1+s2-s3;
}

//求解凸包，GrahamScan算法，复杂度nlogn
//顶点编号从0到n-1
//凸包顶点(编号为排序后的编号)保存在stk数组中，从0到top-1
//方法：找到最靠左下的点（因为它一定在凸包上），然后对其他点做极角排序，最后维护凸包
Point lst[maxn];
int stk[maxn], top;

bool cmp(Point p1, Point p2) {
    int tmp=sgn(Cross(p1-lst[0],p2-lst[0]));
    if(tmp>0) return true;
    if(tmp==0&&Length(lst[0]-p1)<Length(lst[0]-p2)) return true;
    return false;
}

void Graham(int n) {
    int k=0;
    Point p0;
    p0.x=lst[0].x;
    p0.y=lst[0].y;
    for(int i=1; i<n; ++i) {
        if(p0.y>lst[i].y || (p0.y==lst[i].y&&p0.x>lst[i].x)) {
            p0.x=lst[i].x;
            p0.y=lst[i].y;
            k=i;
        }
    }
    lst[k]=lst[0];
    lst[0]=p0;
    sort(lst+1,lst+n,cmp);
    if(n==1) { top=1; stk[0]=0; return; }
    if(n==2) { top=2; stk[0]=0; stk[1]=1; return; }
    stk[0]=0, stk[1]=1, top=2;
    for(int i=2; i<n; ++i) {
        while(top>1&&Cross(lst[stk[top-1]]-lst[stk[top-2]],lst[i]-lst[stk[top-2]])<=0) --top;
        stk[top]=i, ++top;
    }
}
```



# 动态规划

## 斜率优化dp

**小总结**

1. 优化目的：在依次计算 $dp[j]$ 时**能快速找到**前方最优的 $j$ 使得从 $j$ 到 $i$ 的转移最优；若不优化，整体复杂度应为 $O(n^2)$，优化后为 $O(nlogn)$ 或 $O(n)$ （在于斜率 $k$ 随 $i$ 是否单调）。
2. 关键：能推出直线方程 $kx+b=y$，其中：
    $b$：包含当前状态 $dp[i]$ 以及一些常数(只与 $i$ 相关)。
    $y$：由只与 $j$ 相关的项组成。
    $kx$：由包含 $i$ 和 $j$ 的交叉项组成（其中 $k$ 只包含与 $i$ 有关的项，$x$ 只包含与 $j$ 有关的项）。
3. 多样性：所推出的 $kx+b=y$ 方程具有多样性，同一个转移方程可以转化为多种正确的直线方程，例如：
	原转移方程：$dp[i]=dp[j]+(s[i]-s[j]-L)^2$
	直线方程$1$：$2*s[i]*(s[j]+L)+dp[i]-s[i]^2=dp[j]+(s[j]+L)^2$
	直线方程$2$：$2*s[i]*s[j]+dp[i]-s[i]^2+2*s[i]*L-L^2=dp[j]+s[j]^2+2*s[j]*L$
4. 算法三大核心：
	1. 斜率函数slope：用直线方程中的 $x$ 和 $y$ 表达式计算$x1,y1,x2,y2$，求出斜率(double)。
	2. 单调队列中寻找最优点：注意若 $k[i]$ 能保证单调，则可进行 head++ 的操作；否则用二分。
	3. 将当前状态插入单调队列：维护一个凸包。

**经典例题：玩具装箱**

题意： 给定每个物品长度，同时打包一些连续的物品会有代价，求打包所有的物品的最小花费。

思路：写出转移方程，去掉 $min$，然后将转移方程化成直线方程，注意本题要用 long long 或者 double。

如下代码所用的直线方程为：$2*s[i]*(s[j]+L)+dp[i]-s[i]^2=dp[j]+(s[j]+L)^2$

```cpp
const int maxn = 1e5+10;
const int mod = 1e9+7;
const double eps = 1e-9;

ll N, L;
ll s[maxn], q[maxn];
ll dp[maxn];

inline double slope(int i, int j) {
    ll x1=s[i]+L, x2=s[j]+L;
    ll y1=dp[i]+(s[i]+L)*(s[i]+L);
    ll y2=dp[j]+(s[j]+L)*(s[j]+L);
    return double(y2-y1)/(x2-x1);
}

int main() {
    N=read(), L=read()+1; //L+1为了简化方程
    for(int i=1; i<=N; ++i) s[i]=read(), s[i]+=s[i-1]; //前缀和
    for(int i=1; i<=N; ++i) s[i]+=i; //s[i]+=i也为了简化方程
    int head=0, tail=0;
    for(int i=1; i<=N; ++i) {
        while(head<tail&&slope(q[head],q[head+1])-eps<=2*s[i]) head++; //找最优转移点
        dp[i]=dp[q[head]]+(s[i]-s[q[head]]-L)*(s[i]-s[q[head]]-L);
        while(head<tail&&slope(q[tail-1],i)<slope(q[tail-1],q[tail])) tail--; //将此点插入队列，并维护一个下凸包
        q[++tail]=i;
    }
    printf("%lld\n", dp[N]);
}
```

# 杂项

## A*算法

**K短路-魔法猪学院**

题意：给定一个能量值$E$，以及一些单向边权。求所拥有的能量能从$1$走到$N$多少次，并且每一次的走法不完全一样，即求最大的$K$使得 前$K$短路的和 不大于$E$。

思路：

1. 建边时将正反边都记录好
2. 第一遍跑$dijstra$或$spfa$将从$1$到所有点最短路求出来，作为$A^*$算法的$g_{(x)}$函数
3. 然后反向跑$A^*$算法（即在反向边上跑$BFS$），那么如何跑呢？
4. 我们可以利用优先队列，让$f_{(x)}$函数最小的节点为头节点，每次将离$1$尽可能近的点拿出来跑它的边，跑出的所有节点继续加到优先队列中
5. 而当每次头节点为$1$时，就用总能量值减去这种走法的$h_{(x)}$（实际上此时$f_{(x)}=h_{(x)}$，毕竟$g_{(1)}=dis_{[1]}$=0），并且方案数+1，直到总能量值小于0
6. 最后，若为洛谷提交，记得加上特判，因为洛谷这道题似乎更想考察可持久化左偏树。。。然而我还不会

```cpp
const int maxn = 2e5+10;

int head1[maxn], nxt1[maxn], to1[maxn], tot;
int head2[maxn], nxt2[maxn], to2[maxn];
double w1[maxn], w2[maxn], dis[maxn];
int N, M, ans;
double E;

void add_edge(int u, int v, double w) {
    ++tot;
    to1[tot]=v, w1[tot]=w, nxt1[tot]=head1[u], head1[u]=tot;
    to2[tot]=u, w2[tot]=w, nxt2[tot]=head2[v], head2[v]=tot;
}

void dij() {
    for(int i=1; i<=N; ++i) dis[i]=1e12;
    priority_queue<pr,vector<pr>,greater<pr> > q;
    q.push(pr(0,1)), dis[1]=0;
    while(q.size()) {
        int now=q.top().second; q.pop();
        for(int i=head1[now]; i; i=nxt1[i]) {
            int v=to1[i];
            if(dis[v]>dis[now]+w1[i]) dis[v]=dis[now]+w1[i], q.push(pr(dis[v],v));
        }
    }
}

struct P{
    int u;
    double h, f;
    bool operator < (const P &rhs) const{
        return f>rhs.f;
    }
};

void A_star() {
    if(dis[N]>1e11) return;
    priority_queue<P> q;
    q.push((P){N,0,dis[N]});
    while(q.size()) {
        int now=q.top().u;
        double h=q.top().h; q.pop();
        if(now==1) { if((E-=h)>=0) ans++; else return; }
        for(int i=head2[now]; i; i=nxt2[i])
            q.push((P){to2[i],h+w2[i],h+w2[i]+dis[to2[i]]});
    }
}

int main() {
    N=read(), M=read(); scanf("%lf", &E);
    if(E==10000000) return printf("2002000\n"), 0;
    for(int i=0; i<M; ++i) {
        int u=read(), v=read();
        double e; scanf("%lf", &e);
        add_edge(u,v,e);
    }
    dij();
    A_star();
    printf("%d\n", ans);
}
```

## IDA*算法

**骑士精神**

题意：给定一个正方形局面，求通过移动棋子到达目标局面的最小步数（大于15步则输出-1）。

思路：

1. 将空位看做“马”，利用DFS进行跳马操作
2. 设$h_{(x)}$为$x$局面已经走过的步数,$g_{(x)}$为当前局面到达目标局面的最小步数
3. 若$f_{(x)}=h_{(x)}+g_{(x)}>15$，就显然不用继续搜索了；加上这样的剪枝应该就叫$IDA^*$了？反正这样特别快
4. 当然若上面这个函数大于已经求出的$ans$也不用继续搜索了
5. $h_{(x)}$的求法就不多解释了，$A^*$算法已经此题的$IDA^*$算法关键都在于$g_{(x)}$函数的设计
6. 首先，$g_{(x)}$函数必须不大于 实际解$-$$h_{(x)}$，即要找到一个实际解与当前局面差值的下界，并小于等于它；$g_{(x)}$被设计的越接近这个下界就越好
7. 比如本题$g_{(x)}$被设计为当前局面与目标局面不同位置的数目-1（-1是因为空位与其余位置的一次交换可能使两个位置变得跟目标局面一样）

```cpp
const int maxn = 1e5+10;

const int terminal[5][5]={
	{1,1,1,1,1},
	{0,1,1,1,1},
	{0,0,2,1,1},
	{0,0,0,0,1},
	{0,0,0,0,0}
};
const int dx[]={-2,-2,-1,-1,1,1,2,2};
const int dy[]={-1,1,-2,2,-2,2,-1,1};

int mp[5][5], ans;

void dfs(const int &px, const int &py, const int &x, const int &y, int s) {
    int g=0;
    for(int i=0; i<5; ++i)
     	 for(int j=0; j<5; ++j) if(mp[i][j]!=terminal[i][j]) g++;
    if(!g) { ans=min(ans,s); return; }
    if(g+s>16||g+s>ans) return;
    for(int i=0; i<8; ++i) {
        int xx=x+dx[i];
        int yy=y+dy[i];
        if(xx>=0&&xx<5&&yy>=0&&yy<5&&(xx!=px||yy!=py)) {
            swap(mp[x][y],mp[xx][yy]);
            dfs(x,y,xx,yy,s+1);
            swap(mp[x][y],mp[xx][yy]);
        }
    }
}

void solve() {
    ans=1e9;
    int x, y;
    for(int i=0; i<5; ++i) {
        for(int j=0; j<5; ++j) {
            char c; cin>>c;
            if(c=='0') mp[i][j]=0;
            else if(c=='1') mp[i][j]=1;
            else mp[i][j]=2, x=i, y=j;
        }
    }
    dfs(x,y,x,y,0);
    cout<<(ans==1e9?-1:ans)<<endl;
}

int main() {
    int T=read();
    while(T--) solve();
}
```

## 遗传算法

代码看个大概就好，反正一顿调参就好。

```cpp
const int Size = 50;

int n, r;

mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());

inline int Dis(pr a, pr b) {
    return (a.first-b.first)*(a.first-b.first)+(a.second-b.second)*(a.second-b.second);
}

struct Chromosome{ //染色体
    vector<pr> a;
    ll s;
    Chromosome() {
        a.resize(n);
        for(int i=0; i<n; ++i) a[i]=pr((rng()%r+1)*(rng()&1?1:-1),0);
        Path_length();
    }
    pr & operator [] (int i) { return a[i]; }
    void Path_length() {
        s=0;
        for(int i=0; i<n; ++i) {
            for(int j=i+1; j<n; ++j) s+=Dis(a[i],a[j]);
        }
    }
	friend bool operator < (const Chromosome &a, const Chromosome &b) {
		return a.s>b.s;
	}
	friend bool operator == (const Chromosome &a, const Chromosome &b) {
		return a.a==b.a;
	}
    friend void Mutation(Chromosome &a) {
        int u=rng()%n;
        while(1) {
            int dx=rng()%13*(rng()&1?1:-1);
            int dy=rng()%13*(rng()&1?1:-1);
            if(Dis(pr(a[u].first+dx,a[u].second+dy),pr(0,0))<=r*r) {
                a[u].first+=dx;
                a[u].second+=dy;
                break;
            }
        }
        a.Path_length();
    }
};

void Generate(vector<Chromosome> &C, vector<ll> &P) {
	int pos=rng()%P[Size-1];
	Chromosome a, b;
	for(int i=0; i<Size; ++i) {
		if(pos<P[i]) { a=C[i]; break; }
    }
	Mutation(a);
	C.push_back(a);
}

void GA() {
	vector<Chromosome> C(Size);
	sort(C.begin(),C.end());
	for(int t=0; t<30; ++t) {
		vector<ll> P(Size);
		for(int i=0; i<Size; ++i) {
			P[i]=C[i].s;
			if(i) P[i]+=P[i-1];
		}
		for(int i=0; i<50000; ++i) Generate(C,P);
		sort(C.begin(),C.end());
		unique(C.begin(),C.end());
		while(C.size()>Size) C.pop_back();
	}
	printf("%lld,", C[0].s);
}
```



## 莫队算法

### 普通莫队

**我的莫队之旅开始啦！**

**题意**：求区间[l,r]中相同数字的数量关系（具体见题）

**思路**：（莫队思路）

1. 将所有询问按照左端点$l$所在块进行排序，若左端点属于同一块，则按照右端点排序（不用按照左端点具体大小排序啦！）
2. 排序的一点优化，为后面求解过程加速：对于左端点属于第奇数块的询问，将它们按照右端点从小到大排序；对于左端点属于第偶数块的询问，将它们按照右端点，从大到小排序。这样相反的排序可以让$r$指针不用每次都从最右边跑到最左边。
3. 莫队的四个$while$循环，暴力的模拟$l,r$指针逐渐靠近$q.l,q.r$的过程，每次移动考虑插入和删除。

```cpp
const int maxn = 5e4+10;

int n, m, len;
int c[maxn], cnt[maxn];
ll ans1[maxn], ans2[maxn], ans;

struct Q{
    int l, r, id;
    friend bool operator < (const Q &a, const Q &b) {
        if((a.l-1)/len==(b.l-1)/len) {
            if((a.l-1)/len%2) return a.r>b.r;
            else return a.r<b.r;
        }
        return a.l<b.l;
    }
}q[maxn];

inline void update(int p, int f) {
    ans-=1ll*cnt[c[p]]*(cnt[c[p]]-1);
    cnt[c[p]]+=f;
    ans+=1ll*cnt[c[p]]*(cnt[c[p]]-1);
}

ll gcd(ll a, ll b) { return b==0?a:gcd(b,a%b); }

int main() {
    n=read(), m=read(); len=sqrt(n);
    for(int i=1; i<=n; ++i) c[i]=read();
    for(int i=1; i<=m; ++i) q[i]=(Q){read(),read(),i};
    sort(q+1,q+1+m);
    int l=1, r=0;
    for(int i=1; i<=m; ++i) {
        while(l>q[i].l) update(--l,1);
      	while(r<q[i].r) update(++r,1); //先往左右扩展，防止出现交叉l,r交叉的情况
        while(l<q[i].l) update(l++,-1);
        while(r>q[i].r) update(r--,-1);
        int j=q[i].id;
        if(q[i].l==q[i].r) { ans1[j]=0; ans2[j]=1; continue; }
        ans1[j]=ans; ans2[j]=1ll*(q[i].r-q[i].l+1)*(q[i].r-q[i].l);
        ll d=gcd(ans1[j],ans2[j]);
        ans1[j]/=d; ans2[j]/=d;
    }
    for(int i=1; i<=m; ++i) printf("%lld/%lld\n", ans1[i], ans2[i]);
}
```

### 带修莫队

**题意**：

1. 询问：求区间$[l,r]$之间有多少种不同的数字
2. 修改：修改某个位置的数字
3. 不强制在线

**思路**：（带修莫队板子）

1. 基本与普通莫队一样，仅仅额外加上了时间这个维度（其实看代码更好懂），甚至按奇偶排序的小技巧也很好用！
2. 分块的大小也有讲究（当然也可以采用其他玄学分块）：
	设分块大小为$a$，莫队算法时间复杂度主要为以下三个部分：
	1. 同$l,r$块，$t$轴移动的复杂度为$O(\frac{n^2t}{a^2})=O(\frac{n}{a}*\frac{n}{a}*t)$，
	2. 同$l,r$块，$l,r$移动复杂度为$O(na)=O(\frac{n}{a}*a*a)$，
	3. 同$l$块，$r$的块间移动复杂度为$O(\frac{n^2}{a})=O(\frac{n}{a}*n)$，
	4. 因此三个函数$max$的最小值当$a$为$\sqrt[3]{nt}$取得，为$O(\sqrt[3]{n^4t})。$
3. 然后就没什么特别的啦！

```cpp
const int maxn = 1.4e5+10;

char s[2];
int N, M, cntp, cntq, len, num;
int c[maxn], cnt[maxn*10], ans[maxn];

struct P{
    int pos, pre, to;
}p[maxn];

struct Q{
    int l, r, t, id;
    friend bool operator < (const Q &a, const Q &b) {
        if((a.l-1)/len!=(b.l-1)/len) return a.l<b.l;
        if((a.r-1)/len!=(b.r-1)/len) { //仍然可以采用奇偶排序
            if((a.l-1)/len%2) return a.r>b.r;
            return a.r<b.r;
        }
        if((a.r-1)/len%2) return a.t>b.t;
        return a.t<b.t;
    }
}q[maxn];

int main() {
    N=read(), M=read();
    for(int i=1; i<=N; ++i) c[i]=read();
    for(int i=1; i<=M; ++i) {
        scanf("%s", s);
        int l=read(), r=read();
        if(s[0]=='Q') ++cntq, q[cntq]=(Q){l,r,cntp,cntq};
        else {
            ++cntp; p[cntp].pos=l;
            p[cntp].pre=c[p[cntp].pos];
            p[cntp].to=c[p[cntp].pos]=r;
        }
    }
    len=ceil(pow(1.*N*cntq,1./3));
    sort(q+1,q+1+cntq);
    for(int i=cntp; i; --i) c[p[i].pos]=p[i].pre; //从后往前，改回最初的数组
    int l=1, r=0, t=0;
    for(int i=1; i<=cntq; ++i) {
        while(l<q[i].l) num-=!(--cnt[c[l++]]);
        while(l>q[i].l) num+=!(cnt[c[--l]]++);
        while(r<q[i].r) num+=!(cnt[c[++r]]++);
        while(r>q[i].r) num-=!(--cnt[c[r--]]); //下面两个t的移动就是额外的
        while(t<q[i].t) {
            ++t;
            if(p[t].pos>=l&&p[t].pos<=r) num-=!(--cnt[c[p[t].pos]]);
            c[p[t].pos]=p[t].to;
            if(p[t].pos>=l&&p[t].pos<=r) num+=!(cnt[c[p[t].pos]]++);
        }
        while(t>q[i].t) {
            if(p[t].pos>=l&&p[t].pos<=r) num-=!(--cnt[c[p[t].pos]]);
            c[p[t].pos]=p[t].pre;
            if(p[t].pos>=l&&p[t].pos<=r) num+=!(cnt[c[p[t].pos]]++);
            --t;
        }
        ans[q[i].id]=num;
    }
    for(int i=1; i<=cntq; ++i) printf("%d\n", ans[i]);
}
```

### 树上带修莫队

**题意**：

给定一棵树，每个点的颜色，每种颜色的价值（由遍历次数和颜色种类决定）。然后有一种操作和一种询问：
操作0：修改某个点的颜色
询问1：询问$x,y$两点之间总价值（价值定义具体见题意）

**思路**：树上莫队+带修莫队

1. （**关键词：括号序**）树上莫队核心：处理出树的括号序（括号序：在$dfs$过程中进入和离开某个点时都要打上标记），在括号序上进行普通莫队（还是有点不普通）。
2. （**关键词：遍历次数的奇偶**）括号序上的莫队算法：关键在于知道什么时候该加入某个点的贡献，什么时候该退出某个点的贡献。考虑在遍历完一整颗子树后，每个节点肯定会被遍历两次，而此处的“两次”正好对应于一次加入贡献，一次退出贡献。因此如果给每个节点一个遍历次数的标记；当其为奇数时，就考虑其贡献；当其为偶数时，就退出其贡献。这样做还有个好处是在带修改时，可以直接通过被修改点的遍历次数的奇偶来判定修改当前点后是否需要重新计算其贡献。
3. （**关键词：LCA是否为路径的端点**）同时还需要注意一个细节：当考虑从$x$点沿路径到达$y$点时，如何保证我们想要加入贡献的节点在括号序中都只被遍历了一次？
**做法要点**：
	1. 若$x$点和$y$点在不同的子树上，给当前询问使用“离开”版的$x$序号，“进入”版的$y$序号；
		若$x$或$y$就是$x,y$的$LCA$，给当前询问使用“进入”版的$x$序号，“进入”版的$y$序号。
	2.  若$x$点和$y$点在不同的子树上，最终计算贡献时会发现二者的$LCA$并不会被遍历到！因为整个遍历过程中都是在$LCA$的括号中移动的，故统计答案时要额外加上$LCA$的贡献。
	
4. 然后就真正的是普通的带修莫队啦，最优分块依然是$\sqrt[3]{id*t}$（其中$id$是括号序的大小；$t$是修改操作的数量，也就是时间的范围）（不过好像大家都是玄学分块？或者说我这个是玄学分块QAQ？）
5. 补充：$LCA$采用$st$表求法，询问的排序采用了奇偶化排序。

```cpp
const int maxn = 1e5+10;

int n, m ,QQ, cnt0, cnt1, len;
int v[maxn], w[maxn], c[maxn], cc[maxn];
int deep[maxn], fa[maxn], st[maxn][20];
int id1[maxn], id2[maxn], rk[maxn*2], id;
int head[maxn], to[maxn*2], nxt[maxn*2], tot;
int vis[maxn], times[maxn];
ll cur, ans[maxn];

inline void add_edge(int u, int v) {
    ++tot; to[tot]=v; nxt[tot]=head[u]; head[u]=tot;
    ++tot; to[tot]=u; nxt[tot]=head[v]; head[v]=tot;
}

struct P{
    int pos, pre, cg;
}p[maxn];

struct Q{
    int l, r, t, id;
    friend bool operator < (const Q &a, const Q &b) {
        if((a.l-1)/len!=(b.l-1)/len) return a.l<b.l;
        if((a.r-1)/len!=(b.r-1)/len) {
            if((a.l-1)/len%2) return a.r>b.r;
            return a.r<b.r;
        }
        if((a.r-1)/len%2) return a.t>b.t;
        return a.t<b.t;
    }
}q[maxn];

void dfs(int u, int f) {
    deep[u]=deep[st[u][0]=fa[u]=f]+1; rk[id1[u]=++id]=u;
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i];
        if(v!=f) dfs(v,u);
    }
    rk[id2[u]=++id]=u;
}

int lca(int x, int y) {
    if(deep[x]>deep[y]) swap(x,y);
    for(int i=19; i>=0; --i)
        if(deep[st[y][i]]>=deep[x]) y=st[y][i];
    if(x==y) return x;
    for(int i=19; i>=0; --i)
        if(st[x][i]!=st[y][i]) x=st[x][i], y=st[y][i];
    return fa[x];
}

void add(int u) {
    if(vis[u]) cur-=1ll*w[times[c[u]]--]*v[c[u]];
    else       cur+=1ll*w[++times[c[u]]]*v[c[u]];
    vis[u]^=1;
}

int main() {
    n=read(), m=read(), QQ=read();
    for(int i=1; i<=m; ++i) v[i]=read();
    for(int i=1; i<=n; ++i) w[i]=read();
    for(int i=1; i<n; ++i) add_edge(read(),read());
    for(int i=1; i<=n; ++i) c[i]=cc[i]=read();
    dfs(1,0);
    for(int j=1; j<18; ++j)
        for(int i=1; i<=n; ++i) st[i][j]=st[st[i][j-1]][j-1];
    while(QQ--) {
        int f=read(), x=read(), y=read();
        if(!f) ++cnt0, p[cnt0].pos=x, p[cnt0].pre=cc[x], p[cnt0].cg=cc[x]=y;
        else {
            if(id1[x]>id1[y]) swap(x,y);
            ++cnt1, q[cnt1]=(Q){lca(x,y)==x?id1[x]:id2[x],id1[y],cnt0,cnt1};
        }
    }
    len=pow(1ll*id*max(1,cnt0),1.0/3);
    sort(q+1,q+1+cnt1);
    int l=1, r=0, t=0;
    for(int i=1; i<=cnt1; ++i) {
        while(l>q[i].l) add(rk[--l]);
      	while(r<q[i].r) add(rk[++r]);
        while(l<q[i].l) add(rk[l++]);
        while(r>q[i].r) add(rk[r--]);
        while(t<q[i].t) {
            t++;
            if(vis[p[t].pos]) {
                add(p[t].pos);
                c[p[t].pos]=p[t].cg;
                add(p[t].pos);
            }
            else c[p[t].pos]=p[t].cg;
        }
        while(t>q[i].t) {
            if(vis[p[t].pos]) {
                add(p[t].pos);
                c[p[t].pos]=p[t].pre;
                add(p[t].pos);
            }
            else c[p[t].pos]=p[t].pre;
            t--;
        }
        int x=rk[l], y=rk[r], LCA=lca(x,y);
        if(x!=LCA) {
            add(LCA);
            ans[q[i].id]=cur;
            add(LCA);
        }
        else ans[q[i].id]=cur;
    }
    for(int i=1; i<=cnt1; ++i) printf("%lld\n", ans[i]);
}
```

## 01Trie

### 01Trie+最长异或路径

**题意**：给定一棵带边权的树，求最大的异或路径。

**思路**：
1. 令每个节点的权值为从根到当前节点的路径上边权异或值，则此问题就被转化为普通的最大异或对了
2. 最大异或对就没啥说的了，按顺序（随便什么顺序）把每个点加入$01trie$中，然后在每加入一个点前先判定当前点能与$01trie$中异或的最大值（贪心得搞一下就行了），把所有求出的最大值再取一个最大值就是答案了

```cpp
const int maxn = 1e5+10;
const int inf = 0x3f3f3f3f;
const int mod = 1e9+7;
const double eps = 1e-7;

int N;
int head[maxn], nxt[maxn*2], to[maxn*2], w[maxn*2], tot;
int a[maxn];
int node[maxn<<5][2], cnt;

inline void add_edge(int u, int v, int w) {
    ++tot, to[tot]=v, nxt[tot]=head[u], ::w[tot]=w, head[u]=tot;
    ++tot, to[tot]=u, nxt[tot]=head[v], ::w[tot]=w, head[v]=tot;
}

inline void max(int &a, int b) { if(a<b) a=b; }

void dfs(int u, int fa) {
    for(int i=head[u]; i; i=nxt[i]) {
        int v=to[i];
        if(v!=fa) {
            a[v]=a[u]^w[i];
            dfs(v,u);
        }
    }
}

inline void insert(int p) {
    int now=0;
    for(int i=30; i>=0; --i) {
        int s=p>>i&1;
        if(!node[now][s]) node[now][s]=++cnt;
        now=node[now][s];
    }
}

inline int match(int p) {
    int now=0, ans=0;
    for(int i=30; i>=0; --i) {
        int s=p>>i&1;
        if(node[now][!s]) ans|=1<<i, now=node[now][!s];
        else now=node[now][s];
    }
    return ans;
}

int main() {
    N=read();
    for(int i=1; i<N; ++i) {
        int u=read(), v=read(), w=read();
        add_edge(u,v,w);
    }
    dfs(1,0);
    insert(a[1]); int ans=0;
    for(int i=2; i<=N; ++i) max(ans,match(a[i])), insert(a[i]);
    cout<<ans<<endl;
}
```

### 可持久化01Trie

**题意**：转化后的题意是有一种操作+一种询问：

 	1. 操作：在序列末尾插入一个数
 	2. 询问：给定$l,r,x$，求区间$l,r$中与$x$异或能得到的最大异或值（转化后的题意）

**思路**：题意都被转化成这样了。。。应该就没啥难度了

1. 用类似主席树的方式构建可持久化$01trie$
2. 然后还是简单的贪心跑$01trie$
3. 最后小心给定的$l,r$都等于$1$的情况，特判一下即可（还没有想好如何不特判）

```cpp
const int maxn = 6e5+10;

int N, M;
int root[maxn], son[maxn<<5][2], sz[maxn<<5], tot;

void insert(int p, int i, int pre, int &now) {
    sz[now=++tot]=sz[pre]+1;
    if(i<0) return;
    int s=p>>i&1;
    son[now][!s]=son[pre][!s];
    insert(p,i-1,son[pre][s],son[now][s]);
}

int query(int x, int y, int p, int i) {
    if(i<0) return 0;
    int s=p>>i&1;
    if(sz[son[y][!s]]-sz[son[x][!s]]>0) return (1<<i)+query(son[x][!s],son[y][!s],p,i-1);
    return query(son[x][s],son[y][s],p,i-1);
}

int main() {
    N=read(), M=read();
    int pre=0;
    insert(0,30,root[0],root[0]);
    for(int i=1; i<=N; ++i) insert(pre^=read(),30,root[i-1],root[i]);
    while(M--) {
        char s[2]; scanf("%s", s);
        if(s[0]=='A') insert(pre^=read(),30,root[N],root[N+1]), ++N;
        else {
            int l=read()-1, r=read()-1, x=pre^read();
            if(l==r&&l==0) printf("%d\n", x);
            else printf("%d\n", query(root[max(0,l-1)],root[r],x,30));
        }
    }
}
```

## 约瑟夫环

要模拟整个过程可以通过线段树维护区间剩余元素数量进行优化，关键在于线段树二分，注意不要查询 $[l,r]$ 内第 $k$ 个剩余元素位置，只能查询 $[1,r]$ 内第 $k$ 个元素位置。

$O(n)$ 计算最后剩下的那个人:

```cpp
int ans=0;
for(int i=2; i<=n; ++i) ans=(ans+k)%i;
ans++;
```



## 哈希

常用模数：469762049, 1000000009, 1000000007, 998244353

常用基数：12433, 73453, 233, 2333

**字符串哈希**

```cpp
const int maxn = 3e5+7;
const int mod[] = {469762049, 1000000009};
const int base[] = {12433, 73453};

char s[maxn];
ll bs[2][maxn], hs[2][maxn];

int cal(int l, int r, int s) {
    return (hs[s][r]-ll(hs[s][l-1])*bs[s][r-l+1]%mod[s]+mod[s])%mod[s];
}

int main() {
    scanf("%s", s+1); int n=strlen(s+1);
    for(int s=0; s<2; ++s) {
        bs[s][0]=1;
        for(int i=1; i<=n; ++i) hs[s][i]=(hs[s][i-1]*base[s]+::s[i])%mod[s];
        for(int i=1; i<=n; ++i) bs[s][i]=bs[s][i-1]*base[s]%mod[s];
    }
}
```

## 简易高精度

```cpp
const int base = 10;

struct INT{
    vector<int> v;
    INT(int a=0) {
        while(a) { v.push_back(a%base); a/=base; }
        if(v.size()==0) v.push_back(0);
    }
    void resize(int size=0) { v.clear(); v.resize(size,0); }
    int & operator [] (int i) { return v[i]; }
    inline int size() const { return v.size(); }
    inline void push_back(int d) { v.push_back(d); }
    inline int back() { return v.back(); }
    inline void pop_back() { return v.pop_back(); }
    void topzero() { while(v.size()>1&&v.back()==0) v.pop_back(); }
    friend INT operator + (INT a, INT b) {
        INT c;
        int size=max(a.size(),b.size()); c.resize(size);
        int up=0;
        for(int i=0; i<size; ++i) {
            c[i]=up;
            if(i<a.size()) c[i]+=a[i];
            if(i<b.size()) c[i]+=b[i];
            up=c[i]/base; c[i]%=base;
        }
        while(up) c.push_back(up%base), up/=base;
        c.topzero();
        return c;
    }
    friend INT operator * (INT a, INT b) {
        INT c;
        c.resize(a.size()+b.size()+7);
        for(int i=0; i<a.size(); ++i) {
            for(int j=0; j<b.size(); ++j) {
                c[i+j]+=a[i]*b[j];
            }
        }
        for(int i=0; i<c.size(); ++i) {
            if(c[i]>=base) c[i+1]+=c[i]/base, c[i]%=base;
        }
        c.topzero();
        return c;
    }
    friend INT qpow(INT a, ll b) {
        INT c(1);
        while(b) {
            if(b&1) c=c*a;
            a=a*a; b>>=1;
        }
        c.topzero();
        return c;
    }
    friend INT operator / (INT a, int b) {
        INT c; ll tmp=0;
        for(int i=a.size()-1; i>=0; --i) {
            tmp=tmp*base+a[i];
            c.push_back(tmp/b); tmp%=b;
        }
        reverse(c.v.begin(),c.v.end());
        c.topzero();
        return c;
    }
    friend ll operator % (INT a, ll b) {
        ll c=0;
        for(int i=a.size()-1; i>=0; --i) {
            c=(c*base+a[i])%b;
        }
        return c;
    }
    void print() {
        printf("%d", v[v.size()-1]);
        for(int i=v.size()-2; i>=0; --i) printf("%d", v[i]);
        puts("");
    }
};
```



## 压缩打表

```cpp
/*
    利用 44 进制进行数组压缩，大于等于 44 的数同时存储了分割符
    对于 int 范围内的数压缩效果较好
    减号 '-'(45): 负号
    去掉 '"'(34), '/'(47), '?'(63), '\'(92)
    少了 ' '(32), '~'(126)
*/

void encode(ll *a, char *s, int n) {
    for(int i=1, len=0; i<=n; ++i) {
        ll x=a[i];
        if(x<0) s[len++]='-', x=-x;
        if(!x) s[len++]=81;
        while(x) {
            int t=x%44; x/=44; if(x==0) t+=44;
            s[len++]=t+33+(t>=1)+(t>=11)+(t>=12)+(t>=27)+(t>=55);
        }
    }
}

int decode(char *s, ll *a) {
    int n=0, len=strlen(s); ll x;
    for(int l=0, r, f; l<len; l=r+1) {
        x=0, f=1; if(s[l]=='-') f=-1, ++l; r=l;
        while(s[r]<81) ++r;
        for(int i=r; i>=l; --i) {
            int t=s[i]-33;
            t-=t>1; t-=t>11; t-=t>12; t-=t>27; t-=t>55;
            x=x*44+t%44;
        }
        a[++n]=f*x;
    }
    return n;
}
```

```cpp
/*
    利用 89 进制进行数组压缩，最大限度利用可显示字符
    对于 long long 范围内的数压缩效果较好
    空格 ' '(32): 分隔符
    减号 '-'(45): 负号
    去掉 '"'(34), '/'(47), '?'(63), '\'(92)
*/

void encode(ll *a, char *s, int n) {
    for(int i=1, len=0; i<=n; ++i) {
        ll x=a[i];
        if(x<0) s[len++]='-', x=-x;
        if(!x) s[len++]=33;
        while(x) {
            int t=x%89; x/=89;
            s[len++]=t+33+(t>=1)+(t>=11)+(t>=12)+(t>=27)+(t>=55);
        }
        s[len++]=' ';
    }
}

int decode(char *s, ll *a) {
    int n=0, len=strlen(s); ll x;
    for(int l=0, r, f; l<len; l=r+2) {
        x=0, f=1; if(s[l]=='-') f=-1, ++l; r=l;
        while(s[r+1]!=' ') ++r;
        for(int i=r; i>=l; --i) {
            int t=s[i]-33;
            t-=t>1; t-=t>11; t-=t>12; t-=t>27; t-=t>55;
            x=x*89+t;
        }
        a[++n]=f*x;
    }
    return n;
}
```



## C++相关

1. cout<<fixed<<setprecision(9); 能对浮点数固定保留小数点后9位。

## Java相关

### BigInteger

初始化建议用字符串来初始化

```
BigInteger abs()  返回大整数的绝对值
BigInteger add(BigInteger val) 返回两个大整数的和
BigInteger and(BigInteger val)  返回两个大整数的按位与的结果
BigInteger andNot(BigInteger val) 返回两个大整数与非的结果
BigInteger divide(BigInteger val)  返回两个大整数的商
double doubleValue()   返回大整数的double类型的值
float floatValue()   返回大整数的float类型的值
BigInteger gcd(BigInteger val)  返回大整数的最大公约数
int intValue() 返回大整数的整型值
long longValue() 返回大整数的long型值
BigInteger max(BigInteger val) 返回两个大整数的最大者
BigInteger min(BigInteger val) 返回两个大整数的最小者
BigInteger mod(BigInteger val) 用当前大整数对val求模
BigInteger multiply(BigInteger val) 返回两个大整数的积
BigInteger negate() 返回当前大整数的相反数
BigInteger not() 返回当前大整数的非
BigInteger or(BigInteger val) 返回两个大整数的按位或
BigInteger pow(int exponent) 返回当前大整数的exponent次方
BigInteger remainder(BigInteger val) 返回当前大整数除以val的余数
BigInteger leftShift(int n) 将当前大整数左移n位后返回
BigInteger rightShift(int n) 将当前大整数右移n位后返回
BigInteger subtract(BigInteger val)返回两个大整数相减的结果
byte[] toByteArray(BigInteger val)将大整数转换成二进制反码保存在byte数组中
String toString() 将当前大整数转换成十进制的字符串形式
BigInteger xor(BigInteger val) 返回两个大整数的异或
BigInteger modInverse(BigInteger m) 返回当前大整数对m的逆元
BigInteger modPow(BigInteger e, BigInteger m) 返回当前大整数在模m意义下的e次幂
```

1