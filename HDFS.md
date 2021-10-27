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
- hdfs dfs -mkdir /user/emile.ekane-ekane/NewDir : to create a new directory
```

```
- hdfs dfs -tail bonjour.txt: display the last KB of the file;
```

```
-  Remove this file from HDFS by hdfs dfs -rm bonjour.txt;
```

```
- Put this file again by hdfs dfs -copyFromLocal bonjour.txt (check with hdfs dfs -ls). This command is similar to hdfs dfs -put;
```

```

- hdfs dfs -chmod go+w bonjour.txt (check its owner, group and rights
with hdfs dfs -ls);
```

```
- hdfs dfs -chmod go-r bonjour.txt (check the rights);
```

```
- hdfs dfs -mv bonjour.txt dossier/bonjour.txt (check with hdfs dfs
-ls -R);
```

```

- hdfs dfs -get dossier/bonjour.txt demat.txt: transfer the file from
HDFS to your Linux account by changing its name. This command should
not be done with real big data!
```

```
- hdfs dfs -cp dossier/bonjour.txt dossier/salut.txt (check);
```

```
- hdfs dfs -count -h /: display the number of subdirectories, files and bytes occupied in /. This command roughly corresponds to -h in Unix;
```

```
- hdfs dfs -rm dossier/bonjour.txt (check with hdfs dfs -ls dossier);
```

```
- hdfs dfs -rm -r -skipTrash dossier (check with hdfs dfs -ls);
```

```

Therefore we understand correctly that there are two file spaces
- The files and directories of our linux account, visible with ls in the ssh window;
- HDFS files and directories, visible byt doing hdfs dfs -ls in the ssh window;
- Delete the local bonjour.txt and demat.txt 
```

```
<br>

For help : [Click here]("https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html")

<br><br>

### 1.3.3 Uploading a file to HDFS
- Download the most frequently downloaded e-book (in Plain Text UTF-8)
from [Project Gutenberg](https://www.gutenberg.org/files/57333/57333-0.txt).  
```

```
- Create a directory called raw inside your HDFS home directory.
- Put the downloaded e-book inside this directory.
```

```
- Create a copy directly inside your HDFS home directory.
```

```
- Rename the copy inside your HDFS home directory to input.txt.
```

```
- Read directly from HDFS the input.txt file.
```

```
- Remove this file (careful with this command).
```

```
- List your HDFS home directory.
```

```
- Retrieve the file from the raw directory from HDFS to the local filesystem
and rename it local.txt.
```

```