# Upgrading OnlyOffice. 

!!! note Before Upgrading
  Ensure you have a copy of your nginx conf incase this gets overwritten. 
  I strongly suggest using the debian route as to minimise damage. 


## Important commands. 

``` bash
sudo apt-get clean 
sudo apt-get autoclean 
sudo apt-get update
sudo apt upgrade
```
If prompted, N (default) for mysql database. 

### notifier issues
```bash
sudo rm /var/lib/dpkg/info/update-notifier-common.*
sudo apt-get install --reinstall update-notifier-common
```
and restart the installation 
```bash
df -h
sudo apt-get install -f
sudo dpkg --configure -a
```
