# Analyzing The Wisdom of Crowds For Software Recommendations
Replication package accompanying the paper entitled "Analyzing the Wisdom of Crowds for Software Recommendations", submitted to Empirical Software Engineering Journal for Review"
## Downloading the Stack Overflow Data Dump
The quarterly updated Stack Overflow data dump can be downloaded from [here](https://archive.org/details/stackexchange/). It consists of the following compressed folders:
1. stackoverflow.com-Badges.7z 
2. stackoverflow.com-Comments.7z
3. stackoverflow.com-PostHistory.7z 
4. stackoverflow.com-PostLinks.7z
5. stackoverflow.com-Posts.7z
6. stackoverflow.com-Tags.7z
7. stackoverflow.com-Users.7z
8. stackoverflow.com-Votes.7z

## Extracting the Data Dump
Once the download is complete, uncompress each of these folders, each of which contain a single XML file. Apart from Tags, these XML files are generally large sized files which can't be processed easily in their original form.

## Splitting Large Sized XML Files
For splitting the large sized XML files to multiple smaller sized files which can be easily processed, we used gitbash as described [here](https://stackoverflow.com/questions/31786287/how-to-split-large-text-file-in-windows)
Using the gitbash console, go to the folder containing the large sized XML file that you want to split e.g. Posts.xml and type the following command.
```split -l 50000 -d -a 10 Posts.xml Posts```
Similarly we used the following commands for other files. 
```split -l 50000 -d -a 5 Badges.xml Badges```
```split -l 50000 -d -a 5 Users.xml Users```
```split -l 50000 -d -a 8 Comments.xml Comments```
```split -l 50000 -d -a 5 Votes.xml Votes```
