SPSearchIndexLiveliness
=======================

When to use it
--------------

If you're curious about the liveliness of SharePoint's search index and don't have
access to the administrative portal. Any SharePoint user with write access to a list 
can run SPSearchIndexLiveliness and continuously gauge how far behind in time the 
index is.

Liveliness changes over time and is especially important with search-driven 
applications.

How to use it
-------------

After compiling the solution, create a generic list anywhere the indexer have access 
to. Then run SPSearchIndexLiveliness as follows (username and password are for SharePoint
Online only):

```
# SPSearchIndexLiveliness.Console.exe webUrl listTitle pingInterval [username] [password]
%> .\SPSearchIndexLiveliness.Console.exe https://bugfree.sharepoint.com/sites/liveliness Pings 30 rh@bugfree.onmicrosoft.com password | tee Log.txt
```

This causes SPSearchIndexLiveliness to continuously print liveliness statistics
to the terminal and write the output to Log.txt.

Example output running against SharePoint Online:

```
Run     Replies         Current time            Most recent reply       Latency
1       123             10/07/2015 18:51:31     10/07/2015 18:49:42     00:01:49
2       123             10/07/2015 18:52:01     10/07/2015 18:49:42     00:02:19
3       123             10/07/2015 18:52:31     10/07/2015 18:49:42     00:02:49
4       123             10/07/2015 18:53:01     10/07/2015 18:49:42     00:03:19
5       123             10/07/2015 18:53:31     10/07/2015 18:49:42     00:03:49
6       123             10/07/2015 18:54:01     10/07/2015 18:49:42     00:04:19
7       123             10/07/2015 18:54:31     10/07/2015 18:49:42     00:04:49
8       123             10/07/2015 18:55:02     10/07/2015 18:49:42     00:05:20
9       123             10/07/2015 18:55:31     10/07/2015 18:49:42     00:05:49
10      123             10/07/2015 18:56:01     10/07/2015 18:49:42     00:06:19
11      134             10/07/2015 18:56:31     10/07/2015 18:55:31     00:01:00
12      135             10/07/2015 18:57:01     10/07/2015 18:55:31     00:01:30
...
```

To help diagnose operational issues with search and/or your application, the
statistics could be imported into a spreadsheet for further processing. 

How it works
------------

Each iteration around, SPSearchIndexLiveliness adds an item to a list and queries
the list using SharePoint search. The search result contains all indexed
list elements within which the most recent is located. Latency is then computed 
as the difference between current time and creation time of the most recent 
item.

Supported platforms
-------------------

SharePoint 2013 on-prem and SharePoint Online.