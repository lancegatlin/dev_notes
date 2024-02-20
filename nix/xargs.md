https://superuser.com/questions/467176/replacement-for-xargs-d-in-osx
```bash
echo {1..4} | xargs -n1 -I{} ssh root@www{}.example.com hostname
```


https://stackoverflow.com/questions/6958689/running-multiple-commands-with-xargs
```bash
cat a.txt | xargs -d $'\n' sh -c 'for arg do command1 "$arg"; command2 "$arg"; ...; done' _
```

