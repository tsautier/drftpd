# Issue #156 - User Home Directory Feature

## Description
Add home directory functionality for users to automatically CWD to their home dir on login.

## Implementation Notes
- Add homeDir field to User class
- Modify LoginHandler to CWD to home dir after successful login
- Add SITE commands to set/get user home directory
- Update user.conf with home directory settings

## Status
WIP - Requires User class modification and login flow changes

## Files to Modify
- src/core/master/src/main/java/org/drftpd/master/usermanager/User.java
- src/core/master/src/main/java/org/drftpd/master/commands/login/LoginHandler.java
- Configuration files for user management

## Estimated Complexity
Medium - Requires changes to core user management and login flow
