# AuthRemoteUser #

This *<a href="https://www.dokuwiki.org/" target="_blank">DokuWiki</a>*
<a href="https://www.dokuwiki.org/plugin:authremoteuser"
target="_blank">plugin</a> provides Single Sign On authentication via web
server's `REMOTE_USER` environment variable which is set through authentication
systems like

  * HTTP-Auth,
  * LDAP,
  * CAS,
  * Cosign,
  * NTLM,
  * PAM,
  * WebAuth,
  * SSPI,
  * and so on.

It uses the default plain text file `conf/users.auth.php` to store user
information. 

## Installation ##

 1. Enable an authentication system which sets `REMOTE_USER` (and disable
    anonymous authentication) on your web server.

 2. Search and install the plugin using the
    <a href="https://www.dokuwiki.org/plugin:extension"
    target="_blank">Extension Manager</a>. Refer to
    <a href="https://www.dokuwiki.org/plugin_installation_instructions#manual_instructions"
    target="_blank">Plugin Installation Instructions</a> on how to install
    plugins manually.

## Usage ##

 1. Determine your `REMOTE_USER` name:

     1. Save file `phpinfo.php` on your web server:

        ```php
        <?PHP
            phpinfo();
        ?>
        ```

     2. Open `phpinfo.php` in your web browser and search for the value in
        `_SERVER[“REMOTE_USER”]`.

     3. Add this value as new user ID to your user list if it is missing and
        add them groups `admin` and `user`.[^1]

     4. Remove file `phpinfo.php`.

 2. In your *DokuWiki* login as superuser, click *Admin*, choose
    *Configuration Settings*, and configure these settings:

     1. Disable action `profile`.

     2. If enabled, disable option `subscribers` temporarily.

     3. Enable `authtype` *AuthRemoteUser*.

     4. Disable `rememberme`.

     5. Save this configuration.

 3. Remove *DokuWiki* cookie from your browser or close and restart your
    browser.

 4. Reload your *DokuWiki* installation. Your login should be automatically
    detected.

 5. Now, you can re-enable option `subscribers` again (see above).

Copy the configuration settings to the `conf/local.protected.php` file to
<a href="https://www.dokuwiki.org/plugin:config#protecting_settings"
target="_blank">protect the settings</a> against changes via *Config Manager*.

Administration of users and its groups is done in the *User Manager* which is
fully supported by this plugin.

## Storage ##

*AuthRemoteUser* uses the same storage backend like *authplain* that is
`conf/users.auth.php`. Users which are added after switching to
*AuthRemoteUser*, won't contain an encrypted password.

That is: You can switch back to *authplain* (and enable `profile` setting)
whenever you want, and all your users which were already added before are still
able to login using their (hopefully yet known) password. All other users can
use the *forget my password* link.

### File Format ###

Empty lines, and everything after a `#` character are ignored. Each line
contains a colon separated array of five fields:

```txt
loginname:password:Real Name:email:groups
```

  * `loginname`:  
    This has to be a valid <a href="https://www.dokuwiki.org/pagename"
    target="_blank">page name</a>.
  * `password`:  
    Encrypted password if user id was added using *authplain*, otherwise empty.
  * `Real Name`:  
    Real name of the user.
  * `email`:  
    Email address of user.
  * `groups`:  
    Comma separated list of groups a user is member of. The group names must
    follow the rules of valid <a href="https://www.dokuwiki.org/pagename"
    target="_blank">page names</a>.

### Editing ###

Since `conf/users.auth.php` is a plain text file, it can be edited with any
text editor.



[^1]:	Don't be surprised: The user ID is converted to a valid page name.

