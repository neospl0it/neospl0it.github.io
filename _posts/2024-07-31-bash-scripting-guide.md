---
title: "Bash Scripting"
description: "An introduction to Bash scripting, exploring essential concepts, commands, and techniques for automating tasks in Linux."
date: 2024-07-31
categories: [Linux, Bash-Scripting]
tags: [bash, scripting, automation, linux]
image:
  path: thbnls/bash.png
---

---

## Theory

![Pasted image 20240730132838](https://github.com/user-attachments/assets/35a3e652-0de1-4980-9bc0-3004642cb192)

**Operating System (OS):** A system program that provides an interface between the user and the computer. Examples include Windows, Linux, and Android.

**Kernel:** A computer program that is the heart (core) of an operating system. It facilitates communication between hardware and software.

The kernel manages the following:

- **File Management**
- **Process Management**
- **I/O Management**
- **Memory Management**
- **Device Management**

![Pasted image 20240730133327](https://github.com/user-attachments/assets/e74ead71-6899-4c4a-a2e9-12b3b55ba10c)

**The Shell:** A computer program that allows you to directly interact with a computer operating system using either a command-line interface (CLI) or a graphical user interface (GUI). It accepts human-readable commands from the user and converts them into instructions that the kernel can understand.

- **Kernel:** The innermost part of an operating system, responsible for managing system resources and facilitating communication between hardware and software.
- **Shell:** The outermost part of an operating system that interacts with the user, receiving commands and passing them to the kernel for execution.

**Shell Scripting:** A series of commands written in a file, which are read and executed by a shell program, typically the Bash (Bourne Again Shell) program in Unix-like operating systems.

**Bourne Again Shell (Bash):** A command-line interface shell program used in Linux and macOS (UNIX-based). It can be used like any other programming language to write scripts and execute them. Bash is the default shell for many Linux distributions.

**Bourne Shell (sh):** The first Unix shell ever made.

There are many other shells as well, such as:

- **C Shell (csh)**
- **Korn Shell (ksh)**
- **Z Shell (zsh)**

#### Why We Use Bash?

- **Automate repetitive tasks:** Bash scripting helps to automate routine and repetitive tasks, saving time and reducing the chance of human error.
- **Customizing administrative tasks:** System administrators can create custom scripts to manage and maintain systems more efficiently.
- **Run multiple commands as a single command:** Bash allows the execution of multiple commands in a sequence or conditionally, streamlining complex workflows.
- **Create hacking tools for ethical hacking/pen testing:** Ethical hackers and penetration testers can use Bash to develop custom tools and scripts for security testing and vulnerability assessment.

## Coding

**Command to Know Our Bash File**

```bash
┌──(root㉿neo)-[~]
└─# which bash         
/usr/bin/bash
```

**Hello World:**

```bash
┌──(root㉿neo)-[~]
└─# echo "Hello World"                            
Hello World
```

![Pasted image 20240730135629](https://github.com/user-attachments/assets/155c5f71-37cb-4a75-ad5f-1827f2a7f9c4)

![Pasted image 20240730140546](https://github.com/user-attachments/assets/9fde2dca-47f3-4980-a9da-33e92c343d43)

```bash
┌──(root㉿neo)-[~/Bash]
└─# ls -l
total 4
-rw-r--r-- 1 root root 36 Jul 30 13:57 helloworld.sh
                                                                                
┌──(root㉿neo)-[~/Bash]
└─# chmod a+x helloworld.sh 
                                                                                
┌──(root㉿neo)-[~/Bash]
└─# ls -l
total 4
-rwxr-xr-x 1 root root 36 Jul 30 13:57 helloworld.sh   
                                                                                
┌──(root㉿neo)-[~/Bash]
└─# ./helloworld.sh 
Hello World
              
```

```bash
┌──(root㉿neo)-[~/Bash]
└─# cat helloworld.sh 
#!/usr/bin/bash

echo "Hello World"
sleep 2
echo "Welcome to Neosploit"
sleep 3
echo "Learn Bash Scripting"
                                                                                
┌──(root㉿neo)-[~/Bash]
└─# ./helloworld.sh   
Hello World
Welcome to Neosploit
Learn Bash Scripting
```

**Sleep:** Creates a delay before displaying the next message. 

In this script:
- `sleep 2` pauses for 2 seconds before displaying "Welcome to Neosploit".
- `sleep 3` pauses for 3 seconds before displaying "Learn Bash Scripting".

## Variables

### User Defined Variables

```bash
#!/usr/bin/bash

name="John"
echo $name
                                                                                
┌──(root㉿neo)-[~/Bash]
└─# ./variables.sh 
John
```

In this example, the variable is `name`. We stored the data "John" into the variable `name`.

- **Defining a variable:** `name="John"`
- **Accessing a variable:** `$name`

**Note:** Do not start variable names with numbers.

**Note:** When declaring a variable in Bash, do not give spaces around the `=` sign. The correct syntax for variable assignment is:

```bash
variable_name=value
```

#### Correct Example:

```bash
name="John"
```

#### Incorrect Example:

```bash
name = "John"  # This will result in an error or unintended behavior
```

The lack of spaces ensures that the shell correctly interprets the assignment and assigns the value to the variable.

### The `$` Symbol

In Bash scripting, the `$` symbol is used to reference the value of a variable. When you prefix a variable name with `$`, it tells the shell to retrieve the value stored in that variable. Here's a breakdown of why and how the `$` symbol is used:

#### Purpose of `$` Symbol

1. **Retrieve Variable Value:** 
   - When you use `$variable_name`, the shell replaces `$variable_name` with the value stored in that variable.
   - Example: If you have `name="John"`, using `echo $name` will output `John`.

2. **Command Substitution:**
   - The `$` symbol is also used in command substitution to capture the output of a command.
   - Example: `current_date=$(date)` stores the output of the `date` command in the variable `current_date`.

3. **Parameter Expansion:**
   - `$` can be used to perform operations on variables, such as trimming or substituting text.
   - Example: `${variable_name#prefix}` removes the shortest match of `prefix` from the start of the value of `variable_name`.

#### Examples

1. **Simple Variable Usage:**
   ```bash
   name="John"
   echo $name  # Output: John
   ```

2. **Command Substitution:**
   ```bash
   current_date=$(date)
   echo $current_date  # Output: Current date and time
   ```

3. **Parameter Expansion:**
   ```bash
   path="/usr/local/bin"
   echo ${path#/usr}  # Output: /local/bin
   ```

In summary, the `$` symbol is essential for working with variables in Bash, allowing you to access, manipulate, and utilize the values stored in them.

### Curly Braces `{}` for Variables

In Bash scripting, you can use variables inside curly braces `{}` to explicitly define the variable name, especially when it's used in strings or when the variable is followed by characters that might be part of the variable name. This helps in distinguishing the variable from the surrounding text.

#### Example:

```bash
#!/usr/bin/bash

name="John"
age=20
echo "My Name is ${name}, I am ${age} years old"
```

#### Output:

```
My Name is John, I am 20 years old
```

#### Key Points:

- **With Curly Braces `{}`:**
  - `${name}` and `${age}` clearly indicate the boundaries of the variable names. This is useful when concatenating variables with other text or when the variable is part of a longer string.
  - Example: `${name}123` will output `John123`.

- **Without Curly Braces:**
  - `name` and `age` can also be used directly in many cases, especially if they are alone or followed by whitespace or punctuation.
  - Example: `echo "My Name is $name"` will work and produce `My Name is John`.

#### When to Use Curly Braces:

- When variables are immediately followed by letters, numbers, or other characters that could be part of the variable name.
- To enhance readability and avoid ambiguity.

In general, using curly braces is a good practice for clarity and to avoid potential issues in more complex scripts.

### Case Sensitivity in Variables

Variables in Bash are case sensitive. This means `age`, `Age`, and `AGE` are considered different variables.

#### Example:

```bash
#!/usr/bin/bash

name="John"
age=20
Age=22
AGE=24
echo "My Name is ${name}, I am ${age} years old"
```

#### Output:

```
My Name is John, I am 20 years old
```

### Key Points:

- **Case Sensitivity:** Bash variables differentiate between lowercase and uppercase letters. So `age`, `Age`, and `AGE` are treated as separate variables.
- **Usage:** Always be consistent with the casing of variable names to avoid confusion and errors in your scripts.

In this example, only the variable `age` is used in the `echo` command, so only its value (`20`) is displayed. If you need to use `Age` or `AGE`, you would need to reference them explicitly in your script.

### Comments in Bash

In Bash scripting, the `#` symbol is used to denote a comment. Anything following the `#`

 symbol on the same line is ignored by the Bash interpreter. Comments are useful for adding explanations, notes, or documentation to your script, making it easier to understand and maintain.

#### Example:

```bash
#!/usr/bin/bash

# This is a comment explaining the purpose of the script
name="John"  # Assign the value "John" to the variable 'name'
age=20  # Assign the value 20 to the variable 'age'

# Print the name and age
echo "My Name is ${name}, I am ${age} years old"
```

#### Output:

```
My Name is John, I am 20 years old
```

### Key Points:

- **Single-line Comments:** Use the `#` symbol to add a comment on a single line.
  - Example: `# This is a comment`
- **Inline Comments:** You can add comments at the end of a line of code.
  - Example: `name="John"  # Assign the value "John" to the variable 'name'`
- **Ignored by Bash:** Comments are not executed by the Bash interpreter and are purely for human readers.

Using comments effectively helps in documenting your code, making it easier for others (and yourself) to understand the logic and purpose of your script.

## System Variables

System variables are predefined and maintained by the shell. These variables provide information about the system and the shell environment. They are usually written in uppercase letters.

### Key System Variables:

1. **`HOME`:** The home directory of the current user.
2. **`PWD`:** The current working directory.
3. **`USER`:** The username of the current user.
4. **`UID`:** The user ID of the current user.
5. **`SHELL`:** The path to the current shell.

### Example:

```bash
#!/usr/bin/bash

echo "My home directory is $HOME"
echo "My current working directory is $PWD"
echo "My username is $USER"
echo "My user ID is $UID"
echo "My shell is $SHELL"
```

### Output:

```
My home directory is /home/john
My current working directory is /home/john/scripts
My username is john
My user ID is 1001
My shell is /bin/bash
```

### Key Points:

- **Predefined:** System variables are predefined and automatically set by the shell.
- **Uppercase:** System variables are usually written in uppercase letters to distinguish them from user-defined variables.
- **Access:** You can access the values of system variables using the `$` symbol, just like user-defined variables.

Using system variables allows you to write more dynamic and flexible scripts that can adapt to different users and environments.

### System Variable Examples

```bash
#!/usr/bin/bash

echo $BASH
echo $BASH_VERSION
echo $HOME
echo $PWD
```

### Output:

```plaintext
/bin/bash
5.0.17(1)-release
/home/john
/home/john/scripts
```

In this example, we are using system variables to display information about the shell environment.

## Taking Input from the User

You can use the `read` command to take input from the user and store it in a variable.

### Example:

```bash
#!/usr/bin/bash

echo "Enter your name:"
read name
echo "Welcome $name"
```

### Output:

```
Enter your name:
John
Welcome John
```

### Key Points:

- **`read` Command:** The `read` command reads a line of input from the user and stores it in the specified variable.
  - Example: `read name` stores the user input in the variable `name`.
- **Prompting for Input:** Use `echo` to prompt the user for input.
  - Example: `echo "Enter your name:"`
- **Using the Input:** You can use the input stored in the variable just like any other variable.
  - Example: `echo "Welcome $name"`

Taking input from the user allows your script to interact dynamically with the user, making it more versatile and user-friendly.

### Practical Exercise

1. Create a Bash script named `userinfo.sh`.
2. Prompt the user to enter their name, age, and favorite color.
3. Store the inputs in variables.
4. Display the collected information back to the user in a formatted message.

```bash
#!/usr/bin/bash

echo "Enter your name:"
read name
echo "Enter your age:"
read age
echo "Enter your favorite color:"
read color

echo "Hello, $name! You are $age years old and your favorite color is $color."
```

### Output:

```
Enter your name:
John
Enter your age:
25
Enter your favorite color:
blue
Hello, John! You are 25 years old and your favorite color is blue.
```

This exercise helps reinforce the concept of taking input from the user and using it in your script.


Continueee