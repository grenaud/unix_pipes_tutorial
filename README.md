Welcome! This small guide contains a lot of information and examples that will help you as you start learning Linux. 
In addition to simple Linux commands, this tutorial also provides three Python scripts, that you can run and try out the concepts yourself.

# Basic Unix Commands and Concepts
This is where your journey begins! 
If you are new to Unix-based systems and also got used to Windows or MacOS, you really need a brief explanation of a few key commands and concepts. Vice versa, you will have a heart attack every time while writing some code, thinking hopefully you did not delete some important thing (based on experiences).

## cd (Change Directory):

The `cd` command is used to navigate between directories (folders) in a Unix-based system. 
For example, if you are in a directory called home, and you want to move to a directory inside it called documents, you would type:

```bash
cd documents
```

If you want to move to the parent directory, you can use:

```bash
cd ..
```

If you ever want to return to your home directory, simply type:

```bash
cd
```

Also, you can combine some of them! If you want to move to the parent folder, and then go to another directory from there, you can simply write:

```bash
cd ../directory_path
```

## ls (List Directory Contents)

The `ls` command lists the contents of the current directory you are in. It shows all files and subdirectories within that directory. 
For example, to see what files and directories are inside the current folder, type:

```bash
ls
```

You can also add options to ls to view more details. For instance:

- `ls -l` lists files with detailed information, such as file permissions, size, and modification dates.
- `ls -a` lists all files, including hidden ones (files that start with a dot .).

## mkdir (Creating Directories)

The `mkdir (make directory)` command is used for creating new directories (folders) within the Unix file system. Organizing files into directories helps maintain a structured and manageable file system., which is a good thing.
You can simply create directories from `your current directory` using `mkdir` like this:

```bash
mkdir 'directory_path'
```
For example, if you are in a directory named `my_directory` and want to create a directory named `my_new_directory`, you will write:

```bash
mkdir my_new_directory
```
It will be created without notifying you. But you can check if the directory created by using `ls`. The output of this command should be seen like this:

```
_other folders or files_
my_new_directory
```

Checking it by yourself is not bad, but it would be better if it notifies you when the directory is created. For that, you can use the flag `-v`! The 'v' here means `verbose` and notifies you when the directory is created successfully, or vice versa. How it notifies? Outputting the success message to your terminal, since the terminal is where the standard output goes. What is standard output? We will talk about it later!

```bash
mkdir -v my_new_directory
```
This code now prints out the message that you created successfully the directory.

Now imagine you need to create a folder, in a folder, which is in a folder. Creating all of them would not be that hard, but what if you need to create 20 folders like that? Instead of exhaustively doing that, you can use another flag, `-p`! `-p` flag will create parent directories as well, `__if they are not existing__`. You can achieve this like this:

```bash
mkdir -p my_new_directory/my_another_new_directory/unix_tutorial
```

This code will create all directories if they do not exist. Also, you can combine the flags `-v` and `-p` to get notified at every creating step. 

You can ask yourself why are we splitting all directories with `/` but not using it before the first directory? Normally you can use it, but having `/` at the very first position tells your system that you are trying to do something from the `root` directory. So if you add `/` before the `my_new_directory`, your system will create all folders not from your current location, but from the root directory. Yet you can use this if you want to create a directory rooting from different locations.

## htop

`htop` is an interactive and user-friendly process viewer for Unix systems. It provides a real-time, color-coded display of system processes, CPU usage, memory consumption, and more. If you are used to using Windows systems, `htop` is kinda similar to `Task Manager`.
You can open up `htop` by simply writing:

