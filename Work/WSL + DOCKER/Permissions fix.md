Mostly project have already added fix for permissions problem, but if it still not working, you can fix it yourself:
To allow both your user of WSL **"<your_user>"** and **"www-data"** to write to the directory, you need to set proper ownership and permissions.

##### 1. Add Your User to the `www-data` Group
```bash
sudo usermod -aG www-data <your_user>
```
After running this, restart WSL or log out and back in:
```bash
exit
wsl
```
##### 2. Change directory permissions
Make `www-data` the group owner of the directory:
```bash
sudo chown -R <your_user>:www-data /path/to/your/project/folder
```
##### 3. Set permissions
Give both you and `www-data` write access:
```bash
sudo chmod -R 775 /path/to/your/project/folder
```
- `7` (Owner `<your_user>` has read, write, and execute)
- `7` (Group `www-data` has read, write, and execute)
- `5` (Others can read and execute, but not write)