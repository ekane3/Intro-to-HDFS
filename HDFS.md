# HDFS Architecture
## *Emile .E. EKANE*
## *15/09/2021*

<br><br>

# 1. HDFS  
## 1.1 Introduction
We will be working on the Hadoop cluster, using and programming
HDFS (Hadoop Distributed File System).
This is the storage space for distributed files. It looks like the Unix tree and
there has very similar commands for manipulating files and directories. There
is also an API Java for writing programs for processing HDFS files.
The goal of this lab is to learn the concepts and commands in order to properly manage the files on HDFS. Secondly, use the Java API to view files.  <br><br><br>

## 1.2 Working with the Hadoop cluster  

- Connect to edge node : **hadoop-edge01.efrei.online (M1)** or **hadoop-edge02.efrei.online
(M2).**
- Create a new text document called bonjour.txt  
```
cat > bonjour.txt
#And insert
Its Emile
```
- List the files with ls, display the bonjour.txt file, edit it with nano or v
```
ls
nano bonjour.txt
```
- Display the file’s content, you will see that it has been modified
```
tail bonjour.txt
```
<br><br>

## 1.3 Working on HDFS

### 1.3.1 Kerberos
Initialise your Kerberos ticket using the `kinit` command.
<br><br>

### 1.3.2 Basic HDFS Commands
Trying basic commands
-  hdfs dfs -ls  
```
[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -ls
Found 2 items
drwx------   - emile.ekane-ekane emile.ekane-ekane          0 2021-09-30 20:00 .Trash
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-09-30 14:37 raw
```
- hdfs dfs -ls / 
```
[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -ls /
Found 13 items
drwxrwxrwt   - yarn   hadoop          0 2021-10-13 16:55 /app-logs
drwxr-xr-x   - hdfs   hdfs            0 2021-09-28 10:18 /apps
drwxr-xr-x   - yarn   hadoop          0 2021-09-02 20:56 /ats
drwxr-xr-x   - hdfs   hdfs            0 2021-09-02 20:56 /atsv2
drwxr-xr-x   - mapred hdfs            0 2021-09-02 20:56 /mapred
drwxrwxrwx   - mapred hadoop          0 2021-09-02 20:56 /mr-history
drwxr-xr-x   - hdfs   hdfs            0 2021-09-02 20:56 /odp
drwxr-xr-x   - hdfs   hdfs            0 2021-09-02 20:56 /ranger
drwxr-xr-x   - hdfs   hdfs            0 2021-09-02 20:56 /services
drwxrwxrwx   - spark  hadoop          0 2021-09-02 21:02 /spark2-history
drwxrwxrwx   - hdfs   hdfs            0 2021-10-13 15:20 /tmp
drwxr-xr-x   - hdfs   hdfs            0 2021-10-05 18:19 /user
drwxr-xr-x   - hdfs   hdfs            0 2021-09-02 20:58 /warehouse

```
- hdfs dfs -ls -R -h / : to display files in subdirectories, with rounded size in
KB, MB or GB (the -h = human units option also exists on Unix).
```
#too large to print the results
```
- hdfs dfs -mkdir /user/emile.ekane-ekane/NewDir : to create a new directory name NewDir

- hdfs dfs -tail bonjour.txt: display the last KB of the file;

-  Remove this file from HDFS by hdfs dfs -rm bonjour.txt;
```
> hdfs dfs -rm bonjour.txt
```
- Put this file again by hdfs dfs -copyFromLocal bonjour.txt (check with hdfs dfs -ls). This command is similar to hdfs dfs -put;
```
> hdfs dfs -put bonjour.txt
```

- hdfs dfs -chmod go+w bonjour.txt : Give its owner and group the rights to write in the file.To check the rights we type :
```
hdfs dfs -ls -l
```
- hdfs dfs -chmod go-r bonjour.txt : This commands remove the right to read the file by the group or user of the file.

- hdfs dfs -mv bonjour.txt dossier/bonjour.txt : This command change the file location to the /dossier/ directory;
```
hdfs dfs -ls -R
```

- hdfs dfs -get dossier/bonjour.txt demat.txt: transfer the file from
HDFS to your Linux account by changing its name from bonjour.txt to demat.txt. This command should
not be done with real big data!

- hdfs dfs -cp dossier/bonjour.txt dossier/salut.txt: This command copy the file bonjour.txt to the file salut.txt. To check type : 
```
hdfs dfs -ls /dossier
```
- hdfs dfs -count -h / : display the number of subdirectories, files and bytes occupied in /. This command roughly corresponds to -h in Unix;
```
[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -count -h /user/emile.ekane-ekane
          13          140             27.9 G /user/emile.ekane-ekane

```
This command outputs the DIR_COUNT FILE_COUNT CONTENT_SIZE PATHNAME .
- hdfs dfs -rm dossier/bonjour.txt (check with hdfs dfs -ls dossier);

- hdfs dfs -rm -r -skipTrash dossier (check with hdfs dfs -ls);


Therefore we understand correctly that there are two file spaces
- The files and directories of our linux account, visible with ls in the ssh window;
- HDFS files and directories, visible byt doing hdfs dfs -ls in the ssh window;
- Delete the local bonjour.txt and demat.txt 
```
> rm bonjour.txt demat.txt
```
<br>

Documentation for all commands is on this is [here]("https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html")  

<br>


<br><br>

