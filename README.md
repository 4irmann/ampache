 ![Logo](http://ampache.org/img/logo/ampache-logo_x64.png) Ampache Sermon Edition
=======
## Sermon Edition (German only!)

The **Ampache Sermon Edition** is a slightly modified version of the original Ampache  web media player. It is meant to be a special media player for playback of publicly available christian sermons, which are usually recorded during sunday morning services. For that purpose some Ampache features were modified: 

1. main goal was to have a slick and less cluttered UI for anonymous users who just want to listen to sermons or download sermons as MP3 files
2. the German translation files were customized so that the wording fits to the "sermon" context. E.g. the word "**song**" was replaced by the german word "**Predigt**". And the word "**artist**" was replaced by the german word "**Prediger**" and so on.
3. Sermons are publicly available. So a lot of Ampache features which are related to user login were deactivated or removed (e.g. via specific `ampache.cfg.php` and/or modification of some php files). Also social network integration and external music catalogues (e.g. lastfm) were disabled.

For original Ampache documentation have a look at: [www.ampache.org](http://ampache.org/) | [ampache.github.io](http://ampache.github.io)

## Requirements

* A web server. All of the following have been used, though Apache receives the most testing:
    * Apache
    * lighttpd
    * nginx
    * IIS
* PHP 5.4 or greater.
* PHP modules:
    * PDO
    * PDO_MYSQL
    * hash
    * session
    * json
    * simplexml (optional)
    * curl (optional)
* MySQL 5.x

## Installation from scratch via git cloning 

Right now, there are no dedicated tar ball releases for Ampache Sermon Edition. So the installation via git clone is recommended. Alternatively, you can download a generated tar ball from github (should be quite the same).

1. clone (checkout) desired ampache sermon-edition branch or tag using git client. E.g.
   `git clone https://github.com/4irmann/ampache-sermon-edition.git`
2. copy files to your ftp server.
   Hint: don't copy the following files and folders (they are not needed):

```bash
.git
.github
.gitattributes
.gitignore
nbproject
.phpcs
.scrutinizer.yml
.tgitconfig
.travis.yml
.tx
```

3. Optionally replace the following files with your own version 

```bash
favicon.ico
themes/reborn/images/ampache.png
```

4. prepare database 

5. Open webbrowser and run `install.php`, e.g. http://ampache.mydomain.org/install.phpcs

6. move `install.php` to `install.php.back` (or delete it)

7. Adjust `config/ampache.cfg.php` as you like. The following settings are recommended for public anonymous Ampache "sermon" hosting:

   ```properties
   ; if key is not already secure (by installer.php), adjust it
   secret_key = "<your secret key here>" 
   
   ; makes no sense for public anonymous access
   use_auth = "false"
   
   ; ratings clutter the UI, so we disable it
   ratings = "false"
   
   ; makes no sense for public anonymous access
   userflags = "false"
   
   ; Sociable
   ; This turns on / off all of the "social" features of ampache
   ; default is on, but if you don't care and just want sermons
   ; turn this off to disable all social features.
   sociable = "false"
   
   ; This turns on / off all licensing features on Ampache
   licensing = "false"
   
   ; Art Gather Order (we just use (mp3) tags as source)
   art_order = "tags"
   
   ; lastfm makes no sense for sermons
   lastfm_api_key = ""
   
   ; makes no sense for sermons (?)
   wanted = "false"
   
   ; makes no sense for public anonymous access 
   channel = "false"
   
   ; we support no live streams
   live_stream = "false"
   
   ; we support no podcasts
   podcast = "false"
   
   ; no statistics in UI 
   show_footer_statistics = "false"
   
   ; Makes no sense for public anonymous access
   allow_public_registration = "false"
   
   ; E.g. important in Germany (DSGVO)
   cookie_disclaimer = "true" 
   
   ; Needed if https is used
   force_ssl = "true"
   ```

8. If you need redirection to https, you can add the following `.htaccess` file to top level (e.g. `public_html/amp`) folder:

```php
# BEGIN Ampache
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} ^ampache\.mydomain\.org [NC]
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://ampache.mydomain.org/$1 [R,L]
RewriteRule ^index\.php$ browse.php?action=song [L]
</IfModule>
# END Ampache
```

Hints: some files are automatically created during installation, e.g.;

```bash
header.inc.php
config/ampache.cfg.php
lib/components
lib/vendor
play/.htaccess
rest/.htaccess
```

For further reading, see original Ampache installation hints in  [Ampache Wiki](https://github.com/ampache/ampache/wiki/Installation)

## Upgrading to new version via git cloning

1. clone (checkout) desired ampache branch or tag
2. read `docs/CHANGELOG.md`and install / upgrade instructions in `README.md`
3. **Important:** make a backup of your files (ftp server) and the database.
   Some old files will be needed later on
4. Delete all old files on your ftp server account
5. copy all new files to your ftp server account
   Hint: don't copy the following files and folders (they are not needed):

```bash
.git
.github
.gitattributes
.gitignore
nbproject
.phpcs
.scrutinizer.yml
.tgitconfig
.travis.yml
.tx
```

6. diff / merge compare old `config/ampache.cfg.php `  with the new `config/ampache.cfg.php.dist` and adjust old `config/ampache.cfg.php` according to your needs. That's important since config defaults may have changed, or new config features may be available ! Use e.g. a merge tool like meld for that.

7. upload the following old files to your ftp account:

   ```bash
   config/ampache.cfg.php # your merged old+new version !
   rest/.htaccess # optional
   play/.htaccess # optional
   favicon.ico # optional
   themes/reborn/images/ampache.png # optional
   .htaccess # optional for e.g. https
   ```

8. Just relaunch Ampache url in browser and thus all other necessary upgrade tasks will be handled by Ampache (e.g. database updates)

For further reading, see original Ampache installtion hints in [Ampache Wiki](https://github.com/ampache/ampache/wiki/Installation)

## Configuration via Web GUI

TODO

## License

Ampache Sermon Edition is free software; you can redistribute it and/or
modify it under the terms of the GNU Affero General Public License v3 (AGPLv3)
as published by the Free Software Foundation.

Ampache Sermon Edition includes some [external modules](https://github.com/ampache/ampache/blob/develop/composer.lock) that carry their own licensing.

## Translations

Ampache Sermon Edition is currently only available in German. Even English is **NOT SUPPORTED.** So German is the leading language for Sermon Edition.
If you want to translate from German to another language have a look at `/local/de_DE/LC_MESSAGES`. Search e.g. for the word `song`, or even better make a diff with original Ampache German translation. See also [/locale/base/TRANSLATIONS](https://github.com/4irmann/ampache-sermon-edition/blob/develop/locale/base/TRANSLATIONS.md) for more instructions.

## Credits

Thanks to all those who have helped make Ampache awesome: [Credits](docs/ACKNOWLEDGEMENTS)

Ampache Sermon Edition was done by 4irmann
