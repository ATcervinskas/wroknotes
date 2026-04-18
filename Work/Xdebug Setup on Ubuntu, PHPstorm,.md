#### 1. Update Package Lists

```bash
sudo apt update
```
#### 2. Check PHP Version

```bash 
php -v
```
#### 3. Install Xdebug

```bash
sudo apt-get install php8.1-xdebug
```
#### 4. Configure Xdebug

Edit the Xdebug configuration file:
``` bash
sudo nano /etc/php/8.1/mods-available/xdebug.ini
```

Add the following configuration:
```json
[xdebug]
zend_extension=xdebug.so
xdebug.mode=debug
xdebug.client_host=10.0.2.2
xdebug.client_port=9003
xdebug.log=/var/log/xdebug.log
xdebug.idekey=phpstorm
xdebug.start_with_request=yes
xdebug.discover_client_host=true
```
#### 5. Set Up Log File

Create the log file and set the correct permissions:
```bash
sudo touch /var/log/xdebug.log

sudo chown www-data:www-data /var/log/xdebug.log

sudo chmod 644 /var/log/xdebug.log
```
#### 6. Restart Apache

```bash
sudo systemctl restart apache2
```

### Install Xdebug Chrome Extension

- Install the extension from the [Chrome Web Store](https://chromewebstore.google.com/detail/xdebug-chrome-extension/oiofkammbajfehgpleginfomeppgnglk).
- After installation, open the extension's settings.
- Set the IDE key to `phpstorm` to match the key specified in your `xdebug.ini` configuration

### PhpStorm Configuration
---
#### 1. Configure CLI Interpreter

- Open PhpStorm settings: `Ctrl+Alt+S`.
- Go to `PHP` -> `CLI Interpreter`.
- Specify the path to your Vagrant instance.


![[Pasted image 20240905123207.png]]

![[Pasted image 20240905123339.png]]

![[Pasted image 20240905123415.png]]

![[Pasted image 20240905123523.png]]
#### 2. Configure Debug

- Open PhpStorm settings: `Ctrl+Alt+S`.
- Go to `PHP` -> `Debug`.
- Ensure the Xdebug port is set to `9003`.
- Check the checkbox for `Can accept external connections`.
![[Pasted image 20240905134952.png]]
#### 3. Configure Servers

-  Open PhpStorm settings: `Ctrl+Alt+S`.
-  Go to `PHP` -> `Servers`.
-  Add your project host and specify the path on the server.

![[Pasted image 20240905124715.png]]

#### 4. Add Debug Configuration

- Go to `Run` -> `Edit Configurations`.
-  Add a new `PHP Remote Debug` configuration.
-  Select `Filter debug connection by IDE key`.
-  Choose the server added previously.
-  Set the IDE key to `phpstorm` (as configured in `xdebug.ini`).

![[Pasted image 20240905135131.png]]