# Assignment02-Arrabelli
# Sumanth Arrabelli 
###### Favourite place-Miami 
Miami, officially the City of Miami, is a **coastal metropolis** located in Miami-Dade County in **southeastern Florida**, United States. With a population of **442,241 as of the 2020 census**, it is the 44th-largest city in the United States and the core of the nation's **eighth-largest metropolitan area**.

---

# Directions from Maryville to Newjersey 
    1.Directions from Maryville to NewJersey.
        1. start in Maryville (Missouri) drive for about 4 hours
        2. St.Joseph to Springfield
        3. Springfield to  Harristown.
        4. Harristown to Oakwood.
        5. Oakwood to Washington.
        6. Washington to Denver.
    2. Finally NewJersey.

## Products to be packed for enjoyment.
    - Comfortable Shoes.
    - Weather Appropriate Clothes.
    - Jackets & Hoodies.
    - Purse/Backpack.
    - Professional Camera.
    - Extra Batteries/Charger.
    - Water Bottles.
    - Extra Cash.
    
---

[Aboutme](AboutMe.md)

---
	## Beverage Section
This is the beverage, location and price table

| Food		    |  Location | Price    |
| ------------- | --------- | -------- |
| Egg McMuffin	| McDonalds	| $1.50    |
| Chicken tacos | Taco john's| $3.70   |
| Chickenranch  | Subway	| $5.20    |
| Cheeseburger	| Dominos	| $2.60    |

---
# Quotes

>Be who you are and say what you feel, because those who mind don’t matter and those who matter don’t mind.

>We must not allow other people’s limited perceptions to define us.

>Do what you can, with what you have, where you are.

---

# Geometry Convex hull, Sweepline, Misc

