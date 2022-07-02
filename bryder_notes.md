# text for nfs issue

Subject: Problem with COW when using nfs with many nfs clients.

I discovered a problem when using copy on write where I have a readonly NFS mountpoint, and a readwrite NFS mountpoint.

Our use case involves many different nfs client machines accessing the same area at once.

I saw sporadic failures when a client is trying to write many directories deep, and the read write mount point didn't have any of the directories already created.

More than one client was attempting to create the tree at the same time which lead to errors in the mkdir call in unionfs which I saw as 'read-only' filesystem errors.

The next time I ran the test it was fine because all the directories were already created in the read write mountpoint.

After tracing I could see it was an EEXIST error from the mkdir in do_create()
