What do I do if "TypeError: Failed to fetch" is displayed when I use a browser to access a bucket or an object? 
====================================================================================================================================



Causes: The following reasons may cause this problem.

* The network connection is abnormal.

  

* The ad blocker installed in your browser filters the name of the bucket or object because the name of the bucket or object contains the string "ad" such as adtest or aadb.

  




Solution:

* Network exception: Check your network and try again after the exception is resolved.

  

* Ad blocker:

  * Disable the ad blocker of your browser or add the OSS domain name to the whitelist.

    
  
  * Do not include the string "ad" in the name of your buckets or objects.

    
  

  