In geometry, the convex hull or convex envelope or convex closure of a shape is the smallest convex set that contains it. The convex hull may be defined either as the intersection of all convex sets containing a given subset of a Euclidean space, or equivalently as the set of all convex combinations of points in the subset.
[More About Geometry](https://en.wikipedia.org/wiki/Convex_hull)

struct pt {  

  double x, y;    

};  



bool cmp(pt a, pt b) {    
    return a.x < b.x ||(a.x == b.x && a.y < b.y);  
}  

bool cw(pt a, pt b, pt c) {  
    return a.x*(b.y-c.y)+b.x*(c.y-a.y)+c.x*(a.y-b.y) < 0;  
}  

bool ccw(pt a, pt b, pt c) {  
    return a.x*(b.y-c.y)+b.x*(c.y-a.y)+c.x*(a.y-b.y) > 0;  
}  

void convex_hull(vector<pt>& a) {  
    if (a.size() == 1)  
        return;  
      sort(a.begin(), a.end(), &cmp);  
      pt p1 = a[0], p2 = a.back();  
      vector<pt> up, down;  
      up.push_back(p1);  
      down.push_back(p1);  
      for (int i = 1; i < (int)a.size(); i++) {  
          if (i == a.size() - 1 || cw(p1, a[i], p2)) {  
              while (up.size() >= 2 && !cw(up[up.size()-2], up[up.size()-1], a[i]))  
                  up.pop_back();  
              up.push_back(a[i]);  
          }  
          if (i == a.size() - 1 || ccw(p1, a[i], p2)) {  
              while(down.size() >= 2 && !ccw(down[down.size()-2], down[down.size()-1], a[i]))  
                  down.pop_back();  
              down.push_back(a[i]);  
          }  
      }  
      a.clear();  
      for (int i = 0; i < (int)up.size(); i++)  
      a.push_back(up[i]);  
      for (int i = down.size() - 2; i > 0; i--)  
      a.push_back(down[i]);  
      }
[Geometry Covex Hull Source Code](https://cp-algorithms.com/geometry/grahams-scan-convex-hull.html)

A sweep line is an imaginary vertical line which is swept across the plane rightwards. That's why, the algorithms based on this concept are sometimes also called plane sweep algorithms. We sweep the line based on some events, in order to discretize the sweep.
[More About Sweepline](https://www.hackerearth.com/practice/math/geometry/line-sweep-technique/tutorial/#:~:text=A%20sweep%20line%20is%20an,order%20to%20discretize%20the%20sweep.)

const double EPS = 1E-9;  
struct pt {  
    double x, y;  
};  
struct seg {  
    pt p, q;  
    int id;  
    double get_y(double x) const {   
          if (abs(p.x - q.x) < EPS)  
              return p.y;  
          return p.y + (q.y - p.y) * (x - p.x) / (q.x - p.x);  
      }  
};  
bool intersect1d(double l1, double r1, double l2, double r2) {  
    if (l1 > r1)  
        swap(l1, r1);  
    if (l2 > r2)  
        swap(l2, r2);  
    return max(l1, l2) <= min(r1, r2) + EPS;  
}  

int vec(const pt& a, const pt& b, const pt& c) {  
    double s = (b.x - a.x) * (c.y - a.y) - (b.y - a.y) * (c.x - a.x);  
    return abs(s) < EPS ? 0 : s > 0 ? +1 : -1;  
}  

bool intersect(const seg& a, const seg& b)  
{  
    return intersect1d(a.p.x, a.q.x, b.p.x, b.q.x) &&  
           intersect1d(a.p.y, a.q.y, b.p.y, b.q.y) &&  
           vec(a.p, a.q, b.p) * vec(a.p, a.q, b.q) <= 0 &&  
           vec(b.p, b.q, a.p) * vec(b.p, b.q, a.q) <= 0;  
}  
bool operator<(const seg& a, const seg& b)  
{  
    double x = max(min(a.p.x, a.q.x), min(b.p.x, b.q.x));  
    return a.get_y(x) < b.get_y(x) - EPS;  
}  
 struct event {  
    double x;  
    int tp, id;  
     event() {}  
      event(double x, int tp, int id) : x(x), tp(tp), id(id) {}  
    bool operator<(const event& e) const {  
          if (abs(x - e.x) > EPS)  
              return x < e.x;  
          return tp > e.tp;  
      }  
};  
set<seg> s;  
vector<set<seg>::iterator> where;  
set<seg>::iterator prev(set<seg>::iterator it) {  
    return it == s.begin() ? s.end() : --it;  
}  
set<seg>::iterator next(set<seg>::iterator it) {  
    return ++it;  
}  
pair<int, int> solve(const vector<seg>& a) {  
    int n = (int)a.size();  
    vector<event> e;  
    for (int i = 0; i < n; ++i) {  
        e.push_back(event(min(a[i].p.x, a[i].q.x), +1, i));  
        e.push_back(event(max(a[i].p.x, a[i].q.x), -1, i));  
    }  
    sort(e.begin(), e.end());  
        s.clear();  
        where.resize(a.size());  
        for (size_t i = 0; i < e.size(); ++i) {  
         int id = e[i].id;  
            if (e[i].tp == +1) {  
                set<seg>::iterator nxt = s.lower_bound(a[id]), prv = prev(nxt);  
                if (nxt != s.end() && intersect(*nxt, a[id]))  
                    return make_pair(nxt->id, id);  
                if (prv != s.end() && intersect(*prv, a[id]))  
                return make_pair(prv->id, id);  
                where[id] = s.insert(nxt, a[id]);  
            } else {  
                set<seg>::iterator nxt = next(where[id]), prv = prev(where[id]);  
                if (nxt != s.end() && prv != s.end() && intersect(*nxt, *prv))  
                    return make_pair(prv->id, nxt->id);  
                s.erase(where[id]);  
            }  
        }  
    return make_pair(-1, -1);  
}  

[Sweepline Source Code](https://cp-algorithms.com/geometry/intersecting_segments.html)

MISC stands for 'Minimal Instruction Set Computing' or 'Minimal Instruction Set Computer'. MISC is RISC taken to the extreme, with only one instruction - 'subtract and branch if negative'. Despite this, MISC can perform any calculation computable by a normal RISC (Reduced Instruction Set Computing) or CISC (Complex Instruction Set Computing) machine.
[More About Misc](https://esolangs.org/wiki/MISC#:~:text=MISC%20stands%20for%20'Minimal%20Instruction,subtract%20and%20branch%20if%20negative'.)

int lis(vector<int> const& a) {  
    int n = a.size();  
    vector<int> d(n, 1);  
    for (int i = 0; i < n; i++) {  
        for (int j = 0; j < i; j++) {  
            if (a[j] < a[i])  
                d[i] = max(d[i], d[j] + 1);  
        }  
    }  
    int ans = d[0];  
    for (int i = 1; i < n; i++) {  
        ans = max(ans, d[i]);  
    }  
    return ans;  
}  
[Misc Source Code](https://cp-algorithms.com/sequences/longest_increasing_subsequence.html)
