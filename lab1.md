# Lab Report Week 1
By Ekin Celik
## Testing Command: cd
**No Argument:**
```
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$ cd
ecelik@EkinLaptop:~$ pwd
/home/ecelik
```
The command "change directory" functions both with and without an argument. If there is no argument, then it will revert back to the home directory: ~. If provided a directory, then cd will change into that directory.

**Directory Argument:**
```
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$ cd messages/
ecelik@EkinLaptop:~/lecture1/messages$ pwd
/home/ecelik/lecture1/messages
```
The command opens the directory specified, as epected, with no error.

**File Argument:**
```
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$ cd README
-bash: cd: README: Not a directory
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
```
The command fails, because 'cd' expects a path to a directory as an argument, and README is not a directory, instead, README is a file, specifically a text file. It would be impossible to change the directory to a file, because a file does not contain any subdirectories, or other files.

## Testing Command: ls
**No Argument**
```
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$ ls
Hello.class  Hello.java  README  messages
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
```
The 'ls' command lists the contents of the working directory if no arguments are given. This output is expected.

**Directory Argument**
```
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$ ls messages/
en-us.txt  es-mx.txt  tr.txt  zh-cn.txt
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$
```
The 'ls' command, when provided an argument of a file path, will print the contents of that file path. This is what happened here, as expected.

**File Argument**
```
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$ ls Hello.java
Hello.java
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
```
The 'ls' command will simply print the name of the file if the passed argument is a file. It will not fail, but it is a quite useless command, except to perhaps relay that the specified file is not a directory.

## Testing Command: cat
**No Argument**
```
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$ cat


testing
testing
testings
testings
testing 2
testing 2
testing 3
testing 3
I'm about to type "ctrl+d"
I'm about to type "ctrl+d"
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$
```
When a user runs 'cat' without any arguments, the terminal seems to freeze, without prompting a new command, but instead, it simply repeats whatever input the user does after they type a newline character. The terminal stops waiting for user input once 'ctrl+d,' or the "logout/exit" character is pressed. This output is expected.

**Directory Argument**
```
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$ cat messages/
cat: messages/: Is a directory
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
```
The 'cat' command seems to expect only a blank argument or a file argument. Providing it a directory causes a fail, as shown here.

**File Argument**
```
ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
ecelik@EkinLaptop:~/lecture1$ cat Hello.java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class Hello {
  public static void main(String[] args) throws IOException {
    String content = Files.readString(Path.of(args[0]), StandardCharsets.UTF_8);
    System.out.println(content);
  }
}ecelik@EkinLaptop:~/lecture1$ pwd
/home/ecelik/lecture1
```
The cat command outputs the contents of a specified file. It has worked as expected here, as it printed the code contained within the .java file.

## _EOF_

Ekin Celik



















