Example slaveselection.conf entry:

```
<n>.filter=minfreespace
<n>.minfreespace=1GB
<n>.multiplier=1
```

Works like this:

```
if(diskfree < minfreespace) {
  addScore((diskfree - minfreespace) * multiplier)
}
```

If multiplier == 0, the slave is removed as a candidate.