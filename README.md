key-visitor
===========

in python, we can sort or filter list by object's key
```
class Vec:
    def __init__(self, x, y, z):
        self.x = x
        self.y = y
        self.z = z
    
    def sum(self):
        return self.x + self.y + self.z

l = [Vec(1,2,3),Vec(2,3,4), Vec(4,5,6)]
```

if we want sort l by x, we can call:`sort(l, key=operator.attrgetter('x'))`,
but if in c++, we may use a function or lambda like this:
```
sort(begin(l), end(l), [](Vec const& l, Vec const& r)){ return l.x < r.x });
```

key-visitor is a convenience library to adaptor the member variable or variable getter
to the unary or binary function list `std::less<T>`, then it can be rewrite like
```
sort(begin(l), end(l), binary_of<less>(&Vec::x));
```

sort by sum like:
```
sort(begin(l), end(l), binary_of<less>(&Vec::sun));
```

unique vector by sum like
```
unique(begin(l), end(l), binary_of<equal>(&Vec::sum));
```

if a binary function wrapper by a key,
the function can called both the raw objeact and the return type,
like 

```
binary_of<less>(&Vec::x)(v, 0);//test v.x < 0
binary_of<less>(&Vec::x)(0, v);//test 0 < v.x
binary_of<less>(&Vec::x)(v1,v2);//test v1.x < v2.x
```
