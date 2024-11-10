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
It will be created without notifying you. But you can check if the directory was created by using `ls`. The output of this command should be seen like this:

```
*other folders or files
my_new_directory
```

Checking it yourself is not bad, but it would be better if it would notify you when the directory is created. For that, you can use the flag `-v`! The 'v' here means `verbose` and notifies you when the directory is created successfully, or vice versa. How does it notify? Outputting the success message to your terminal, since the terminal is where the standard output goes. What is standard output? We will talk about it later!

```bash
mkdir -v my_new_directory
```
This code now prints out the message that you created successfully the directory.

Now imagine you need to create a folder, in a folder, which is in a folder. Creating all of them would not be that hard, but what if you need to create 20 folders like that? Instead of exhaustively doing that, you can use another flag, `-p`! `-p` flag will create parent directories as well, `__if they are not existing__`. You can achieve this like this:

```bash
mkdir -p my_new_directory/my_another_new_directory/unix_tutorial
```

This code will create all directories if they do not exist. Also, you can combine the flags `-v` and `-p` to get notified at every creating step. 

You can ask yourself, why are we splitting all directories with `/` but not using it before the first directory? Normally you can use it, but having `/` at the very first position tells your system that you are trying to do something from the `root` directory. So if you add `/` before the `my_new_directory`, your system will create all folders not from your current location, but from the root directory. Yet you can use this if you want to create a directory rooting from different locations.

## htop

`htop` is an interactive and user-friendly process viewer for Unix systems. It provides a real-time, color-coded display of system processes, CPU usage, memory consumption, and more. If you are used to using Windows systems, `htop` is kinda similar to `Task Manager`.
You can open up `htop` by simply writing:

```bash
htop
```
By writing that, you should get a tab like the following:

