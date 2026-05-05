### Go to Redmin's root folder

```bash
cd /opt/redmine
```
### Copy and unarchive plugin 

```bash
cp /home/webmaster/plugin.zip /opt/redmine/plugins
cd plugins
unzip plugin.zip
```
### Install required gems

```bash
cd /opt/redmine
bundle install --without development test --no-deployment
```

### Migrate plugin's tables

```bash
bundle exec rake redmine:plugins RAILS_ENV=production
```

### Restart Redmine

```bash
sudo systemctl restart apache2
```

#redmine #plugins #installation
