Welcome! This small guide contains a lot of information and examples that will help you as you start learning Linux. 
In addition to simple Linux commands, this file also provides three Python scripts that you can run and try out for yourself.

# Unix Commands and Concepts
If you are new to Unix or Linux systems, here is a brief explanation of a few key commands and concepts that are used when working with the command line:

*cd (Change Directory):

The cd command is used to navigate between directories (folders) in a Unix-based system. 
For example, if you are in a directory called home, and you want to move to a directory inside it called documents, you would type:

cd documents

If you want to move to a higher-level directory (the parent directory), you can use:

cd ..

If you ever want to return to your home directory, simply type:

cd

*ls (List Directory Contents)

The ls command lists the contents of the current directory you are in. It shows all files and subdirectories within that directory. 
For example, to see what files and directories are inside the current folder, type:

ls

You can also add options to ls to view more details. For instance:

- ls -l lists files with detailed information, such as file permissions, size, and modification dates.
- ls -a lists all files, including hidden ones (files that start with a dot .).


*stdout (Standard Output)

`stdout` stands for "standard output" and is where a program sends its regular output. 
In most cases, this is your terminal screen. For example, when a command or program runs successfully, the result is displayed on `stdout`.
You can redirect this output to a file if you don’t want it displayed on the screen. For example:

[command] > output.txt

This command takes what would normally appear on the screen and saves it to a file called `output.txt`.


*stdin (Standard Input)

`stdin` stands for "standard input" and is where a program receives its input. By default, this is the keyboard, but it can also come from a file or the output of another command. 
For example, if you run a command and are prompted to type something, that input is coming from `stdin`.

You can also pipe the output of one command as the input (`stdin`) for another command like this:

command1 | command2
In this case, `command1`'s output becomes `command2`'s input.


*stderr (Standard Error)

`stderr` stands for "standard error" and is used by programs to send error messages or diagnostics. 
This is also shown on your terminal screen by default, but it is separate from `stdout`. 
This distinction allows you to handle errors separately if needed.

For example, you can redirect errors to a file without affecting the regular output:
command 2> error_log.txt

This command saves any error messages to `error_log.txt` while still displaying the normal output on the screen or wherever `stdout` is directed.


*stdout, stdin, and stderr in Python

In Python, `stdout`, `stdin`, and `stderr` can also be accessed and manipulated programmatically. Here's how they are used:

*stdout in Python

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


*stdin in Python

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


*stderr in Python

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


**Summary

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
