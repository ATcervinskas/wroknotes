Git should be installed on Windows and **on every** Linux distribution within WSL.
Before configuration - update git and git extension on Windows
## Git configuration

#### 1. Check if Git is installed, if not:
```bash
sudo apt update && sudo apt install git
```
#### 2. Set up your name
```bash
git config --global user.name "Your Name"
```
#### 3. Set up your email
```bash
git config --global user.email "youremail@domain.com"
```
#### 4. Set up GCM (git credential manager) for use with WSL distribution
##### 1. If GIT installed is >= v2.39.0
```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"
```
##### 2. else if GIT installed is >= v2.36.1
```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```
##### 3. else if version is < v2.36.1 enter this command:
```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager-core.exe"
```
## GitLab certificate
#### 1. Get the SSL Certificate
```bash 
echo | openssl s_client -showcerts -servername gitlab.vgtu.lt -connect gitlab.vgtu.lt:443 2>/dev/null | openssl x509 -outform PEM > gitlab.crt
```
#### 2. Add the Certificate to Your System
```bash
sudo mv gitlab.crt /usr/local/share/ca-certificates/
```

```bash
sudo chmod 644 /usr/local/share/ca-certificates/gitlab.crt
```

```bash
sudo update-ca-certificates
```
#### 3. Configure Git to Use the Certificate
```bash
git config --global http.sslCAinfo /etc/ssl/certs/gitlab.pem
```
#### 4. Test the Connection
Try cloning or pulling

## How to access git repo with Windows program.

#### 1 option
To Windows Git config add every project as safe directory:
```bash
git config --global --add safe.directory /path/to/project/repository
```
#### 2 option
Open `\\wsl$` > Right click on Ubuntu folder icon > Select Map network drive (Z:) > Now Z: can be used as other partition/drive.  Specify repository path from disk  Z:/home/..../www

![[Pasted image 20250211115304.png]]

## Permission denied while pulling 

![[Pasted image 20250211115703.png]]

```bash
sudo chown -R YOUR_USERNAME:YOUR_USERNAME home/YOUR_USERNAME/www
```

## FATAL: LF would be replaced by CRLF 

![[Pasted image 20250211121451.png]]

```bash
git config --global core.safecrlf false
```

