# AI Executor program installation and execution guideline (Docker version)

## Installation

### 0. Hardware specification

A dedicated machine that can keep the program running continuously. The program supports Linux, Windows & MacOS.

Minimum requirement:

```
2vCPUs
2GB RAM
```

### 1. Docker & docker-compose

With docker, the AI Executor program can run on any platforms. As a result, it is a must to install and download Docker. Docker-compose is a tool for defining & running multi-container Docker applications. It is convenient to use docker-compose to work with Docker containers. You can follow the following links: [Downloading & installing Docker](https://docs.docker.com/engine/install/#server), [Downloading & installing docker-compose](https://docs.docker.com/compose/install/).

Next, please create a file called: ***docker-compose.yml*** that has the following content:

```yml
version: '3.3'
services:
  ai_executor:
    container_name: ai_executor
    image: orai/ai-executor:0.0.1
    tty: true
    environment:
      - PIN=${PIN}
      - DOCKER=true
    restart: on-failure
    logging:
      driver: "local"
      options:
        max-size: "100m"
        max-file: "3"
    volumes:
      - ./:/workspace
    command: ./aioracle-executor-process
```

### 2. Download the executor zip file

***Shell (Mac, Linux):***
```bash
wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=180aYBeOlakKorDpHsaHImR1pFlHEGZ26' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=180aYBeOlakKorDpHsaHImR1pFlHEGZ26" -O executor.zip && rm /tmp/cookies.txt && unzip executor.zip
```

***Windows:***

With windows, you can download using the following link: ```https://drive.google.com/file/d/180aYBeOlakKorDpHsaHImR1pFlHEGZ26/view?usp=sharing```

### 3. Configure the .env file

The .env file in the zip configures the network, wallet, and other basic variables for your program to use. All the key-value pairs are heavily commented already. If you still have questions about them, freel free to ask us, the Oraichain team.

### 4. File checksum (optional but recommended)

Please follow [this guildeline](https://docs.orai.io/developer/executors/ai-executor-checksum) to verify your executable.

### 5. Start the container & program

***Shell (Mac, Linux):***

Type:

```bash
PIN=<your-pin-for-encrypted-mnemonic> docker-compose up -d
```

***Windows:***

with powershell:

```bash
$Env:PIN = "<your-pin-for-encrypted-mnemonic>" && docker-compose up -d
```

If you do not use an encrypted mnemonic, then you don't have to type in the PIN variable.

### 6. Monitoring the program

To view the program's log, please type the following: 

```
docker-compose logs -f --tail=100 ai_executor
```