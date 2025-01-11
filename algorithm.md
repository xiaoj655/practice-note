## 快速排序
*nlog(n)*
```cpp
void quick_sort(int arr[], int l, int r)
{
    if (l>=r) return;

    int mid = l+r >> 1;
    int v = arr[mid], i=l-1, j=r+1;

    while(i<j)
    {
        do(i++);while(arr[i]<v);
        do(j--);while(arr[j]>v);

        if(i<j) swap(arr[i], arr[j]);
    }

    quick_sort(arr, l, j), quick_sort(arr, j+1, r);
}
```
边界要对称, 这里的边界是*mid*与下轮递归传参*l,r*的关系

mid = l+r >> 1, 这样是左取整, 奇数的话就会下取整 例如 3 -> 1

mid = l+r+1>>1, 这样是上取整

如果*mid*以右取整计算, 哪么递归传参以左边界*l*为基础, 反之亦然


## 归并排序
每次2分排序, 尽头为2个数字, 之后回溯再排

时间复杂度 *nlog(n)*
```cpp
void merge_sort(int arr[], int l, int r)
{
    if (l>=r) return;

    int mid = l+r >> 1;
    merge_sort(arr, l, mid), merge_sort(arr, mid+1, r);

    int i=l, j=mid+1, k=0;

    while(i<=mid && j<= r)
    {
        if(arr[i]<arr[j]) tmp[k++] = arr[i++];
        else tmp[k++] = arr[j++];
    }
    while(i<=mid) tmp[k++] = arr[i++];
    while(j<=r) tmp[k++] = arr[j++];

    for(i=l, j=0;j<k;j++,i++) arr[i] = tmp[k];
}
```
与快排相比, 有额外的空间复杂度, 不过无伤大雅


## 二分查找
*log(n)*

```cpp
bool check(){...};

// [l,r] -> [l, mid], [mid+1, r]
int binary_search_1(int arr[], int l, int r)
{
    while(l<r)
    {
        int mid = l+r >> 1; // down
        if(check(...)) r = mid;
        else l = mid+1;
    }
    return l;
}

int binary_search_2(int arr[], int l, int r) 
{
    while(l<r)
    {
        int mid = l+r+1 >> 1; // up
        if(check(...)) l = mid;
        else r = mid-1;
    }
    return l;
}
```

## 浮点数二分
相比于整数二分, 无需考虑因边界取值导致的执行出错
```cpp
int binary_search_3(double l, double r)
{
    const double eps = 1e-6;
    while(l+eps < r)
    {
        double mid = (l+r) / 2;
        if(check(...)) r = mid;
        else l = mid;
    }
    return l;
}
```


## 大数运算
__KEY:__ 模拟四则运算, 以一个变量存储中间进位(加法), 借一(减法) 的中间值

大的vector通过引用传递, 可以优化效率
```cpp
vector<int> add(vector<int> A, vector<int> B)
{
    vector<int> C;
    int t=0;

    for(int i=0; i<A.size() || i<B.size(); i++)
    {
        if(i<A.size()) t+=A[i];
        if(i<B.size()) t+=B[i];

        C.push_back(t%10);
        t /= 10;
    }
    return C;
}

vector<int> sub(vector<int>& A, vector<int>& B)
{
    vector<int> C;
    int t=0;

    for(int i=0; i<A.size(); i++)
    {
        t = A[i] - t;
        if(i<B.size()) t -= B[i];

        C.push_back((t+10)%10);
        if(t<0) t = 1;
        else t = 0;
    }
    while(C.back() == 0 && C.size() > 1) C.pop_back();
    return C;
}

// 大整数A, 小整数b
vector<int> mul(vector<int> A, int b)
{
    vector<int> C;

    int t = 0

    for(int i=0; i<A.size() || t; i++)
    {
        if(i < A.size()) A+=b*A[i];
        C.push_back(t%10);
        t /= 10;
    }

    while(C.back() == 0 && C.size() > 1) C.pop_back();

    return C;
}

// 大整数A, 小整数b
vector<int> div(vector<int> A,int b, int& r)
{
    vector<int> C;
    r = 0;
    for(int i=A.size(); i>=0 ; i--)
    {
        r = r*10 + A[i];
        C.push_back(r/b);
        r %= b;
    }
    reverse(A.begin(), A.end());
    while(A.back() == 0 && A.size() > 1) A.pop_back();

    return C;
}

```

