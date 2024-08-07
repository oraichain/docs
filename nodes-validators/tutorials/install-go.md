# Install go lang

### 1. Download Go

First, you need to download the Go 1.21 tarball. Open a terminal and use the following command to download it. Please replace the download URL with the correct one if the version or the link has changed. You can choice another version url from go.dev

```bash
wget https://go.dev/dl/go1.21.10.linux-amd64.tar.gz
```

### 2. Extract the Tarball

Extract the downloaded tarball to /usr/local to install Go. This location is recommended as it is the default and will make version management easier. Use the following command:

```bash
sudo tar -C /usr/local -xzf go1.21.10.linux-amd64.tar.gz
rm go1.21.10.linux-amd64.tar.gz
```

### 3. Set Go Environment Variables

You need to set the PATH environment variable to include the Go binary directory. You can do this by adding the following lines to your ~/.profile or ~/.bashrc file for a single user or /etc/profile for system-wide installation.

```bash
echo "export GOPATH=$HOME/go" >> ~/.profile
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.profile
source ~/.profile
```

### 4. Verify the Installation

To ensure that Go is installed correctly, use the go version command which should output the version of Go that was installed:

```bash
go version
```

If everything was set up correctly, you should see go version go1.21.10 linux/amd64 (or similar, depending on your architecture) in the terminal output.
