## Log Format

This is how drftpd format the log for external tools to use.

**NEWDIR**

```
NEWDIR: "/path/to/release" "username" "group" "tagline"
```

**SFV**

```
SFV: "/path/to/release" "release_files" "release_size"
```

**UPDATE**

```
UPDATE: "/path/to/release" "username" "group" "release_size"
```

**HALFWAY**

```
HALFWAYNORACE only outputs the current user uploading.
HALFWAYRACE and here u get the leading user and info about the leading group thats uploading.
HALFWAYNORACE: "/path/to/release" "leading_username" "group" "mb_uploaded" "files_uploaded" "percent_uploaded" "avrage_speed"
HALFWAYRACE: "/path/to/release" "leading_username" "group" "mb_uploaded" "files_uploaded" "percent_uploaded" "avrage_speed" "leading_group" "mb_uploaded" "files_uploaded" "percent_uploaded" "avrage_speed"
```

**COMPLETE**

```
COMPLETENORACE: "/path/to/release" "release_size" "release_files" "release_avrage_speed" "release_total_upload_time" "username" "group" "users_fastest_speed"
COMPLETERACE: "/path/to/release" "release_size" "release_files" "release_avrage_speed" "release_total_upload_time" "number_of_racers" "number_of_racing_groups" "fastest_uploader_username" "fastest_uploader_group" "fastest_uploaders_speed" "slowest_uploader_username" "slowest_uploader_group" "slowest_uploaders_speed"
STATSUSER: "/path/to/release" "race_place" "username" "group" "mb_uploaded" "files_uploaded" "percent_uploaded" "avrage_speed"
STATSGROUP: "/path/to/release" "race_place" "group" "mb_uploaded" "files_uploaded" "percent_uploaded" "avrage_speed"
```