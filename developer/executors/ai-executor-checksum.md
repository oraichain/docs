## AI Executor program checksum tutorial

File integrity is one of the most important factors when it comes to security check. One can inject a malicious program which reads & collects all sensitive data of your computer.

As a result, the executors need to verify the integrity of the executable you are about to run. To do so, you need to compare the checksum of your program against the appropriate checksums shown here, which are provided by the Oraichain team. If they don't match, you must not run that program. Otherwise, you would risk losing your sensitive data, especially your wallet.

Below is the list of checksums for Linux, Macos, and Windows:

```
Windows: 364650bcd34a89378264acd243eb295b
MacOS: 002a6c17fab4f92eb20ec553b47c5254
Linux: 6da5f0d17c3072ff8e62dd13188fb576
```

These checksums will be updated accordingly to the latest executable's version. As of now, the current version of the executable is v0.3.4.3.

Please follow the step below depending on the type of machine you are using to generate your local checksum:

***Windows & MacOS:***

Please use [this link](http://emn178.github.io/online-tools/md5_checksum.html) and upload the file. It will display its checksum. Then, please compare the checksum with the appropriate checkum above. They should match.

***Linux:***

For Linux users, you can use the following command to generate the file's checksum (assuming the executable's name is **aioracle-executor-process**):

```bash
md5sum aioracle-executor-process | cut -d ' ' -f 1
```

The checksum should match the Linux checksum above.