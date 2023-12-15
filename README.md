# Segment-Tree
class SGTree
{
    vector<int> seg;

public:
    SGTree(int n)
    {
        seg.resize(4 * n + 1);
    }

    void build(int ind, int low, int high, int arr[])
    {
        if (low == high)
        {
            seg[ind] = arr[low];
            return;
        }

        int mid = (low + high) / 2;
        build(2 * ind + 1, low, mid, arr);
        build(2 * ind + 2, mid + 1, high, arr);
        seg[ind] = min(seg[2 * ind + 1], seg[2 * ind + 2]);
    }

    int query(int ind, int low, int high, int l, int r)
    {
        // no overlap
        // l r low high or low high l r
        if (r < low || high < l)
            return INT_MAX;

        // complete overlap
        // [l low high r]
        if (low >= l && high <= r)
            return seg[ind];

        int mid = (low + high) >> 1;
        int left = query(2 * ind + 1, low, mid, l, r);
        int right = query(2 * ind + 2, mid + 1, high, l, r);
        return min(left, right);
    }
    void update(int ind, int low, int high, int i, int val)
    {
        if (low == high)
        {
            seg[ind] = val;
            return;
        }

        int mid = (low + high) >> 1;
        if (i <= mid)
            update(2 * ind + 1, low, mid, i, val);
        else
            update(2 * ind + 2, mid + 1, high, i, val);
        seg[ind] = min(seg[2 * ind + 1], seg[2 * ind + 2]);
    }
};






#Segment Tree With Lazy Propagation

class SGTree
{
    ll NO_OPERATION = inf;
    vector<ll> seg;

public:
    SGTree(ll n)
    {
        seg.resize(4 * n + 1);
    }

    ll operate(ll a, ll b)
    {
        if (b == NO_OPERATION)
            return a;
        return b;
    }

    void apply_operate(ll &a, ll b)
    {
        a = operate(a, b);
    }

    void build(ll ind, ll low, ll high, vector<ll> &arr)
    {
        if (low == high)
        {
            seg[ind] = arr[low];
            return;
        }

        ll mid = (low + high) / 2;
        build(2 * ind + 1, low, mid, arr);
        build(2 * ind + 2, mid + 1, high, arr);
        seg[ind] = min(seg[2 * ind + 1], seg[2 * ind + 2]);
    }

    void propagate(ll ind, ll l, ll r)
    {
        if (l == r)
        {
            return;
        }
        apply_operate(seg[2 * ind + 1], seg[ind]);
        apply_operate(seg[2 * ind + 2], seg[ind]);
        seg[ind] = NO_OPERATION;
    }

    ll query(ll ind, ll low, ll high, ll l, ll r)
    {
        // no overlap
        // l r low high or low high l r
        if (r < low || high < l)
            return 0;

        // complete overlap
        // [l low high r]
        if (low >= l && high <= r)
            return seg[ind];

        ll mid = (low + high) >> 1;
        ll left = query(2 * ind + 1, low, mid, l, r);
        ll right = query(2 * ind + 2, mid + 1, high, l, r);
        return min(left, right);
    }

    ll value_at_index(ll ind, ll low, ll high, ll pos)
    {
        propagate(ind, low, high);

        if (low == high)
        {
            return seg[ind];
        }

        ll mid = (low + high) / 2;

        ll value = 0;

        if (pos <= mid)
        {
            value = value_at_index(2 * ind + 1, low, mid, pos);
        }
        else
        {
            value = value_at_index(2 * ind + 2, mid + 1, high, pos);
        }

        return value;
    }

    void operation(ll ind, ll low, ll high, ll l, ll r, ll val)
    {
        propagate(ind, low, high);

        // no overlap
        // l r low high or low high l r
        if (r < low || high < l)
            return;

        // complete overlap
        // [l low high r]
        if (low >= l && high <= r)
        {
            apply_operate(seg[ind], val);
            return;
        }

        ll mid = (low + high) >> 1;
        operation(2 * ind + 1, low, mid, l, r, val);
        operation(2 * ind + 2, mid + 1, high, l, r, val);
    }

    void update(ll ind, ll low, ll high, ll i, ll val)
    {
        if (low == high)
        {
            seg[ind] += val;
            return;
        }

        ll mid = (low + high) >> 1;
        if (i <= mid)
            update(2 * ind + 1, low, mid, i, val);
        else
            update(2 * ind + 2, mid + 1, high, i, val);
        seg[ind] = max(seg[2 * ind + 1], seg[2 * ind + 2]);
    }
};
