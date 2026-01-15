# Issue #163 - Constant Mirroring

## Problem
IllegalStateException when destSlaveSet size is 0 with transferNum > 0
Occurs when preset slaves already hold the files to be sent

## Analysis
The issue is in the parent ArchiveType class, not ConstantMirroring.
The send() method in ArchiveType needs to check if files already exist
on destination slaves before creating jobs.

## Status: WIP - Requires ArchiveType Investigation

## Files to Investigate
- src/plugins/archive/src/main/java/org/drftpd/archive/master/archivetypes/ArchiveType.java
- Look for send() method and job creation logic
- Add check to skip files already on all destination slaves

## Complexity: Medium
