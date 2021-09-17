# 1/ MAGICSEQ
# Solution
- Ban đầu ta sẽ khởi tạo mảng a[] với n phần tử sắp xếp tăng dần: ```for(int i = 1; i <= n; i++) a[i] = i``` (Điều này tương đương với chuỗi n - 1 kí tự '<')
- Ta nhận thấy với mỗi chuỗi con của s có dạng ">>...>" ta sẽ xoay ngược dãy con tương ứng trong mảng n phần tử, ví dụ như sau:<br/>
__Ban đầu__:<br/>
string s = "<>>>";<br/>
int a[] = {1, 2, 3, 4, 5};<br/>
__Thực hiện:__<br/>
int a[] = {1, 5, 4, 3, 2};<br/>
# Code
```
#include <bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int maxn = 2e5 + 7;

int n,a[maxn];
string s;
stack <int> t;

void program(){
    cin >> s;
    n = s.size() + 1;
    s = '-' + s;
    // khởi tạo ban đầu
    for(int i = 1; i <= n; i++) a[i] = i;
    // thực hiện
    for(int i = 1; i < n; i++){
        t.push(a[i]); // đầu tiên cho phần tử đang xét vào stack
        
        if(s[i]=='<'){ // nếu '<' thì ta sẽ xoay phần tử hiện tại cùng với các phần tử trước đó (nếu có)
            while(t.size()){
                // vì là stack nên sẽ cout ra phần tử push vào cuối cùng đến phtu push vào sau cùng
                cout << t.top() << " ";
                t.pop();
            }
        }
    }
    t.push(a[n]);
    // xoay phần tử hiện tại cùng với các phần tử trước đó (nếu có)
    while(t.size()){
        cout << t.top() << " ";
        t.pop();
    }
    
}
signed main(){
    //freopen("MAGICSEQ.INP","r",stdin);
    //freopen("MAGICSEQ.OUT","w",stdout);
    ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
    program();
}
```
# 2/ GHEPSO
# Solution
- Vì số cuối cùng nhận được có n + m chữ số nên ta sẽ thực hiện n + m lần **thao tác**
- **Thao tác:**
+ _Tìm số lớn nhất_: Ta sẽ gọi i là chỉ số đang xét ở string thứ nhất (string a), j là chỉ số đang xét ở string thứ hai (string b).  **(Đánh dấu 1)**  Nếu ```a[i]>b[j]``` thì ta sẽ ```cout``` ra a[i], còn nếu ```a[i]<b[j]``` thì ta sẽ ```cout``` ra b[j]. Nhưng nếu ```a[i]==b[j]``` thì sao? Ta sẽ làm nnao? Ta tịnh tiến i và j cho đến khi a[i] khác b[j], và làm tương tự như bước trên thôi. Giả sử như khi tịnh tiến hoặc i hoặc j ra ngoài phạm vi (tức là i>n hoặc j>m) ta sẽ ```cout``` ra thằng còn lại, còn nếu như cả i và j đều ngoài phạm vi thì cout ra thằng nào cx đc. Quay trở lại mốc **(Đánh dấu 1)**, điều này sẽ chỉ thực hiện được khi lúc này i và j chưa ra ngoài phạm vi. Nếu một trong i và j ra ngoài phạm vi thì ta cx sẽ ```cout``` ra thằng còn lại như trên thôi, nma có điểm đặc biệt là không có trường hợp cả i cả j đều ra ngoài phạm vi, vì vốn dĩ trong 1 **thao tác** hoặc i hoặc j sẽ tăng thêm 1 đơn vị, có n + m thao tác tương ứng với số cuối cùng nhận được có n + m chữ số
+ _Tìm số bé nhất__ \: Tương tự trên thôi, chỉ khác là ta sẽ làm riêng **thao tác** đầu tiên, do sẽ có trường hợp số 0 ở đầu
# Code
```
#include <bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

int n,m;
string a,b;

void program(){
    cin >> a >> b;
    n = a.size();
    m = b.size();
    a = '-' + a;
    b = '+' + b;
    //be
    
    // trường hợp làm riêng
    int i = 1, j = 1;
    if(a[i]!='0' && b[j]!='0'){
        if(a[i]<b[j]) cout << a[i++];
        else cout << b[j++];
    }
    else{
        if(a[i]=='0') cout << b[j++];
        else cout << a[i++];
    }
    //
    
    for(int ik = 2; ik <= n + m; ik++){
        if(i>n) cout << b[j++];
        else if(j>m) cout << a[i++];
        else{
            if(a[i]<b[j]) cout << a[i++];
            else if(a[i]>b[j]) cout << b[j++];
            else{
                int l = i, r = j;
                while(a[l]==b[r] && l<=n && r<=m){
                    l++;
                    r++;
                }
                if(l>n && r>m) cout << a[i++];
                else if(l>n) cout << b[j++];
                else if(r>m) cout << a[i++];
                else{
                    if(a[l]<b[r]) cout << a[i++];
                    else cout << b[j++];
                }
            }
        }
    }
    cout << endl;
    //lon;
    i = 1, j = 1;
    for(int ik = 1; ik <= n + m; ik++){
        if(i>n) cout << b[j++];
        else if(j>m) cout << a[i++];
        else{
            if(a[i]>b[j]) cout << a[i++];
            else if(a[i]<b[j]) cout << b[j++];
            else{
                int l = i, r = j;
                while(a[l]==b[r] && l<=n && r<=m){
                    l++;
                    r++;
                }
                if(l>n && r>m) cout << a[i++];
                else if(l>n) cout << b[j++];
                else if(r>m) cout << a[i++];
                else{
                    if(a[l]>b[r]) cout << a[i++];
                    else cout << b[j++];
                }
            }
        }
    }
    
}
signed main(){
	freopen("ghepso.INP","r",stdin);
	freopen("ghepso.OUT","w",stdout);
    ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
    program();
}
```
