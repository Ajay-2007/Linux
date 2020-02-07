## Linux `find` command basics

user@user$ find (location) -name filename_to_be_find

user@user$ find /home -name find_command.md

##### to ignore the `letter Case` use -i flag in the find command
user@user$ find /etc -iname firewallservice.txt

user@user$ find /home -name "*.txt" 

user@user$ find /home -type f -iname  "*.txt"  -exec rm -rf {} +

##### Finding empty files
user@user$ find /home type f -empty
user@user$ find /home type f -empty -exec rm -rf {} +

##### To read the content of the file after finding we use `cat` command
user@user$ find /home type f -iname "firewallservice.txt" -exec cat {} +
user@user$ find /home type f -iname "firewallservice.txt" -exec grep -i char_to_extract {} +

##### To move a file from its current location to computer current location
user@user$ find /home type f -iname "firewallservice.txt" -exec mv -t . {} +

user@user$ find /home type f -iname "firewallservice.txt" -exec cp -t /home/some_directory {} +

##### Find the file according to the size of the file
user@user$ find . -size +50k -type f
user@user$ find . -size +50k -type f -exec du -sh {} + 
here `du` stands for disk usage and `-sh` stands for show

##### It finds the file between 200 and 500 kb size
user@user$ find . -size +200k -size -500k -type f -exec du -sh {} +


##### How to find largest file and smallest file in a particular location
user@user$ find . -type f -exec du -sh {} + | sort -n (it sorts in a ascending order)
user@user$ find . -type f -exec du -sh {} + | sort -nr (it sorts in a decending order)

###### To find top 3 largest file we will pass -3
user@user$ find . -type f -exec du -sh {} + | sort -nr | head -3
user@user$ find . -type f -exec du -sh {} + | sort -nr | tail -3

##### Find the file with a particular permission

user@user$ find . -type f -perm 777
user@user$ find . -type f perm 777 -exec ls -al {} +

##### To find the files without 777 permission
user@user$ find . -type f ! -perm 777 -exec ls -al {} +

##### This will show the file and directory also with 777 permission
user@user$ find . -perm 777 -exec ls -al {} +


