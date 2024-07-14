# Analyzing The Wisdom of Crowds For Software Recommendations
Replication package accompanying the paper entitled "Analyzing the Wisdom of Crowds for Software Recommendations", submitted to Empirical Software Engineering Journal for Review"
## Getting the Stack Overflow Data Dump
The quarterly updated Stack Overflow data dump can be downloaded from [here](https://archive.org/details/stackexchange/). It consists of the following compressed folders:
1. stackoverflow.com-Badges.7z 
2. stackoverflow.com-Comments.7z
3. stackoverflow.com-PostHistory.7z 
4. stackoverflow.com-PostLinks.7z
5. stackoverflow.com-Posts.7z
6. stackoverflow.com-Tags.7z
7. stackoverflow.com-Users.7z
8. stackoverflow.com-Votes.7z

### Extracting the Data Dump
Once the download is complete, uncompress the above mentioned folders one by one, each of which contain a single XML file. Apart from Tags and PostLinks, these XML files are generally large sized files which can't be processed easily in their original form.

### Splitting the Large Sized XML Files
For splitting the large sized XML files to multiple smaller sized files which can be easily processed, we used gitbash as described [here](https://stackoverflow.com/questions/31786287/how-to-split-large-text-file-in-windows)
Using the gitbash console, go to the folder containing the original large sized XML file that you want to split e.g. Posts.xml and type the following command.
```
split -l 50000 -d -a 10 Posts.xml Posts
```
Similarly we used the following commands for other files. 
```
split -l 50000 -d -a 5 Badges.xml Badges
```
```
split -l 50000 -d -a 5 Users.xml Users
```
```
split -l 50000 -d -a 8 Comments.xml Comments
```
```
split -l 50000 -d -a 5 Votes.xml Votes
```
After executing the above mentioned command, your parent directory e.g. E:\Dataset_Jun2024\stackoverflow.com-Posts will contain original file (Posts.xml) along with several files other files like Posts0000000000, Posts0000000001 and so on. As the split was performed line by line, the first file will contain the xml header i.e. 
```
<?xml version=\"1.0\" encoding=\"utf-8\"?><posts>
```
whereas the last file will contain the opposite part i.e. ```</posts>```. For the first and the last chunk (file) we will manually add the missing part either ```</posts>``` at the end of first file or the ```<?xml version=\"1.0\" encoding=\"utf-8\"?><posts>``` at the start of last file and save them as ```.xml``` files. The rest of the files will not contain any such header/footer and we need to add it at the start and end of each file. The ```convert_file_chunk_to_xml(....)``` method can be helpful for the remaining files. But before executing this method, move the original Posts.xml and the first and the last chunk (already converted to ```.xml``` to some other location otherwise they will also get modified. The above code snipped when executed will convert the splitted files to proper xml files of small size which can be easily processed further. 
## From Data Dump (Large Sized XML Files) to Database Records
The next step is to read these XML files one by one and store them in a database which can be queried for easy retrieval of relavant records. We used the MS SQL Server Developer Edition as the Express Edition restricts the maximum database size to ```10GB``` only which was not useful in this case. However, the Developer Edition can't be used in a production environment.

### Creating the Databases and Tables
Using SQL Server Management Studio (SSMS),right click on the ```Databases``` under your SQL Server instance in the ```Object Explorer``` and select ```New Database```
In the ```New Database``` pop-up window, type the name of the database. Instead of using a single database to store all the above mentioned files in different tables, we created separate databases corresponding to each of the above mentioned files. e.g. ```StackOverflowPostsDb_April2024```, ```StackOverflowTagsDb_April2024``` and so on. Owing to the large size of data to be stored, make sure to change default Path of database files from ```C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\ ``` to some other directory containing ample amount of storage capacity.

### Converting XML Files to Database Records
We simply read all small xml files one by one and inserted them into the relevant database table using parse methods e.g. # parse_badges(), parse_comments(), parse_post_links() and so on. To execute them without any errors, you may need to install ```lxml``` package in addition to the packages listed in the ```import``` statements. 
