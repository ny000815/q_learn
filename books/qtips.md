# q tips  
### pointer-like behavior by symbol (assignment)

```q  
ch set x:x+1;til 10  
/ch  
get `ch  
/1 4 9 16 25 36 49 64 81 100  
show a:`ch  
/`ch
get a  
/1 4 9 16 25 36 49 64 81 100  
a set x:i+1;til 10  
/a
get `ch  
/1 2 3 4 5 6 7 8 9 10  
get a  
/1 2 3 4 5 6 7 8 9 10  
a  
/ch  
```

