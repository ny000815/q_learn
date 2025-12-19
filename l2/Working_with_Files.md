# Working with Files
`:.                   //current directory
`:folder/otherFolder    // relative path
`:c:/folder/otherFolder     // full path

```q
count key `:.
```

normal sv usage mistake
```q
/cannot take symbol　(same for date)
"/" sv `a`b`c
/ error
"/" sv string `a`b`c
a/b/c
```

⭐️⭐️
sv only takes string vector for normal usage(not ) & using (;;) for sv
```q
/This works.
pathToTable:{[a;b;c]
    ` sv (a;`$string b;c)
    }

pathToTable[`:.;2020.01.01;`trade]

/But this doesn't work?
pathToTable:{[a;b;c]
    ”/” sv (a;`$string b;c)
    }

pathToTable[`:.;2020.01.01;`trade]

-->
/this can work
pathToTable:{[a;b;c]
  "/" sv string each (a; b; c)
}
```


hopen
```q
myFileHandle: hopen `:myLog.txt  //creating a text log file 
myFileHandle                     //handles are stored as integers 
/
1026i
\
key `:myLog.txt    //we can see this in our current directory now! - it returns the path so we know it exists
/
:myLog.txt
\
key `:a.txt
/
()
\

myFileHandle "Writing some text " //writing data to our new file 
myFileHandle "on the same line"   //continuing our write on the same line
/⭐️
read0 `:myLog.txt
/"Writing some text on the same line"
neg[myFileHandle] "this will end with a new line " //writing then starting a new line 
neg[myFileHandle] "Now on the next line"             //Check this in the file!
/
-1026i
-1026i
\
read0 `:myLog.txt`
/
"Writing some text on the same linethis will end with a new line "
"Now on the next line"
\

" " vs' read0 `:myLog.txt  //splitting each line where spaces occur
/
("Writing";"some";"text";"on";"the";"same";"linethis";"will";"end";"with";,"a..
("Now";"on";"the";"next";"line")
\
```
⭐️⭐️
Logging for a func, protectedAdd
```q
/ .[f; args; handler]`
LOG_HANDLE: hopen `:myLog.txt
protectedAdd:{.[+;(x;y);
            {[err] errorMsg: string[.z.p],"| ERROR | protectedAdd failed with error:",err; 
               neg[LOG_HANDLE] errorMsg; 0b }]};
                   
protectedAdd[1;2]   //works fine without logging
protectedAdd[1;"123"]  //returns 0b and we see a message in our file

/easy ver.
protectedAdd:{
    .[+;(x;y);
    {[err]err
        }]
    }
/this doesn't work because hopen is included in the vaiable
read0 LOG_HANDLE
/this works
read0 `:myLog.txt    

```

⭐️⭐️
```q
/("SJF"; enlist csv) 0: <file>
/0: IS func which is dyadic like below(the delims should be list)
/⭐️(types; delims) 0: `:file.csv
show newsummary:("SJF";enlist ",") 0: `$":summary.csv" 
/
sym num price   
----------------
IBM 3   18.7187 
JPM 5   58.29811
KX  2   55.07852
\
("S F";enlist ",") 0: `$":summary.csv" 
/
sym num
-------
IBM 3  
JPM 5  
KX  2
\
("SJ"; enlist ",") 0: `:covidCasesPerCountry.csv

/
US    45000 
------------
UK    30200 
CHINA 94000 
SPAIN 292000
\

/without enlist
("SJF";csv) 0: `$":summary.csv" 
/
sym IBM     JPM      KX      
    3       5        2       
    18.7187 58.29811 55.07852
\
/⭐️You'll notice the column headings are not included in two of our lists - do you know why? 
/This is because we are casting our lists to the appropriate list types indicated, in this case "J" and "F". There is no way to represent the string "num" as a whole literal long type, and so this returns a null value, similarly for "price".
```
