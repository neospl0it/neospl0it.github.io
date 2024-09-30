---
title: "How to Hide Secrets Inside JPG Images Using a Python"
description: "Inject secret messages and files into JPG images and extract them using Python"
date: 2024-09-23  
categories: [Cybersecurity, Steganography]  
tags: [JPG, hex-editor, python, steganography, secrets]  
image:  
  path: thbnls/steg.png
---

## How to Hide Secrets Inside JPG Imgaes

### JPG File Structure and Steganography

JPG files are composed of a sequence of binary data that begins with 4 bytes of specific marker (`\FF\D8\FF\E0`) and ends with 2 bytes of another marker (`\FF\D9`). After the closing marker (`\FF\D9`), any additional data is ignored by image viewers, making it an excellent spot to hide extra information.

Using Python’s file-handling capabilities, we can read, write, and manipulate this binary data to inject and retrieve secret messages or files, without changing the visible properties of the image.

### How to Hide Secrets Inside JPG Images Using a Hex Editor and Python

A simple way to hide data inside JPG images using a hex editor and Python. This technique leverages the JPG file structure to store hidden messages or even entire files without affecting the appearance of the image. We'll also provide the code to inject and extract hidden content.

1. **Hex Editor** (we will use `ghex`)
2. **Python** (with file-handling functionality)

### Step 1: Install `ghex` (Hex Editor)

We need to first install the `ghex` hex editor to examine and manipulate the binary structure of a JPG file.

Run the following command on Linux:

```bash
apt install ghex
```

Once installed, open a JPG file with `ghex`:

```bash
ghex image.jpg
```
![ghex](bimgs/ghex-steg.png)

Any content added after the `\FF\D9` marker will be ignored when displaying the image. However, it can still be extracted using a programmatic approach, such as Python.

### Step 2: Injecting a Secret Message into the JPG Using Python

Here's how you can inject a secret message into a JPG file:

```python
# Injecting a secret message into a JPG file
with open("image.jpg", "ab") as f:
    # Write the secret message to the end of the image file
    f.write(b"my secret message")
```

This will append the message `"my secret message"` after the `\FF\D9` marker in the `image.jpg` file.

### Step 3: Extracting the Hidden Message from the JPG

To extract the hidden message from the JPG, we need to read the file, locate the `\FF\D9` marker, and extract the content following it.

```python
# Extracting the hidden message from the JPG
with open("image.jpg", "rb") as f:
    content = f.read()
    offset = content.index(bytes.fromhex("FFD9"))  # Find the end of the JPG image
    f.seek(offset + 2)  # Move the pointer just after \FF\D9
    print(f.read())  # Print the hidden message
```

This Python code reads the JPG file, identifies the location of `\FF\D9`, and then extracts and prints everything after that marker.

### Step 4: Hiding an Executable or Any File Inside the JPG

In addition to hiding text messages, you can also hide files (e.g., an executable, `program.exe`) inside the JPG file using a similar method.

#### Injecting a File into the JPG

```python
# Injecting an executable (or any file) into a JPG file
with open("image.jpg", "ab") as f1:
    with open("program.exe", "rb") as f2:
        # Append the contents of the executable to the image
        f1.write(f2.read())
```

This code will append the binary data of `program.exe` after the `\FF\D9` marker in the image.

#### Extracting the Hidden File from the JPG

To extract the hidden file, you can read the data after the `\FF\D9` marker and write it to a new file:

```python
# Extracting the hidden file from the JPG
with open("image.jpg", "rb") as f1:
    content = f1.read()
    offset = content.index(bytes.fromhex("FFD9"))  # Find the end of the JPG image
    f1.seek(offset + 2)  # Move the pointer just after \FF\D9

    # Write the extracted file content to a new file
    with open("program_extracted.exe", "wb") as f2:
        f2.write(f1.read())
```

This code will extract the hidden file from the JPG and write it to a new file called `program_extracted.exe`.


### Why Use This Technique?

- **Simple and Effective:** No complex steganography algorithms are required.
- **Stealthy:** Hidden data does not affect the image display.
- **Cross-Platform:** Can be implemented using basic tools available on all platforms.
  
This easy method of encrypting data and messages within JPG pictures might be a fun way to get started with steganography. You can simply inject and retrieve secret data by using Python and a hex editor such as `ghex` . This technique shows how to cleverly change files, however it is not very safe for sensitive information (because anybody with knowledge of the file structure may simply retrieve the hidden data).
