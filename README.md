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
whereas the last file will contain the opposite part i.e. ```</posts>```. For the first and the last chunk (file) we will manually add the missing part either ```</posts>``` at the end of first file or the ```<?xml version=\"1.0\" encoding=\"utf-8\"?><posts>``` at the start of last file. The rest of the files will not contain any such header/footer and we need to add it at the start and end of each file. The following code snippet can be helpful for the remaining files. But before executing this code, move the original Posts.xml to some other location otherwise it will also get modified. 
```
import glob
import os
input_path =r'<directory-containing-splitted-files>'
output_path=r'<output-directory-for-converted-xml-files>\\'

for filename in glob.glob(os.path.join(input_path, '*')):
    with open(os.path.join(os.getcwd(), filename), 'r',encoding='utf8') as f: 
        print("Reading file {}".format(filename))
        Lines = f.readlines()
        xml_lines=[]
        xml_lines.append("<?xml version=\"1.0\" encoding=\"utf-8\"?>\n<posts>")
        xml_lines.extend(Lines)
        xml_lines.append("</posts>")
        head, tail = os.path.split(filename)
        outputfile = output_path+tail+'.xml'      
        print("Writing file to {}".format(outputfile))
        with open(outputfile, "w",encoding='utf8') as output:
            output.write(''.join (xml_lines))
```
The above code snipped when executed will convert the splitted files to proper xml files of small size which can be easily processed further. 
## From XML files to Database Records
The next step is to read these XML files one by one and store them in a database which can be queried for easy retrieval of relavant records. We used the MS SQL Server Developer Edition as the Express Edition restricts the maximum database size to 10GB only which was not useful in this case.

### Creating the Databases and Tables
