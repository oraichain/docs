# AI Executor program installation and execution guideline

## Installation

### 0. Hardware specification

A dedicated machine that can keep the program running continuously. The program supports Linux, Windows & MacOS.

Minimum requirement:

```
2vCPUs
2GB RAM
```

### 1. Deno

[Deno](https://deno.land/) is a JavaScript & TypeScript runtime, which provides a secured environment for third parties to run scripts safely. Oraichain leverages this amazing feature and integrates Deno into the AI Executor program, where it downloads & runs deno scripts from the data source, test case, & oracle script providers. 

Deno protects the AI executors from malicious provider scripts that attempt to hack into their host machines and only enables the host network access to execute the scripts.

To install Deno, please follow the official [Deno's documentation](https://deno.land/#installation).

After finishing the Deno's installation process, you should be able to try running their simple program and receive the following result: ```Welcome to Deno!```

Then you can move on to the next step.

### 2. Download the program

***Shell (Mac, Linux):***

```bash
wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=180aYBeOlakKorDpHsaHImR1pFlHEGZ26' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=180aYBeOlakKorDpHsaHImR1pFlHEGZ26" -O executor.zip && rm /tmp/cookies.txt && unzip executor.zip
```

You would probably need to install ***unzip*** afterward if your dedicated host does not have it.

***Windows:***

For Windows users, you can download the zip and unzip it directly.

### 3. Configure the env file

The .env file in the zip configures the network, wallet, and other basic variables for your program to use. All the key-value pairs are heavily commented already. If you still have questions about them, freel free to ask us, the Oraichain team.

### 4. Run the program

In the zip contains the ```aioracle-executor-process-*``` programs, please choose one that matches your OS and run it.

If you see the following logs, then the program has run successfully:

```
Oraichain AI Executor program, v0.3.4
```

**For Linux and MacOS users:**

```
./aioracle-executor-process
```

**For Windows users:**

You can click on the exe file to start running!
