# langdedect
multi-language online language detection tool


All commands must be done as root or sudo user.
Useful links:
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
http://flask.pocoo.org/docs/0.11/deploying/mod_wsgi/

1. First of all, it is recommended to upgrade your Ubuntu to actual state:
apt-get update
apt-get upgrade

2. Installing Apache, mod_wsgi, python3-dev and other dependencies
apt-get install apache2
apt-get install libapache2-mod-wsgi-py3 python3-dev
apt-get install wkhtmltopdf

3. Creating site structure (assuming we are in a home directory now and archive file with site's files is here too)
mkdir /var/www/web_langdetect
unzip -a <archive.zip> (if your distribution does not contain unzip, install it with "apt-get install unzip" as usually)
cp -R app /var/www/web_langdetect
cp app.wsgi /var/www/web_langdetect
cp web_langdetect.conf /etc/apache2/sites-available

4. Creating virtual environment
apt-get install python3-pip
pip3 install virtualenv
cd /var/www
virtualenv --no-site-packages web_langdetect

4.1 Installing packages to virtual environment
cd /var/www
mkdir nltk_data
cd web_langdetect
source bin/activate
pip3 install flask
pip3 install langdetect
pip3 install nltk
pip3 install lxml
pip3 install pdfkit

python3 (this will open python interpreter, write the following commands there)
import nltk
nltk.download()
This will open nltk download menu, which can be either with graphical interface or in console. 
First of all, you must check that nltk data directory is set to /var/www/nltk_data, if not, set it. Then download package named "punkt" and exit from interpreter with Ctrl-C or exit() call.

5. Configuring Apache. We have already copied web_langdetect.conf to apache sites directory on step 3.
If you have domain name, open that file (nano /etc/apache2/sites-available/web_langdetect.conf) and replace mywebsite.com with name needed. 
Then:
a2ensite web_langdetect
a2dissite 000_default
service apache2 restart

6. Open a browser and go to http://localhost, you should see main page
7. Upload directory permissions
cd /var/www/web_langdetect/app
chown -R www-data upload
chmow 744 upload

8. That is all, now files uploading should work. 
If anything goes wrong, call:
cat /var/log/apache2/error.log
to see latest error messages from Apache

