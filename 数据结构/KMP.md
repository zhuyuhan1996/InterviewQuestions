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

