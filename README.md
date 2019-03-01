Namecheap DDNS Update Docker container
======================================

The idea is to have a docker container to be able to run it into a RaspberryPi, updating your domain as soon as it detects the IP has changed. The update script is based on the one done by Kent Behrends (https://github.com/bci/Namecheap-DDNS-Update). I improved a couple tweks to make it fit better to my needs.

# Run

    sudo docker run -d --name ddns --restart unless-stopped -e DOMAIN="YOUR_DOMAIN" -e PASSWORD="THE_PASSWORD" -e HOSTS="'HOST_NAME'" eidansoft/namecheap-ddns-update

# Full list of variables to configure the behaviour:

* DOMAIN string - name of domain to update. Example: lorente.info
* PASSWORD string - the password given from NameCheap.com --> My Account --> Manage Domains --> Modify Domain --> Dynamic DNS. Example: Eu32UEuoAOEUeuiAIPIeui33i34i3ED33nth
* HOSTS string - the array of hosts to update, in order to pass multiple values here, the hosts must be quoted with single quoutes (') and separate by a space. Example with a single host: -e HOSTS="'testhost1'" Example with two hosts: -e HOSTS="'host' 'hosh2'"
* EMAIL string - if exists, then an email will be generated on error or change of IP address and send to that address. Example: -e EMAIL="testyour@mail.es"
* SECONDS integer - the number of seconds between updates. If not provided, the default value is 600 seconds (10 minutes). Example: -e SECONDS=300

# Stop the container with the DDNS service

    sudo docker rm -f ddns

# Log

The log can be found with the command:

    sudo docker logs ddns

If you want save the log files, the container save it by default in /mnt, so you can just include a volume mounted there when run the container. This way the logs will be saved there. Example:

    -v /home/pi/my-logs:/mnt
