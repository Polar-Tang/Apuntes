Encerrar cada string (que esta separado por espacios) entre somillas dobles
```
awk '{for(i=1; i<=NF; i++) printf "\"%s\" ", $i; printf "\n"}'
```