### 1.3.3 Uploading a file to HDFS
- Download the most frequently downloaded e-book (in Plain Text UTF-8)
from [Project Gutenberg](https://www.gutenberg.org/files/57333/57333-0.txt).  
We download the file
```
> wget https://www.gutenberg.org/files/57333/57333-0.txt

[emile.ekane-ekane@hadoop-edge01 ~]$ ls
57333-0.txt    bonjour.txt  message.txt  reducer.py                  sudoku.dta  ulysse.txt
57333-0.txt.1  mapper.py    outline.txt  secret-of-the-universe.txt  text.txt    vinci.txt

```
Let's rename the file to gut.txt
```
> mv 57333-0.txt gut.txt

[emile.ekane-ekane@hadoop-edge01 ~]$ ls
57333-0.txt.1  bonjour.txt  gut.txt  mapper.py  message.txt  outline.txt  reducer.py  secret-of-the-universe.txt  sudoku.dta  text.txt  ulysse.txt  vinci.txt

```

- Create a directory called raw inside your HDFS home directory.
```
> hdfs dfs -mkdir /raw

[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -ls
Found 13 items
drwx------   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-25 02:00 .Trash
drwx------   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 10:43 .staging
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-18 17:06 NewDir
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-24 12:12 data
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-24 19:04 gutenberg
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 08:26 gutenberg-output
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 10:43 gutenberg-outputs
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane        540 2021-10-24 19:45 mapper.py
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-09-30 14:37 raw
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane       1053 2021-10-24 19:45 reducer.py
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane        162 2021-10-21 14:15 sudoku.dta
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane     798774 2021-10-21 13:41 text.txt
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-21 13:49 wordcount

```
- Put the downloaded e-book inside this directory.
```
> hdfs dfs -put gut.txt /user/emile.ekane-ekane/raw


[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -ls /user/emile.ekane-ekane/raw
Found 1 items
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane    5426096 2021-10-28 09:23 /user/emile.ekane-ekane/raw/gut.txt

```
- Create a copy directly inside your HDFS home directory.
```
> hdfs dfs -cp /user/emile.ekane-ekane/raw/gut.txt /user/emile.ekane-ekane/

[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -ls /user/emile.ekane-ekane/
Found 14 items
drwx------   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-25 02:00 /user/emile.ekane-ekane/.Trash
drwx------   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 10:43 /user/emile.ekane-ekane/.staging
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-18 17:06 /user/emile.ekane-ekane/NewDir
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-24 12:12 /user/emile.ekane-ekane/data
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane    5426096 2021-10-28 09:36 /user/emile.ekane-ekane/gut.txt

```
- Rename the copy inside your HDFS home directory to input.txt.
```
> hdfs dfs -mv /user/emile.ekane-ekane/gut.txt /user/emile.ekane-ekane/input.txt 

[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -ls /user/emile.ekane-ekane/
Found 14 items
drwx------   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-25 02:00 /user/emile.ekane-ekane/.Trash
drwx------   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 10:43 /user/emile.ekane-ekane/.staging
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-18 17:06 /user/emile.ekane-ekane/NewDir
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-24 12:12 /user/emile.ekane-ekane/data
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-24 19:04 /user/emile.ekane-ekane/gutenberg
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 08:26 /user/emile.ekane-ekane/gutenberg-output
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 10:43 /user/emile.ekane-ekane/gutenberg-outputs
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane    5426096 2021-10-28 09:36 /user/emile.ekane-ekane/input.txt

```
- Read directly from HDFS the input.txt file.
```
> hdfs dfs -cat /user/emile.ekane-ekane/input.txt
```
- Remove this file (careful with this command).
```
> hdfs dfs -rm /user/emile.ekane-ekane/input.txt

[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -rm /user/emile.ekane-ekane/input.txt
21/10/28 09:49:19 INFO fs.TrashPolicyDefault: Moved: 'hdfs://efrei/user/emile.ekane-ekane/input.txt' to trash at: hdfs://efrei/user/emile.ekane-ekane/.Trash/Current/user/emile.ekane-ekane/input.txt

```
- List your HDFS home directory.
```
> hdfs dfs -ls /user/emile.ekane-ekane


[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -ls /user/emile.ekane-ekane/
Found 13 items
drwx------   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-28 09:49 /user/emile.ekane-ekane/.Trash
drwx------   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 10:43 /user/emile.ekane-ekane/.staging
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-18 17:06 /user/emile.ekane-ekane/NewDir
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-24 12:12 /user/emile.ekane-ekane/data
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-24 19:04 /user/emile.ekane-ekane/gutenberg
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 08:26 /user/emile.ekane-ekane/gutenberg-output
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-27 10:43 /user/emile.ekane-ekane/gutenberg-outputs
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane        540 2021-10-24 19:45 /user/emile.ekane-ekane/mapper.py
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-28 09:23 /user/emile.ekane-ekane/raw
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane       1053 2021-10-24 19:45 /user/emile.ekane-ekane/reducer.py
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane        162 2021-10-21 14:15 /user/emile.ekane-ekane/sudoku.dta
-rw-r--r--   3 emile.ekane-ekane emile.ekane-ekane     798774 2021-10-21 13:41 /user/emile.ekane-ekane/text.txt
drwxr-xr-x   - emile.ekane-ekane emile.ekane-ekane          0 2021-10-21 13:49 /user/emile.ekane-ekane/wordcount

```
- Retrieve the file from the raw directory from HDFS to the local filesystem
and rename it local.txt.
```
> hdfs dfs -get /user/emile.ekane-ekane/raw/gut.txt local.txt

[emile.ekane-ekane@hadoop-edge01 ~]$ hdfs dfs -get /user/emile.ekane-ekane/raw/gut.txt local.txt
[emile.ekane-ekane@hadoop-edge01 ~]$ ls
57333-0.txt.1  gut.txt    mapper.py    outline.txt  secret-of-the-universe.txt  text.txt    vinci.txt
bonjour.txt    local.txt  message.txt  reducer.py   sudoku.dta                  ulysse.txt

```
