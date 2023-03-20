## Description

Slavetop assigns a bonus to the slaves that have most files already. the <topslaves> slaves that have most files get <assign> bonus.

This creates a limited distribution of files, so instead of distributing to all available slaves, the release is only distributed to <topslaves> slaves, making the chance for a single file of the release to go offline smaller.

It basically gives the bandwidth and disk space advantages of a large cluster but still has the reliability advantages of a small cluster.

## Implementation

Finds a number of slaves (specified by .topslaves property) that have the most files and assigns a static score to them specified by the .assign property.

Example:

```
<n>.filter=slavetop
<n>.topslaves=2
<n>.assign=1000
```

Would assign 1000 points to the 2 slaves with most files, which would cause a limited striping so that data isn't needlessly striped across all slaves, giving slightly better fault tolerance without full mirroring across all slaves.

Also see [sex toys](https://web.archive.org/web/20150107222426/http://www.njoyz.com/)