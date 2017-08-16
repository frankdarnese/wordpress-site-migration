# WordPress Live/Local Site Migration

### Prerequisites

Before you follow this steps please ensure that the <a href="https://www.mamp.info/en/">MAMP</a> stack have been installed. Please also ensure that you have WP-CLI installed [see instructions below](#3).

![MAMP v4.1.1+](https://img.shields.io/badge/MAMP-v4.1.1%2B-brightgreen.svg) ![MySQL v5.6.35+](https://img.shields.io/badge/MySQL-v5.6.35%2B-brightgreen.svg) ![PHP v7.0.15+](https://img.shields.io/badge/PHP-v7.0.15%2B-brightgreen.svg) ![phpMyAdmin v4.6.5.2+](https://img.shields.io/badge/phpMyAdmin-v4.6.5.2%2B-brightgreen.svg)
![WordPress v4.8+](https://img.shields.io/badge/WordPress-v4.8%2B-brightgreen.svg)
![WP-CLI v1.2.4+](https://img.shields.io/badge/WPCLI-v1.2.4%2B-brightgreen.svg)

## Quick guide
- Download Database from the live server
- Search and replace database links
- Download WordPress
- Clone or download the existing Wordpress theme
- Download images & plugins
- Run the WordPress install

### Table of Contents
1. [Download Database from the live server](#1)
2. [Search and replace database links](#2)
3. [Installing WP-CL](#3)
4. [Download WordPress](#4)
5. [Clone or download the existing Wordpress theme](#5)
6. [Download images & plugins](#6)
7. [Run the WordPress install](#7)
8. [Testing](#8)
9. [More reading](#9)
10. [Footnotes](#10)

### <a name="1"></a>1. Download Database from the live server

You can download the database directly from your web server or alternatively, if you have ** SSH access ** to your live server, you can use the command line:

See example below:

```TXT
1. website-wp user$ ssh www-data@125.25.125.125
2. Are you sure you want to continue connecting (yes/no)? yes
3. www-data@125.25.125.125's password: your-ssh-password
4. www-data@website-wp:~$ ls
html  mod_cloudflare
5. www-data@website-wp:~$ cd html/
6. www-data@website-wp:~/html$ wp db export db.sql
7. www-data@website-wp:~/html$ exit
8. Logout and DOWNLOAD DATABASE VIA SFTP
```

** Please not the IP address above is just an example **

### <a name="2">2. Search and replace database links

![Alt text](images/mysql-updates.png?raw=true "Optional Title")

We need to replace the live site links to localhost links. We can use search and replace from the command line ( instructions to come ) or if you use phpMyAdmin, go to the databases and click to the database name you want to change, then click on the SQL tab (2 tab from top menu) and then insert the following command inside the:

Run SQL query/queries on database DATABASENAME

```TXT
UPDATE wp_options SET option_value = replace(option_value, 'http://localhost:8888/testing', 'http://www.live-website.com') WHERE option_name = 'home' OR option_name = 'siteurl';

UPDATE wp_posts SET guid = replace(guid, 'http://localhost:8888/testing','http://www.live-website.com');

UPDATE wp_posts SET post_content = replace(post_content, 'http://localhost:8888/testing', 'http://www.live-website.com');

UPDATE wp_postmeta SET meta_value = replace(meta_value,'http://localhost:8888/testing','http://www.live-website.com');

UPDATE wp_options SET option_value = replace(option_value, 'http://www.localhost:8888/testing', 'http://www.live-website.com') WHERE option_name = 'home' OR option_name = 'siteurl';

UPDATE wp_posts SET guid = replace(guid, 'http://www.localhost:8888/testing','http://www.live-website.com');

UPDATE wp_posts SET post_content = replace(post_content, 'http://www.localhost:8888/testing', 'http://www.live-website.com');

UPDATE wp_postmeta SET meta_value = replace(meta_value,'http://www.localhost:8888/testing','http://www.live-website.com');
```

Alternatively use this nice and easy script <a href="https://codepen.io/FDarnese/full/OjJLya">link here</a> that takes the old and new URLs and hands you the sql code for the WordPress swap.

### <a name="3">3. Installing WP-CLI

The WordPress installation and general development is made easier through the use of WP-CLI tool. Follow the commands below to install it prior to setting up the project.

```TXT
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp
```
### <a name="4">4. Download WordPress

Download and create a new WordPress project folder:

```TXT
mkdir your-wordpress-folder && cd your-wordpress-folder
wp core download --locale=en_GB
```

Or if you have 'Oh My Zsh' installed, you can simplify this with the take command which will create and swap into the directory:

```TXT
take your-wordpress-folder && wp core download --locale=en_GB
```

Alternatively go to <a href="https://wordpress.org/download/">wordpress.org</a> and download WordPress directly from there.

### <a name="5">5. Clone or download the existing WordPress theme

You need to clone from GitHub or download through FTP the existing theme into your WordPress theme folder:

```TXT
cd wp-content/themes && rm -rf twenty*
mkdir your-theme && cd your-theme
git clone git@github.com:frankdarnese/wordpress-site-migration.git
```
or

- FTP to your server and download the theme folder
- go to the theme folder and initialize .GIT from command line

### <a name="6">6. Download images & plugins

Download images & plugins via FTP from the LIVE wp-content folder to the local  wp-content folder

### <a name="7">7. Run the WordPress install

Go to MAMP websites/ localhost:8888 and click on the new website we have just created on step 4 and run the installation.

(You will get a message saying that WordPress is already installed, that's absolutely fine as the database will take care of the rest).

IMPORTANT: Login with the online username and password. This is due to the fact that the database stores the live server dates.

### <a name="8">8. Testing

You should be up and running with the local WordPress site on `http://localhost:8081/WORDPRESS-SITE`. Just run some test to make sure all is working fine and there are no conflict or plugins issues.


### <a name="9">9. More Reading

- <a href="https://wpbeaches.com/updating-wordpress-mysql-database-after-moving-to-a-new-url/">Change and Update WordPress URLS in Database When Site is Moved to new Host</a>

- <a href="https://developer.wordpress.org/cli/commands/search-replace/">Wp search-replace</a>

- <a href="https://www.webnots.com/how-to-move-live-wordpress-site-to-localhost/">How to Move Live WordPress Site to Localhost?</a>

### <a name="10">10. Footnotes

#### Contributions

Thank you in advance for your contributions! This repository doesn't really contain code, it serves to share my expertise with the world, hence I am not really expecting a lot of contributions from the community.

If you would like to fix a typo or propose suggestions, you are welcome to. Simply make sure your changes are being proofread and then submit a Pull Request. Thanks! 😄