```bash
htop
```
By writing that, you should get a tab like following:
![htop](https://github.com/user-attachments/assets/0ca69cd7-05e0-40d7-ba0f-8f539fda5b91)

The best thing (for me) `htop` providing is the `with mouse navigating`. You can click the buttons on green line and access CPU-Usage, Memory-Usage and so on.

## nano

`nano` is a simple, user-friendly text editor for Unix-like systems. It is ideal for beginners and those who prefer straightforward editing without the complexity of more advanced editors like `vim` or `emacs`. nano operates entirely within the terminal, allowing quick editing of configuration files, scripts, and other text documents. 

# stdout, stdin and stderr

## stdout (Standard Output)

`stdout` stands for "standard output", where a program sends its regular output. 
In most cases, this is your terminal screen. For example, when a command or program runs successfully, the result is displayed on `stdout`, i.e. your terminal.
You can redirect this output to a file if you don’t want it displayed on the screen. For example, if you have a program that simply writes out "Hello World!" to the terminal, you can redirect this output from the terminal to a file. When you achieve this, it still writes it out to the `stdout` but the directory of `stdout` would be changed. You can achieve this with the following command:

python3 hello_world.py > output.txt

Let's break down this code together. First, we need to write python3 in unix-based systems to call python files successfully. Then, we need to say which file would be called. In this case, let's name our little program `hello_world.py`. When you give only these two as a command, it will normally write out `Hello World!` to the terminal. But instead, we are saying `> output.txt` to redirect the output into a file. You can redirect this output also to the `null container`. Null Container is a place that acting like a black hole. Redirected outputs will be gone forever. You can achieve this with:

python3 hello_world.py > /dev/null

## stdin (Standard Input)

`stdin` stands for "standard input" and is where a program receives its input. By default, this is the keyboard, but it can also come from a file or the output of another command. 
For example, if you run a command and are prompted to type something, that input is coming from `stdin`.

Imagine our `hello_world.py` also says our name! As the program can not know your name, "legally", you need to specify this. You can give your name like this:

python3 hello_world.py ozgur

Aaaaand it won't work. It is because normally, your python code can not understand if an `argument` exists in your command line. The library named `argparse` in python helps you to take inputs from the command line! When you set up argparse and modify your code correctly, it will take input from the command line and process it. 

Now imagine you have two python codes. One of them picks a random name and the second one prints Hello World [name] with given input. You can run your first code, see what it outputs, and use the second code by writing the output of the first code. It won't bother you since you are taking only one name at a time, but imagine inputting 50 random names. To hindering this hard work, you can use `pipes!` Pipe is a kind of operator in unix-based systems, that helps you connect `stdout` and `stdin` of different codes. Also when you want to use the `pipe` operator, you do not need `argparse`. It takes the output of the first one and gives it to the second one.
You can achieve this like this:

python3 random_name_generator.py | python3 hello_world.py
In this case, `random_name_generator.py`'s output becomes `hello_world.py`'s input.

But wait! Yet it does not need to use argparse, you still have to take the output by using `sys.stdin.read()` to take input correctly. The exact usage of it may vary based on your code. 


## stderr (Standard Error)

`stderr` stands for "standard error" and is used by programs to send error messages or diagnostics. 
This is also shown on your terminal screen by default, but it is separate from `stdout`. 
This distinction allows you to handle errors separately if needed.

For example, you can redirect errors to a file without affecting the regular output:
command 2> error_log.txt

This command saves any error messages to `error_log.txt` while still displaying the normal output on the screen or wherever `stdout` is directed.


# stdout, stdin, and stderr in Python

In Python, `stdout`, `stdin`, and `stderr` can also be accessed and manipulated programmatically. Here's how they are used:

## stdout in Python

In Python, `stdout` is accessed using the `sys.stdout` object. When you print something, it is sent to `stdout`. You can manually write to `stdout` as follows:

```python
import sys
sys.stdout.write("This is a message to stdout\n")
```

You can also redirect `stdout` to a file:

```python
import sys
with open('output.txt', 'w') as f:
    sys.stdout = f
    print("This will go to the file instead of the screen")
```


## stdin in Python

Python can read from `stdin` using the `input()` function or directly from `sys.stdin`. Here's an example:

```python
# Using input
user_input = input("Enter something: ")
print(f"You entered: {user_input}")

# Reading directly from sys.stdin
import sys
for line in sys.stdin:
    print(f"Received from stdin: {line.strip()}")
```

You can also redirect `stdin` to read from a file:

```python
import sys
with open('input.txt', 'r') as f:
    sys.stdin = f
    for line in sys.stdin:
        print(f"Reading from file: {line.strip()}")
```

## stderr in Python

Errors in Python can be printed to `stderr` instead of `stdout`, which is useful for separating normal output from error messages. Here's how to write to `stderr`:

```python
import sys
sys.stderr.write("This is an error message\n")
```

You can also redirect `stderr` to a file:

```python
import sys
with open('error_log.txt', 'w') as f:
    sys.stderr = f
    sys.stderr.write("This error message will be written to the file\n")
```


## Summary

- `stdout` in Python is used for regular output and can be redirected to files.
- `stdin` is used to get input from the user or other sources, and it can be redirected to read from files.
- `stderr` is used for error messages and can be redirected to log files for error tracking.

These streams are part of the `sys` module and can be controlled or redirected within your Python programs.



# Random Integer Generator

This script is designed to generate random integers within a specified interval. It is intended for use as an internship task, providing a simple way to produce a list of random numbers.
The script can be executed from the command line with optional arguments to specify the number of integers to generate and the range of values.
It returns none since the generated numbers are written directly to the stdout.

Ensure that the `min` value is less than or equal to the `max` value to avoid errors.
The script writes the output directly to the standard output, which can be redirected to a file if needed.

Arguments are:
- `n` (positional, optional): The number of random integers to generate. Defaults to 100 if not specified.
- `--min` (optional): The minimum value of the interval. Defaults to 10.
- `--max` (optional): The maximum value of the interval. Defaults to 100.

To generate 50 random integers between 1 and 50, you would run:
python random_int_generator.py 50 --min 1 --max 50


# Prime Checker (5 different)

The prime checker scripts are designed to determine if numbers provided via standard input are prime. 
It outputs the prime numbers to standard output and logs the runtime of the operation to a file named runtime_[x].txt, where X is the prime checker algorithm.
To use this script, you need to provide a list of numbers through standard input. The script will then check each number to determine if it is prime and output the prime numbers.
It returns True if the provided number is prime, and False otherwise.

The script can also be used in conjunction with a random integer generator. For example, you can generate random numbers and pipe them directly into the script:
python random_integer_generator.py X --min Y --max Z | python prime_checker.py

In this setup, random_integer_generator.py is a script that outputs random integers to standard output, which are then piped into prime_checker.py for prime checking.

**The pipe (|) is a shell function that takes the output of one command and uses it as the input for another command. 
**In the context of this script, it allows the output of a random integer generator to be directly fed into the prime_checker.py script without the need for intermediate files.

5 different prime checking algorithm provided in this code bundle. These are:
- AKS Primality
- Sieve of Atkin
- Sieve of Eratosthenes
- Trivial Prime Checker
- Miller - Rabin Primality 

The script currently does not utilize command-line arguments, despite setting up an argparse parser.
The itertools module is imported but not used, which could be removed to clean up the code.

# RSA Checker

The RSAchecker.py script is designed to verify the validity of RSA key pairs generated from two lists of prime numbers. 
It reads prime numbers from two files, computes the RSA key pair for each pair of primes, and checks if the encryption and decryption process is successful.

To use this script, you need to provide two files containing prime numbers. Each file should have one prime number per line. The script will read these files, compute RSA key pairs, and verify their validity.
The correct way to use this script follows:
python RSAchecker.py [file_with_primes_1] [file_with_primes_2]

The script can also be used with process substitution to handle input from other scripts or commands, allowing for dynamic generation and checking of RSA key pairs. As follows:
python3 RSAchecker.py <(command1) <(command2)

Where command1 and command2 seems like:
python random_integer_generator.py X --min Y --max Z | python prime_checker.py

So:
python3 RSAchecker.py <(python random_integer_generator.py X --min Y --max Z | python prime_checker.py) <(python random_integer_generator.py X --min Y --max Z | python prime_checker.py)

**Process substitution allows the output of a command to be used as a file input to another command. 
**The syntax <(command) creates a temporary file descriptor that can be read by the script as if it were a regular file. 
**This is particularly useful for chaining commands together without creating intermediate files.

The script assumes that the input files contain valid prime numbers and does not perform primality testing.
The common public exponent used is 65537, which is a commonly used value in RSA cryptography.
