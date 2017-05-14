---
author: StevenTTuD
layout: post
title: EDX Linux Foundation Ch13 Manipulating Text
published: true
date: 2014-10-07 14:59
tags:
  - Linux
  - EDX Linux Foundation
comments: true

---
# Section 1 cat and echo
## cat(concatenate)
cat file
顯示檔案，按空白鍵可以捲動

cat file1 file2
把file1和file2接起來顯示

cat file1 file2 > newfile
把file1和file2接起來並且存成newfile。

cat > filename
可以自行輸入內容，輸入完跳出後存成file(可輸入多行)。

cat >> existingfile
代表在檔案後端插入( append )檔案，可以自行輸入內容，輸入完跳出後存成file(可輸入多行)。

cat > filename << EOF
新增檔案的另一個方法，可以自行輸入內容，要離開的時候在句首輸入EOF。

## tac
cat反過來 ，效果是從檔案的後面幾行開始顯示，用法跟cat相同
tac file
tac file file2 > newfile



## echo
跟cat不同是的是echo預設插入或新增一行。
加上```-e```後允許使用`\n`或`\t`...等等的的特殊字元

echo string > newfile
echo string >> existingfile
echo $variable
印出變數。

# Section 2 sed and awk
## sed
sed Command Syntax
sed -e command <filename>
sed -f scriptfile <filename>

## sed 的格式
sed s/pattern/replace_string/ file
sed s/pattern/replace_string/g file
sed 1,3s/pattern/replace_string/g file
sed -i s/pattern/replace_string/g file

> 如果需要寫入檔案，建議直接輸出，不要使用 `-i` 參數，會比較安全。例如：`$ sed s/pattern/replace_string/g file > file2`

## awk
awk ‘command’ var=value file
awk -f scriptfile var=value file

Examples:
awk '{ print $0 }' /etc/passwd
awk -F: '{ print $1 }' /etc/passwd
awk -F: '{ print $1 $6 }' /etc/passwd

# Section 3 File Manipulation

## sort
sort filename
sort -u
sort -r

sort -u 將重複的資料僅列出一個(這邊不太確定)
sort -k pos1[,pos2] 指定排序的key

## uniq
刪除檔案中重複的資料，並儲存到另一個檔案，可以使用以下兩個方法。
sort file1 file2 | uniq > file3
sort -u file1 file2 > file3
使用`-u`比較好，因為uniq本身有bug，只有在兩個相同的東西相鄰時才會合併。

uniq -c filename
計算重複的entry數量

## paste
想要把兩張表合併起來可以用paste來做到，輸出的結果依照delimiters來區分欄位，delimiter可以是tab、空格, comma,  '|'等等。paste並不是很嚴謹的合併，需要先進行set且資料欄位齊全才可以使用。常用在user與group的對應。
paste -d
使用` -d`可以自訂delimiter。
paste -s
paste file1 file2
paste -d, file1 file2
paste -d ':' names phone

## join

$ cat phonebook
```
555-123-4567 Bob
555-231-3325 Carol
555-340-5678 Ted
555-289-6193 Alice
```

$ cat directory
```
555-123-4567 Anytown
555-231-3325 Mytown
555-340-5678 Yourtown
555-289-6193 Youngstown
```

The result of joining
$ join phonebook directory
```
555-123-4567 Bob Anytown
555-231-3325 Carol Mytown
555-340-5678 Ted Yourtown
555-289-6193 Alice Youngstown
```
## split

split infile <Prefix>

範例：把一個字典檔切成99000行
wc可以用來查看檔案的行數或字數
$ wc -l american-english
99171 american-english
$ split american-english dictionary

$ ls -l dictionary*
-rw-rw-r 1 me me 8552 Mar 23 20:19 dictionaryab
-rw-rw-r 1 me me 8653 Mar 23 20:19 dictionaryaa
. . .


# Section 4 grep
用來搜尋文字的工具，會依照設定的pattern來搜尋，pattern中可以使用regular experssion

## grep
grep [pattern] <filename>
grep -v [pattern] <filename>
grep [0-9] <filename>
grep -C 3 [pattern] <filename>
grep -A 3 [pattern] <filename>
grep -B 3 [pattern] <filename>

# Section 5

## tr
用來刪除一段文字，或者替換一段文字。

Command	Usage
$ tr abcdefghijklmnopqrstuvwxyz ABCDEFGHIJKLMNOPQRSTUVWXYZ
Convert lower case to upper case

$ tr '{}' '()' < inputfile >
outputfile	Translate braces into parenthesis

$ echo "This is for testing" | tr [:space:] '\t'
Translate white-space to tabs

$ echo "This is for testing" | tr -s [:space:]
Squeeze repetition of characters using -s

$ echo "the geek stuff" | tr -d 't'
Delete specified characters using -d option

$ echo "my username is 432234" | tr -cd [:digit:]
Complement the sets using -c option

$ tr -cd [:print:] < file.txt
Remove all non-printable character from a file

$ tr -s '\n' ' ' < file.txt
Join all the lines in a file into a single line

## tee
想要將這個資料流的處理過程中將某段訊息存下來，就使用tee指令。
 ls -l | tee newfile

## wc
word count
計算line的數量或字數

## cut
這個指令可以將一段訊息的某一段給他『切』出來

# Section 6
使用 `strings` 需安裝 `binutils`。string可以用來

## 作業更好的解法(By Carl)
Lab 2
`awk -F: '{ print $7 }' /etc/passwd | sort -u | sed '/^$/d'` or
`awk -F: '{ print $7 }' /etc/passwd | sort -u | grep -v '^$'`

Lab 3
解答的做法不好，應該單獨對 `$7` 做處理。

# 讀後心得
課程中對awk sed等強大的指令只有輕輕帶過，但這次的範圍跟之後要學的shell script有密切的關係，開啟了
