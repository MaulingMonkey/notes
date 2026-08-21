# Data Storage Security & Destruction

There are multiple conflicting goals
-   Encryption:
    -   Encrypt data to keep it safe from unauthorized access
    -   Keep data decrypted for performance, recoverability, ease of deduplication, etc.
-   Duplication:
    -   Distribute across multiple nodes, backups, etc. to avoid loss of important data
    -   Keep sensitive data on one machine, off of backups, to avoid security holes from e.g. dumpster diving, network infiltration, etc.
-   Deletion:
    -   Immediately remove sensitive data for "right to be forgotten" compliance + to reduce risk of leaking PI
    -   Keep long term (recycling bin, backups, etc.) in case of mistaken deletion
    -   Keep to comply with a legal hold



### Recycling Bin
-   Pros: Gives you a second chance to retain data.
-   Cons: Just adds extra friction, I've wanted to recover data after emptying the recycling bin.



### Expiring Recycling Bin
-   Could delete only when on low disk
-   Could delete only after X days
