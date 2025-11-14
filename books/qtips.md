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

### Compoound Assingment
>q passes arguments by reference. In most cases, they are immutable references. Attempting to extend, resize, or update a list, for example, will create a complete copy of the original data before modifying the original. This is an inefficient process, especially if our intention is to assign the result back to the original variable. Compound assignment allows simple operators to potentially modify the original data structure, thus removing the need to create another copy. Each of the single character operators + - * % & | ^ , # ! _  has a compound assignment variant. It is also possible to modify structures without creating a copy by adding a layer of indirection and passing the global variable’s name instead of the variable itself.
