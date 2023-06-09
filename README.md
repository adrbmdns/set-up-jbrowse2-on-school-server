# How to set up JBrowse Web on a school server (group Linux machine)

This tutorial teaches how to set up JBrowse2 Web application on an intranet server. You need administrator rights for this. 

## 1. Install Node.js LTS on Ubuntu 

* [Installation instructions](https://github.com/nodesource/distributions#debinstall)

```sh
# Install the LTS version 
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```

* Test installation using 

```sh
node -v
npm version 
```

## 2. Install Samtools 

```sh
sudo apt update
sudo apt install samtools
samtools --version
```

## 3. Install tabix 

```sh
sudo apt install tabix
tabix --version
```

## 4. Install genometools

```sh
sudo apt install genometools
genometools --version 
```

## 5. Install JBrowse CLI 

```sh
sudo npm install -g @jbrowse/cli
jbrowse --version
```

## 6. Download and unzip [JBrowse2 Web](https://jbrowse.org/jb2/download/)

```sh
cd ~
wget https://github.com/GMOD/jbrowse-components/releases/download/v2.5.0/jbrowse-web-v2.5.0.zip
mkdir jbrowse2
unzip jbrowse-web-v2.5.0.zip -d jbrowse2/
```

## 7. Install Apache2 

```sh
sudo apt install apache2
```

After installation, Apache2 should start automatically. You can verify its status by runnning:

```sh
sudo systemctl status apache2
```

## 8. Put the Jbrowse2 Web folder into the folder `/var/www/html/`

```sh
sudo cp -r ~/jbrowse2 /var/www/html
```

## 9. Get the IP address of your server 

First, we need to remove the `index.html` file in the folder `/var/www/html/`:

```sh
sudo rm /var/www/html/index.html
```

Then, get the IP address of your server:

```sh
hostname -I 
```

Then, in the browser go to the address `http://your_ip_address`. You should see a Welcome page. 

Finally, you can read this [tutorial](https://jbrowse.org/jb2/docs/quickstart_web/#adding-tracks) for adding tracks and etc.

## 10. Example code for adding a track 

Here, we use the [Arabidopsis genome](https://www.arabidopsis.org/download/index-auto.jsp?dir=%2Fdownload_files%2FGenes%2FTAIR10_genome_release%2FTAIR10_chromosome_files) as an example.

```sh
mkdir ~/genome
cd ~/genome
wget wget https://www.arabidopsis.org/download_files/Genes/TAIR10_genome_release/TAIR10_chromosome_files/TAIR10_chr_all.fas.gz
gunzip TAIR10_chr_all.fas.gz 
mv TAIR10_chr_all.fas TAIR10_chr_all.fa
samtools faidx TAIR10_chr_all.fa
sudo jbrowse add-assembly TAIR10_chr_all.fa --load copy --out /var/www/html/jbrowse2  
```

Then, go to the website `http://your_ip_address`, you should see there are options for creating a new session. 

## References

* [JBrowse web setup using the CLI](https://jbrowse.org/jb2/docs/quickstart_web/#upgrade-jbrowsecli-to-the-latest) 
* [I downloaded jbrowse2 using the command line interface, but encountered an error that prevented the page from loading. #3684](https://github.com/GMOD/jbrowse-components/discussions/3684) 
* [How to install Node.js via binary archive on Linux](https://github.com/nodejs/help/wiki/Installation#how-to-install-nodejs-via-binary-archive-on-linux) 
* [Jbrowse](https://jbrowse.org/jb2/) 
* [nodejs.org](https://nodejs.org/en) 
* [ChatGPT](https://chat.openai.com/) 