![htop](https://github.com/user-attachments/assets/0ca69cd7-05e0-40d7-ba0f-8f539fda5b91)

The best thing (for me) `htop` providing is the `with mouse navigating`. You can click the buttons on green line and access CPU-Usage, Memory-Usage and so on.

# stdout, stdin and stderr

## stdout (Standard Output)

`stdout` stands for "standard output", where a program sends its regular output. 
In most cases, this is your terminal screen. For example, when a command or program runs successfully, the result is displayed on `stdout`, i.e. your terminal.
You can redirect this output to a file if you don’t want it displayed on the screen. 

Lets say, you have a program, `hello_world.py`, that simply writes out "Hello World!" to the terminal, looking like this:

```python
print("Hello World!")
```

When you run this command in Linux by writing `python3 hello_world.py` you will see the output `Hello World!` in your terminal. 

Let's break down this code together. First, we need to write `python3` in unix-based systems to call python files successfully. Then, we need to say which file would be called. In this case, the name of our little program is `hello_world.py`. When you give only these two as a command, it will normally write out `Hello World!` to the terminal.

Cool, right? But what if you want to print out this output to a text file named `greeting.txt`? The first way to achieve this, you could change the program itself like this:

```python
import sys

# Redirect stdout to a file
with open("greeting.txt", "w") as file:
    sys.stdout = file
    print("Hello World!")
```

And then, `python3 hello_world.py` would create `greeting.txt`, and append `Hello World!` in it. When you achieve this, it still writes it out to the `stdout` but the directory of `stdout` would be changed. Yet it works for us, it kinda seems a bit exhaustive. 

The second way, and a bit easier way is using directly `file descriptors` of Linux. Using file descriptors is a way to manipulate the outputs, errors, and inputs of programs. Using the very first version of `hello_world.py` and file descriptors, you can achieve it like:

```bash
python3 hello_world.py > greeting.txt
```

The `>` operator here, is one of the basic `file descriptors` in Linux. Using it like that, you will redirect the `output` of the program into a file. In detail, we will talk about it in next chapters.

There may also be a situation where you want to delete the output of the program. You can do this again using file descriptors. The directory named ‘/dev/null’ is a special directory and acts like a black hole, so to speak. Everything you send there will be lost. Suppose we don't want to see the output of `hello_world.py`. We can achieve this as follows: 

```python
python3 hello_world.py > /dev/null
```

## stdin (Standard Input)

`stdin` stands for "standard input" and is where a program receives its input. By default, this is the keyboard, but it can also come from a file or the output of another command. 
For example, if you run a command and are prompted to type something, that input is coming from `stdin`.

Imagine our `hello_world.py` also says our name! As the program can not know your name, "legally", you need to specify this. You can give your name like this:

```bash
python3 hello_world.py ozgur
```

Aaaaand it won't work. It is because normally, your python code can not understand if an `argument` exists in your command line. The library named `argparse` in python helps you to take inputs better from the command line! When you set up argparse and modify your code correctly, it will take input from the command line and process it.

We can modify our little code like this:

```python
import argparse

def main():
    parser = argparse.ArgumentParser(description="Greeting Message")
    parser.add_argument('name', nargs='?', help='Your name to greet correctly')
    args = parser.parse_args()

    print(f"Hello World! {args.name}")

if __name__ == "__main__":
    main()
```

Well, that's a huge modification at all. 

Here what we call 'parser' is our python class. We add an argument to this class and name it 'name'. Then we use parser.parse_args() to get the arguments correctly. This will allow us to keep each argument by flags. So when you type your name in the argument point flagged 'name', you can call it as name.`yourname`. Now, if you call the code like:

```bash
python3 hello_world.py ozgur
```

You will get:

```
Hello World! ozgur
```

Even if it doesn't make sense, we were able to get our output right, that's something.

Now imagine you have two python codes. One of them picks a random name and the second one prints Hello World [name] with the chosen name (our little programme). You can run your first code, see what it outputs, and use the second code by writing the output of the first code. It won't bother you since you are taking only one name at a time, but imagine inputting 50 random names. To hinder this hard work, you can use `pipes!` Pipe is a kind of operator in unix-based systems, that helps you connect `stdout` and `stdin` of different codes. Also when you want to use the `pipe` operator, you do not need `argparse`. By using file descriptors, or pipes, you change the type of the input into a file, so you need to process it like a file.

Let's name our first code `random_name_generator.py`:

```python
import random

names = [
    "Anders", "Niels", "Jens", "Poul", "Lars", "Morten", "Søren", "Thomas", "Peter", "Martin",
    "Henrik", "Jesper", "Frederik", "Kasper", "Rasmus", "Svend", "Jacob", "Simon", "Mikkel", "Christian",
    "Brian", "Steffen", "Jonas", "Mark", "Daniel", "Carsten", "Torben", "Bent", "Erik", "Michael",
    "Viggo", "Oskar", "Emil", "Victor", "Alexander", "Sebastian", "Oliver", "William", "Noah", "Lasse",
    "Mads", "Bjørn", "Leif", "Gunnar", "Elias", "August", "Aksel", "Finn", "Ebbe", "Vladimir",
    "Anne", "Karen", "Pia", "Mette", "Lise", "Hanne", "Rikke", "Sofie", "Camilla", "Maria",
    "Julie", "Christine", "Birthe", "Tine", "Kirsten", "Ingrid", "Line", "Trine", "Kristine", "Mia",
    "Cecilie", "Charlotte", "Emma", "Ida", "Nadia", "Sanne", "Sara", "Eva", "Helene", "Nanna",
    "Maja", "Lærke", "Molly", "Stine", "Emilie", "Amalie", "Signe", "Freja", "Isabella", "Tuva",
    "Viktoria", "Ane", "Dorte", "Laura", "Asta", "Marie", "Clara", "Sofia", "Filippa", "Ella",
    "Alex", "Robin", "Kim", "Sam", "Alexis", "Charlie", "Taylor", "Jamie", "Morgan", "Riley"
]

# Select 10 random names without replacement
random_names = random.sample(danish_names, 10)

# Print each name on a separate line
for name in random_names:
    print(name)
```

And after a little adjustments, our `hello_world.py`:
```python
import sys

def main():
    # Reading names
    for line in sys.stdin:
        name = line.strip()  # Stripping lines
        if name:  # For every name
            print(f"Hello World! {name}")

if __name__ == "__main__":
    main()
```

You can achieve the given task using pipes like this:

```bash
python3 random_name_generator.py | python3 hello_world.py
```

or with file descriptors:

```bash
python3 random_name_generator.py > names.txt
python3 hello_world.py < names.txt
```

Both work perfectly, but notice how easier to use `pipes` for this type of task, compared to file descriptors.

## stderr (Standard Error)

`stderr` stands for "standard error" and is used by programs to send error messages or diagnostics. 
This is also shown on your terminal screen by default, but it is separate from `stdout`. Reading both of them on your terminal would be hard to distinguish them, so redirecting one of them would be better in general.

Let's say we want to print a status message for the `hello_world.py`. After every line is written out as stdout, it should provide the status message, `Name greeted: name`. We can directly print it out with print function like this:

```python
import sys

def main():
    # Reading names
    for line in sys.stdin:
        name = line.strip()  # Stripping
        if name:
            print(f"Hello World! {name}")
            print(f"Name greeted: {name}")

if __name__ == "__main__":
    main()
```

When you run this code, it will output something like that:

```
Hello World! Maria
Name greeted: Maria
Hello World: Anders
Name greeted: Anders
...
```

It works, but it is not something we want to achieve. First, the `status message` is still going to `stdout`. 

If you change the stdout location using the file descriptor, all messages will still go to the same place. So first we need to define the status message as `stderr` and then change the `output location of stderr`.

We can achieve the defining `stderr` like this:

```python
import sys

def main():
    for line in sys.stdin:
        name = line.strip()
        if name:
            print(f"Hello World! {name}")
            print(f"Name greeted: {name}", file=sys.stderr)

if __name__ == "__main__":
    main()
```
`file` is an argument of `print` function in python, which specifies where the output goes. If you give a specific text file to that argument, it prints out there. The default value of it is `sys.stdout`, so basically `stdout`. You can change it by specifying that argument as `file=sys.stderr`. 

Now we want to redirect this status message into a file named `status.txt`. As we do it before, we can use `file descriptors`! Let's try it like this:

```bash
python3 hello_world.py > status.txt
```

Did not work right? That's because the `>` operator redirects only `stdout`. If we want to redirect `stderr`, we specify this with `2>`! But why, we did not use some number for redirecting `stdout`? All `stdout, stderr, and stdin` have values for specifying.

- Standard Input (stdin): File descriptor 0
- Standard Output (stdout): File descriptor 1
- Standard Error (stderr): File descriptor 2

But the default one is `stdout`, so you do not need to define it explicitly. 

Based on this information, we can redirect our status message into `status.txt` with following command:

```bash
python3 hello_world.py 2> status.txt 
```

That's the end of this chapter. Next on, we will talk about a real-world implementation of all concepts above.

# Real World Example

Welcome! This part of the tutorial provides a real-world example where you can use what you have learned above. All of the code examples below can be found in this GitHub repository. So let's get started!

# Random Integer Generator

This script is designed to generate random integers within a specified interval. It is intended for use as an internship task, providing a simple way to produce a list of random numbers.
The script can be executed from the command line with optional arguments to specify the number of integers to generate and the range of values.
It returns none since the generated numbers are written directly to the `stdout`.

Ensure that the `min` value is less than or equal to the `max` value to avoid errors.

Arguments (or flags) are:
- `n` (positional, optional): The number of random integers to generate. Defaults to 100 if not specified.
- `--min` (optional): The minimum value of the interval. Defaults to 10.
- `--max` (optional): The maximum value of the interval. Defaults to 100.

To generate 50 random integers between 1 and 50, you would run:

```bash
python3 random_int_generator.py 50 --min 1 --max 50
```

Since this code prints out all the numbers as we did in `random_name_generator`, we will process it as we did in `hello_world.py`. 

This code also provides the `runtime`. By measuring the runtime, you can evaluate how quickly the program generates the desired number of random integers within the specified interval. This information is crucial for optimizing the code, especially when scaling up to generate larger datasets or integrating the generator into larger applications where performance may impact overall system efficiency. And while this process ongoing, you can check memory or CPU usage by using `htop`, as we talked about in the previous section.

# Prime Checkers (Naive and 4 Others)

The prime checker scripts are designed to determine if numbers provided via standard input (`stdin`) are prime. 
It outputs the prime numbers to standard output and logs the runtime of the operation to a file named `runtime_[x].txt`, where `x` is the name of the prime checker algorithm.
To use this scripts, you need to provide a list of numbers through standard input. The script will then check each number to determine if it is prime and output the prime numbers.
It returns prime numbers line by line.

Let's start talking about the `trivial` one. The `trivial` one, which can be found in this repository as (`prime_checker.py`), is a function that efficiently determines whether a given number `num` is prime. It first excludes numbers less than or equal to 1 and directly identifies 2 and 3 as prime. It then eliminates any even numbers and multiples of 3 to reduce unnecessary checks. For numbers greater than 3, the function iterates from 5 up to the square root of num, checking divisibility in steps of 6. This approach leverages the fact that all primes greater than 3 are of the form `6k ± 1`, thereby minimizing the number of iterations and enhancing performance compared to the naive method of checking all numbers up to `num - 1`. If no divisors are found, the function concludes that num is prime.

Since this script needs a list of integers, you can take these integers from `random_int_generator.py`! Instead of exhaustively having these numbers and feeding them into `prime_checker.py` separately, let's use the brand new thing we learned, `pipes`!

You can pipe both scripts like this:
```bash
python3 random_integer_generator.py | python prime_checker.py
```

As we specified earlier, the random integer generator generates 100 numbers between 10 and 100, so our prime checker would be fed with them. It will then print out only `prime ones`. That means, the original output of `random_int_generator.py` would be omitted since it has been redirected to the `prime_checker.py`. Also this prime checker code provides the runtime to the user, for assessing the performance of this code.

There are 4 different prime checker algorithms provided in this repository, all could be called with the same command by changing the name of the `python file`. 
These are:
- AKS Primality (`prime_aks.py`)
- Sieve of Atkin (`prime_atkin.py`)
- Sieve of Eratosthenes (`prime_ars.py`)
- Miller - Rabin Primality (`prime_miller-rabin.py`)

These are all providing `runtime`, so check yourself which one runs faster when it comes to checking the high volume of random integers.

# RSA Checker

The RSAchecker.py script is designed to verify the validity of RSA key pairs generated from two lists of prime numbers. 
It reads prime numbers from two files, computes the RSA key pair for each pair of primes, and checks if the encryption and decryption process is successful. If you want to learn about it further, you can find information about RSA encryption further on the internet.

To use this script, you need to provide two files containing prime numbers, one is so-called `public keys`, and the other one is `private keys`. Each file should have one prime number per line. The script will read these files, compute RSA key pairs, and verify their validity.

The correct way to use this script follows:
python RSAchecker.py [file_with_primes_1] [file_with_primes_2]

But since we do not have the prime numbers in files, we need to utilize `file descriptors`! A way to use two file descriptors at the same time is by bundling commands together with parenthesis. It will bundle the codes together and redirects the output of all code inside the parentheses. A little confusing, right? Let's break it down, using an example:

```bash
python3 RSAchecker.py <(python random_integer_generator.py | python prime_checker.py) <(python random_integer_generator.py | python prime_checker.py)
```
We knew the part inside the parentheses, it outputs a list of prime numbers. Now, we do it two times since we need a pair of prime numbers. We bundled the parts that output prime numbers and redirected them to the `RSAchecker.py`. `<` indicates that the input goes into the file, so that is the reverse of what we did in the previous section. 

And voila! It worked perfectly, and we have valid prime number pairs for encryption. 

Congrats! Your encryption works!

## Thanks for the attention! See you in another tutorial!
Written by Özgür Yolcu

Instructed by Gabriel Renaud
