KMP算法就是改进了不匹配时候返回去从哪个位置开始再进行比较。比如当第一个主串的c与子串中的a不匹配时，下一次的 主串的b和子串的a(第一个)的比较可以通过分析子串的特点直接跳过这次比较。KMP算法就是为了告诉我们，我们应该每当一趟匹配过程中出现比较不等时，我们不需要回溯i指针。而是利用已经得到的“部分匹配”的结果将模式子串想右“滑动”尽可能远的距离，然后继续比较。首先先要统计一下要匹配的字符串中的重复情况，也就是求一个next[]数组。 

```
#include <vector>
#include <string>
#include <iostream>
using namespace std;
const vector<int> * kmp_next(string &m)
{
    static vector<int> next(m.size());
    next[0]=0;
    int temp;
    for(int i=1; i<next.size(); i++)
    {
        temp=next[i-1];
        while(m[i]!=m[temp]&&temp>0)
        {
            temp=next[temp-1];
        }
        if(m[i]==m[temp])
            next[i]=temp+1;
        else next[i]=0;
    }
    return &next;
}

bool kmp_search(string text,string m,int &pos)
{
    const vector<int> * next=kmp_next(m);
    int tp=0;
    int mp=0;
    for(tp=0; tp<text.size(); tp++)
    {
        while(text[tp]!=m[mp]&&mp)
            mp=(*next)[mp-1];
        if(text[tp]==m[mp])
            mp++;
        if(mp==m.size())
        {
            pos=tp-mp+1;
            return true;
        }
    }
    if(tp==text.size())
        return false;
}
int main()
{
    int pos=0;
    kmp_search("abcacbc","ca",pos);
    cout<<"position = "<<pos+1<<endl;
    return 0;
}
```