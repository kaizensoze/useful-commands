# useful-commands

```
ack
  ack -i <pattern>

ag (silver searcher; ack clone)
  ag -i <pattern>

apns
  apn push "<7db6d40a 79f083d0 f5d7ed4a 7392035b 022ef8ed 53d04b53 e998851b eddaea65>" -c ~/Downloads/dev.pem -m "Hello from the command line" -b 1 -s sosumi.aiff

avconv
  avconv -i input.mkv -codec copy output.mp4

awk
  sudo tail /var/log/nginx/access.log -n 200 | awk '/postVote/ {print $1}' | sort | uniq
  sudo tail /var/log/nginx/access.log -n 1000 | awk '/s~stellar-envoy-781/ {print $1}' | sort | uniq
  sudo tail -n 9000 /var/log/nginx/access.log | awk '/postVote/ {print $0}' | awk '!/stellar-envoy/ {print $0}';

aws
  autoscaling
    aws autoscaling update-auto-scaling-group --auto-scaling-group-name asg --min-size 1 --max-size 1 --desired-capacity 1
    aws autoscaling suspend-processes --auto-scaling-group-name asg
    aws autoscaling resume-processes --auto-scaling-group-name asg
  ec2
    aws ec2 describe-images
    aws ec2 describe-instances
    aws ec2 describe-key-pairs
    aws ec2 describe-regions
    aws ec2 describe-security-groups
    aws ec2 describe-subnets
    aws ec2 describe-volumes
    aws ec2 describe-vpcs
    aws ec2 reboot-instances
    aws ec2 run-instances
    aws ec2 start-instances
    aws ec2 stop-instances
    aws ec2 terminate-instances

    // creating ec2 instance
    A)
    groupId=`aws ec2 describe-security-groups | jq -r '.SecurityGroups[] | select(.GroupName == "launch-wizard-1") .GroupId'`
    aws ec2 run-instances --image-id ami-fce3c696 --count 1 --instance-type t2.micro --key-name joegallo --security-group-ids $groupId --subnet-id subnet-65546d3c --block-device-mappings "[{\"DeviceName\":\"/dev/sda1\",\"Ebs\":{\"VolumeSize\":20,\"VolumeType\":\"gp2\",\"DeleteOnTermination\":true}}]"
    unset groupId

    ec2Id=`aws ec2 describe-instances | jq -r '.Reservations[] | select (.Instances[].State.Name == "running") .Instances[].PublicIpAddress'`
    echo "ubuntu@$ec2Id"
    unset ec2Id

    B)
    aws ec2 create-security-group --group-name launch-wizard-2 --description "test" --vpc-id vpc-175cfb73
    aws ec2 run-instances --image-id ami-d05e75b8 --count 1 --instance-type t2.micro --key-name joegallo --security-group-ids sg-ae10c4c8 --subnet-id subnet-65546d3c --block-device-mappings "[{\"DeviceName\":\"/dev/sda1\",\"Ebs\":{\"VolumeSize\":20,\"VolumeType\":\"gp2\",\"DeleteOnTermination\":true}}]"
    aws ec2 create-tags --resources i-c110e862 --tags Key=Name,Value=php-test

    // in the event you fuck up ssh access
    aws ec2 stop-instances --instance-ids i-9a31e5f8
    aws ec2 describe-instances --instance-ids i-9a31e5f8
    aws ec2 detach-volume --volume-id vol-91b3defc
    aws ec2 attach-volume --volume-id vol-91b3defc --instance-id i-3785de80 --device /dev/sdf
    ssh scratch
    sudo -i
    mount /dev/xvdf /mnt
    umount /mnt
    aws ec2 describe-instances --instance-ids i-3785de80
    aws ec2 detach-volume --volume-id vol-91b3defc
    aws ec2 attach-volume --volume-id vol-91b3defc --instance-id i-9a31e5f8 --device /dev/sda1
    aws ec2 start-instances --instance-ids i-9a31e5f8
  s3
    aws s3 ls s3://nooklyn-pro/locations/ --recursive --profile nooklyn | vim -
    aws --profile hbs-stage s3 rm s3://hbs-stage/uploads/background_image/image/2/background2.JPG
    aws s3 cp "7th Avenue Donuts and Diner" 's3://storage.tastesavant.com/restaurants/7th Avenue Donuts and Diner' --recursive

babel
  npm install -g babel-cli
  babel-node <script>

bc
  echo 'ibase=2;obase=A;10000001' | bc
  echo 'ibase=16;obase=A;FF' | bc

brew
  brew list
  brew search elasticsearch
  brew info elasticsearch
  brew switch elasticsearch 1.7.3
  brew services list
  brew deps --tree --installed
  brew tap
  brew cleanup
  brew update && brew upgrade && brew cleanup && brew prune

brew cask
  brew cask list
  brew cask cleanup

carthage
  carthage update
  carthage bootstrap

cat
  cat /etc/resolv.conf

cordova
  cordova create hello com.example.hello "HelloWorld"
  cd hello
  cordova platform add ios
  cordova build
  cordova plugin add plugin.google.maps --variable API_KEY_FOR_IOS="AIzaSyCSi7Zh5unqv-Vt83BOgvOdfvPHEGANi_g"
  cordova emulate

cp
  cp dirA/blah/* blah  # copy contents of dirA/blah into blah folder

curl
  cat urls.txt | parallel -j30 --eta curl -I
  curl --user name:password http://www.example.com
  curl --data "param1=value1&param2=value2" http://example.com/resource.cgi
  curl -X GET -H 'Authorization: Token token="<token>"' http://0.0.0.0:3000/api/v1/help-now > blah.html && chrome blah.html
  curl -X GET -H "Authorization: token <token>" "https://api.github.com/user/emails"
  curl -X POST -H "Authorization: token <token>" -d '["test1@test.com", "test2@test.com"]' https://api.github.com/user/emails
  curl -X DELETE -H "Authorization: token <token>" -d '["test1@test.com", "test2@test.com"]' https://api.github.com/user/emails
  curl -v <url>

dd
  dd if=/dev/disk2 | strings > recover.txt

diskutil
  diskutil secureErase freespace 0 /Volumes/sparta

dns
  sudo dscacheutil -flushcache && sudo killall -HUP mDNSResponder

ffmpeg
  for x (*.flac); do ffmpeg -i $x -ab 196k -ac 2 -ar 48000 "${x:r}.mp3"; done

gem
  gem list
  gem search cocoapods
  gem install cocoapods [-v 0.25.0]   # optionally specify version
  gem list cocoapods --remote --all   # show all available versions

ghostscript
  gs \
 -sOutputFile=output.pdf \
 -sDEVICE=pdfwrite \
 -sColorConversionStrategy=Gray \
 -dProcessColorModel=/DeviceGray \
 -dCompatibilityLevel=1.4 \
 -dNOPAUSE \
 -dBATCH \
 records.pdf

git
  git grep 'addSection' $(git rev-list --all)
  git bisect start
  git bisect bad
  git bisect good <hash>
  git bisect reset
  git push --follow-tags
  git push origin :refs/tags/v1.2 (for updating tag)
  git tag -f -a v1.2 -m '1.2 release'
  git push --follow-tags
  easier way of pulling down pull-requests from an upstream
    [remote "upstream"]
      url = https://github.com/mikedeboer/node-github.git
      fetch = +refs/heads/*:refs/remotes/upstream/*
      fetch = +refs/pull/*/head:refs/remotes/upstream/pr/*
  git merge and then git checkout --ours or --theirs before adding file(s) in conflict
  git show master:Expert/CallsViewController.swift
  git log --since="12am"
  git co origin/master Expert/Info.plist
  git d master -- Expert/Info.plist
  git checkout -p
  git diff -R (reverse additions/deletions for diff [between branches])
  git dw master -- . ':!test' ':!package.json' ':!examples' ':!doc' ':!CHANGELOG.md'  # exclude files from diff
  git branch --track fork-master kaizensoze/master  # having multiple local master branches from different remotes
  git diff [-R] > patch-name.diff
  git apply pack-name.diff
  git log -L 444,+10:lib/routes.json  # get history of range of lines in file
  git submodule update --init (update submodules for already-cloned repo)

github
  curl -u kaizensoze:<token> https://api.github.com/rate_limit

gpg
  gpg --gen-key
  gpg --list-keys
  gpg --list-secret-keys
  gpg --list-sigs
  gpg --delete-secret-key <id>
  gpg --delete-key <id>
  gpg --import pgp.asc
  gpg --fingerprint <id>
  gpg --verify FILE.sig
  gpg --verify FILE.asc FILE
  gpg --output doc.gpg --encrypt --recipient blake@cyb.org doc  # encrypt
  gpg --output FILE --decrypt FILE.gpg                          # decrypt
  gpg --clearsign doc                                           # sign text [but don't compress]

hash
  hash      # list bindings
  hash -r   # reset bindings

hdiutil
  hdiutil attach -imagekey diskimage-class=CRawDiskImage image-file-name

heroku
  heroku pg:psql
  heroku pg:backups
  heroku pg:backups public-url a399
  heroku pg:backups capture

  heroku pg:info
  heroku pg:reset DATABASE_URL
  heroku pg:reset HEROKU_POSTGRESQL_PINK_URL --confirm hbs-stage && heroku run rake db:migrate db:seed

  heroku logs -t

  heroku run rake db:migrate
  heroku run rails console
  heroku run rails console -a hbs-stage

  heroku config

  heroku releases

  heroku run rake db:migrate -a nooklyn-dev-pr-1075
  git push heroku master && heroku run rake db:migrate && heroku restart

httpie
  http httpie.org
  http httpie.org --style autumn (other decent options: native, solarized)
  http -f POST example.org this="A" that="B"
  http -f POST example.org/upload file@test.pdf
  http -f POST 67.205.189.70/upload mission=@test.json
  http -v -f POST http://127.0.0.1:8080/setReportInstanceSubject reportInstanceId="800" subject="TESTREPORT"
  http httpie.org 'Authorization: token TOKEN'
  http -f DELETE 67.205.189.70/missions/59d3b92e7d4fc28ad34372ed Authorization:AeroboDeleteAuthorization961

imagemagick
  identify -verbose [image]
  exiftool -all= [image]
  convert *.jpg deposit_and_receipt.pdf
  convert -strip -quality 85% -colorspace gray -resize 1224x1632 a.jpg b.jpg '2015 personal return.pdf'

iptables
  sudo vim /etc/iptables.firewall.rules
  sudo iptables-restore < /etc/iptables.firewall.rules
  sudo vim /etc/network/if-pre-up.d/firewall
  #!/bin/sh
  /sbin/iptables-restore < /etc/iptables.firewall.rules
  sudo chmod +x /etc/network/if-pre-up.d/firewall

jq
  jq -r '.SecurityGroups[] | select(.Description == "default group") | .GroupId
  cat data.json | jq -r '.[] | .host'
  curl -s --user "kaizensoze:XXXXX" "https://api.github.com/user/repos?per_page=100" | jq -r '.[] | select(.fork == false) | "\(.full_name)\t\(.clone_url)"' | sort | column -t

laravel
  php artisan init
  php artisan serve

  php artisan migrate:refresh && php artisan init
  php artisan serve

launchctl
  sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.atrun.plist
  sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.atrun.plist

letsencrypt
  sudo ./certbot-auto renew
  ./letsencrypt-auto certonly -d brassafrax.com -d www.brassafrax.com -d alpha.brassafrax.com

lsof
  sudo lsof -i :17600
  sudo lsof -i tcp:17600
  sudo lsof -n -P -i +c 0              # `+c 0` means show all characters for process name
  sudo lsof -i -n -P | grep -i LISTEN  # all listening traffic (list of used ports)

mac
  mac update
  mac lock
  mac apps:app-store
  mac info
  mac speedtest
  mac ports   # seems to be the same as `sudo lsof -i -n -P | grep -i LISTEN`
  mac dev:monitor system.log
  mac system
  mac temp
  mac memory
  mac git:open # open current repository on github

macaddress
  echo $(echo 'current mac address:') $(macaddr)
  sudo /System/Library/PrivateFrameworks/Apple80211.framework/Resources/airport -z
  sudo ifconfig en0 ether $1
  networksetup -detectnewhardware

mail
  echo "This is a test." | mail -s Testing gallo.j@gmail.com

microsoft word (not excel)
  alt+F9: show formulas
  F9: update formulas
  Layout > Data > Formula: edit formula

mongodb
  sudo systemctl start mongod
  sudo systemctl status mongod
  mongo -u aerobo -p --authenticationDatabase admin
  mongo karass
  show collections
  db.users.find({})
  db.users.find({}, {username: 1, _id: 0})
  db.users.remove({})

netcat
  nc -vz <ip> <port>  # check if port [on given host/ip] is open

netstat (netstat sucks in osx)
  sudo netstat -an | grep 443             # use lsof instead
  sudo netstat -atp tcp | grep -i LISTEN  # use lsof instead

nginx
  sudo service nginx stop
  sudo service nginx start
  sudo nginx -t
  sudo nginx -s reload

ngrep
  sudo ngrep -d any metafilter  # grep anything related to metafilter traffic (go to http://metafilter.com in browser)
  sudo ngrep special_id         # filter for special_id in any network traffic

nmap
  nmap -sP 192.168.0.1/24

npm
  npm ls [--global] --depth=0
  npm show github@* version   # list all versions of github package
  npm publish --tag <old-version>  # publish older version

nvm
  nvm ls
  nvm install node (latest)
  nvm install <version>
  nvm use <version>

opensnoop
  sudo opensnoop -p PID

osx
  sudo killall -HUP mDNSResponder
  sysctl -a hw
  cmd+alt+i to get size of selected files
  split view: hold click on green maximize icon of window
  sudo purge

pdf
  brew install poppler
  pdfunite in1.pdf in2.pdf out.pdf

photoshop


php
  brew install homebrew/php/php70
  brew install homebrew/php/php70-pdo-pgsql
  add export PATH="$(brew --prefix homebrew/php/php70)/bin:$PATH" to ~/.zshrc
  php -i # php info from the command line

pip
  pip install --user (already as alias in zshrc)
  pip freeze --local | grep -v '^\-e' | cut -d = -f 1 | xargs pip install -U   (upgrades all installed)

pipe
  ls ~ > f
  ls ~ > f 2> e
  ls ~ > f 2>&1

pm2
  pm2 start ~/augnav/index.js -n "augnav" --watch --ignore-watch="uploads"
  pm2 list
  pm2 stop all
  pm2 restart all
  pm2 reload all
  pm2 delete all
  pm2 startup
  pm2 unstartup systemd
  pm2 logs

postgres
  \l # list databases
  \c <db> # connect to database
  \d # list tables
  \d <table> # describe table
  \q # quit

  create database nooklyn_test with template nooklyn owner joegallo;
  select table_name from information_schema.columns where column_name like 'mate_post_id';

  pg_restore --verbose --clean --no-acl --no-owner -h localhost -U joegallo -d nooklyn ~/Downloads/latest.dump  # import dump into existing database
  pg_dump -U joegallo nooklyn -f ~/Downloads/nooklyn.sql
  pg_restore -d newdb db.dump
  psql nooklyn < ~/Downloads/nooklyn.sql

  pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start  # start
  pg_ctl -D /usr/local/var/postgres stop -s -m fast  # stop

  use case statement for switch for column
  use <datetime>::text like ''
  select generate_series('2012-10-01', '2012-10-31', '1 day'::interval) as ts  # get every day in a given month
  select extract(month from b.starttime)
  select extract(epoch from (timestamp_B - timestamp_A))
  trunc(63.4567, 2) -> 63.45
  row_number() over(order by m.joindate) as row_number
  rank() over(order by sum(b.slots) desc) as rank

ps
  ps aux
  ps -p <pid>

pyenv
  pyenv install -l  # available versions
  pyenv install <version>
  env PYTHON_CONFIGURE_OPTS="--enable-framework" pyenv install 3.5.1  # for pyinstaller requiring the library (.dylib)
  pyenv global <version> <version> ...
  pyenv local <version>
  pyenv local --unset
  pyenv shell <version>
  pyenv shell --unset

rails
  bundle install && rake db:migrate && PORT=3000 foreman start

  be rake db:migrate VERSION=0 && be rake db:migrate
  be rake db:drop db:create db:migrate db:seed

  puts BackgroundImage.all.select("sort_order").order("sort_order").map(&:sort_order).join("\n")
  Location.joins(:neighborhood).where(neighborhoods: { region_id: [value] })

  User.find(9999).destroy

  rake test [testpath]
  be rspec [testpath]

  y MatePost.find(2409)  (in rails console)

redhat
  cat /etc/redhat-releas  # check version

rsync
  rsync -avzhe ssh --progress ubuntu@54.175.82.101:~/son ~/Downloads/son

ruby-install
  ruby-install --latest

scp
  scp foobar.txt your_username@remotehost.edu:/some/remote/directory

screen
  rename window:  C-a A
  reorder window: C-a :number n

siege
  siege -c200 -b -t3M -i -f urls.txt
  siege -c200 -b -t3M 162.243.187.168:8080/hello

smc
  ./smc -l | grep 'F0ID'

solr
  cd /usr/local/Cellar/solr/<version>/libexec/example/ && java -jar start.jar &

sort
  sort -u (unique)
  sort -f (ignore case)

sqlite
  .tables
  .schema <table>
  .quit
  PRAGMA table_info(table_name);
  attach database '/home/duckspeaker/gitpop/backup.db' as backup;
  insert into main.ignores (user_id, id) select 1, id from backup.ignores;

ssh
  ssh -fN -L 3307:127.0.0.1:3306 web@caribou.tastesavant.com

ssh-agent
  eval "$(ssh-agent -s)"
  ssh-add -l
  ssh-add -L
  ssh-add <private_key>

ssh-keygen
  ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

strace
  (prefix command with strace; e.g. strace python blah.py)

supervisor
  supervisord -c <config>
  supervisorctl -c <config> reload
  supervisorctl -c <config> restart gunicorn

systemctl
  systemctl start nginx
  systemctl stop nginx
  systemctl try-restart nginx
  systemctl reload nginx
  systemctl reload-or-restart nginx
  systemctl status nginx
  systemctl enable nginx
  systemctl disable nginx

tcpdump
  sudo tcpdump -nS
  sudo tcpdump -nnvvS
  sudo tcpdump -nnvvXS
  sudo tcpdump -nnvvXSs 0
  sudo tcpdump -nn -tttt -XX -s 0 -l -i any | tee output.txt
  sudo tcpdump -nn -tttt -XX -s 0 -l -i any src 192.168.0.100 | tee output.txt

transmission
  sudo apt-get update && sudo apt-get install -y transmission-cli && transmission-cli <url>

tshark
  sudo ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport /usr/local/bin/airport
  tshark -I -i en0 -Y http -T fields -e http.host -e ip.src
  tshark -I -i en0 -Y 'http and not udp'
  tshark -I -i en0 -Y http.request -T fields -e http.host

tugboat
  tugboat authorize
  tugboat keys
  vim ~/.tugboat
  tugboat droplets
  tugboat create
  tugboat info <droplet>
  tugboat ssh <droplet>
  tugboat restart <droplet>
  tugboat destroy <droplet>

ufw
  sudo ufw app list
  sudo ufw allow OpenSSH
  sudo ufw enable
  sudo ufw status

vim
  :%s/\r/\r/g
  Ctrl-]      # follow link
  Ctrl-T      # go to previous topic

virtualenv
  mkvirtualenv blah
  workon blah
  rmvirtualenv blah

wireshark
  DON'T FORGET: https://ask.wireshark.org/questions/42897/how-do-i-turn-on-monitor-mode-in-mac-os-x-with-wireshark-v199
  http
  ip.src == 192.168.1.109 || ip.dst == 192.168.1.109

xcode
  po thing.recursiveDescription
  xcode-select -p # get path of xcode cli

xcrun
  xcrun simctl list # simulator

xmllint
  xmllint --format out.xml

zsh
  print -l ${(ok)functions}  # list functions

xxd
  echo '3be0' | xxd -r -p | xxd -b   # hex to binary
```
