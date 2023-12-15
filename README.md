# aws-linux-troubleshooting

## How to combine two files line by line in linux bash script
* `paste file1.txt file2.txt > fileresults.txt`

## Add prefix and suffix to every line in a file bash script

* `awk '{ print "Prefix TXT", $0, "Suffix TXT" }' file.txt`

## PHP output showing little black diamonds with a question mark
php.ini and add:

* `default_charset = "ISO-8859-1"`
## Docker overlays disk space 
* `docker system prune -a -f`

## Install MYSQL connector ubuntu 16.04 for tableau server
* `https://www.tableau.com/support/drivers?_ga=2.162700796.1335875126.1612552598-743759460.1612552598&_fsi=Dr99RyGZ`

### Error remove long listed files /bin/rm: Argument list too long
* `find . -name "*.pdf" -print0 | xargs -0 rm`

### Error git Jenkins unable to connect to github private repo
* Try to connect to github private repo in Jenkins 
* issue: Its sending error `failed to connect to repository : error performing git command: git ls-remote -h https://github.com/`
* Solution: As in I have installed Jenkins on Amazon Linux make sure you install git on it , `yum install git -y`
* reference: https://stackoverflow.com/questions/76536785/jenkins-unable-to-connect-to-github-private-repo
