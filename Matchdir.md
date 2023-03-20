```
n.filter=matchdir
n.match=/dir/*
n.assign=slave1+remove slave2-500 slave3+remove slave4+500
```

This will remove slave1 and slave3 for transfers under /dir/ It will also give 500 points to slave4 and remove 500 points from slave2.

You may use multiple matchdir filters in your chain