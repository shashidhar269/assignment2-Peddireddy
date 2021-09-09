# assignment2-Peddireddy
# Shashidhar Reddy Peddireddy
##### Paris
I love **Paris** because it has **Eiffel Tower** and we can enjoy the aerial views of the city. 

## Travel Guide

***

1. Get a cab in Maryville to Kansas City International Airport(MCI).
    1. You will be charged around $50.
2. Take a Flight from Kansas City International Airport(MCI) to Paris Charles de Gaulle Airport(CDG),France.
    1. Ticket Price for One-way Trip - $ 500.
    2. Ticket price for Round Trip - $ 1000.
3. From Paris Charles de Gaulle Airport(CDG) take a cab or rental car to Eiffel tower.
    1. You will get Rental Cars for best prices.
        1. Charge for Rental Car will be around $100 per day.
    * Food/Drinks for Enjoyment
        * Food Items
            * Butter Chicken with roti's
            * Loose Prawns
            * Chicken 65
            * Chicken Biryani 
            * Ice Cream with Gulab Jam
        * Drinks 
            * Beer's 
            * Whisky
            * Vodka
            * Mocktail's
            * Tequila
            * Thickshake 

[AboutMe](AboutMe.md)

The below table describes the types of food/drinks that everyone should try.<br>
It describes the food/drink item name, location and amount of money.
 
### FOOD & Drink Table
 
---
 
| Food/Drink Item | Location | Amount |
|   ----------    |  -----   |   ---- | 
| loose prawns | Hyderabad | $10 |
| Bourbon Whisky | Maryville | $74 |
| Chicken Biryani | Hanamkonda | $20 |
| afghani Mandi special | Warangal | $12 |

---

### Quotes

> If you are good at something, never do it for free - *JOKER* 

> Live like there's no tomorrow - *Jerry Spinelli*

---

### Suffix tree

> Suffix tree is a compressed trie of all the suffixes of a given string. Suffix trees help in solving a lot of string related problems like pattern matching, finding distinct substrings in a given string, finding longest palindrome etc...

suffix tree Content <https://www.hackerearth.com/practice/data-structures/advanced-data-structures/suffix-trees/tutorial/>

```

string s;
int n;

struct node {
    int l, r, par, link;
    map<char,int> next;

    node (int l=0, int r=0, int par=-1)
        : l(l), r(r), par(par), link(-1) {}
    int len()  {  return r - l;  }
    int &get (char c) {
        if (!next.count(c))  next[c] = -1;
        return next[c];
    }
};
node t[MAXN];
int sz;

struct state {
    int v, pos;
    state (int v, int pos) : v(v), pos(pos)  {}
};
state ptr (0, 0);

state go (state st, int l, int r) {
    while (l < r)
        if (st.pos == t[st.v].len()) {
            st = state (t[st.v].get( s[l] ), 0);
            if (st.v == -1)  return st;
        }
        else {
            if (s[ t[st.v].l + st.pos ] != s[l])
                return state (-1, -1);
            if (r-l < t[st.v].len() - st.pos)
                return state (st.v, st.pos + r-l);
            l += t[st.v].len() - st.pos;
            st.pos = t[st.v].len();
        }
    return st;
}

int split (state st) {
    if (st.pos == t[st.v].len())
        return st.v;
    if (st.pos == 0)
        return t[st.v].par;
    node v = t[st.v];
    int id = sz++;
    t[id] = node (v.l, v.l+st.pos, v.par);
    t[v.par].get( s[v.l] ) = id;
    t[id].get( s[v.l+st.pos] ) = st.v;
    t[st.v].par = id;
    t[st.v].l += st.pos;
    return id;
}

int get_link (int v) {
    if (t[v].link != -1)  return t[v].link;
    if (t[v].par == -1)  return 0;
    int to = get_link (t[v].par);
    return t[v].link = split (go (state(to,t[to].len()), t[v].l + (t[v].par==0), t[v].r));
}

void tree_extend (int pos) {
    for(;;) {
        state nptr = go (ptr, pos, pos+1);
        if (nptr.v != -1) {
            ptr = nptr;
            return;
        }

        int mid = split (ptr);
        int leaf = sz++;
        t[leaf] = node (pos, n, mid);
        t[mid].get( s[pos] ) = leaf;

        ptr.v = get_link (mid);
        ptr.pos = t[ptr.v].len();
        if (!mid)  break;
    }
}

void build_tree() {
    sz = 1;
    for (int i=0; i<n; ++i)
        tree_extend (i);
}

```

Suffix tree code link <https://cp-algorithms.com/string/suffix-tree-ukkonen.html>

