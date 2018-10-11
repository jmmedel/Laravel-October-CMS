Minimum system requirements
Wizard installation
Troubleshooting installation
Command-line installation
Post-installation steps
Delete installation files
Review configuration
Setting up the scheduler
Setting up queue workers
There are two ways you can install October, either using the Wizard installer or Command-line installation instructions. Before you proceed, you should check that your server meets the minimum system requirements.

Minimum system requirements
October CMS has some server requirements for web hosting:

PHP version 7.0 or higher
PDO PHP Extension
cURL PHP Extension
OpenSSL PHP Extension
Mbstring PHP Library
ZipArchive PHP Library
GD PHP Library
Some OS distributions may require you to manually install the PHP JSON and XML extensions. For example, when using Ubuntu this can be done via apt-get install php7.0-json and apt-get install php7.0-xml respectively.

When using the SQL Server database engine, you will need to install the group concatenation user-defined aggregate.

Wizard installation
The wizard installation is a recommended way to install October. It is simpler than the command-line installation and doesn't require any special skills.

Prepare a directory on your server that is empty. It can be a sub-directory, domain root or a sub-domain.
Download the installer archive file.
Unpack the installer archive to the prepared directory.
Grant writing permissions on the installation directory and all its subdirectories and files.
Navigate to the install.php script in your web browser.
Follow the installation instructions.
image

Troubleshooting installation
An error 500 is displayed when downloading the application files: You may need to increase or disable the timeout limit on your webserver. For example, Apache's FastCGI sometimes has the -idle-timeout option set to 30 seconds.

A blank screen is displayed when opening the application: Check the permissions are set correctly on the /storage files and folders, they should be writable for the web server.

An error code "liveConnection" is displayed: The installer will test a connection to the installation server using port 80. Check that your webserver can create outgoing connections on port 80 via PHP. Contact your hosting provider or this is often found in the server firewall settings.

The back-end area displays "Page not found" (404): If the application cannot find the database then a 404 page will be shown for the back-end. Try enabling debug mode to see the underlying error message.

Note: A detailed installation log can be found in the install_files/install.log file.

Command-line installation
If you feel more comfortable with a command-line or want to use composer, there is a CLI install process on the Console interface page.

Post-installation steps
There are some things you may need to set up after the installation is complete.

Delete installation files
If you have used the Wizard installer you should delete the installation files for security reasons. October will never delete files from your system automatically, so you should delete these files and directories manually:

install_files/      <== Installation directory
install.php         <== Installation script
Review configuration
Configuration files are stored in the config directory of the application. While each file contains descriptions for each setting, it is important to review the common configuration options available for your circumstances.

For example, in production environments you may want to enable CSRF protection. While in development environments, you may want to enable bleeding edge updates.

While most configuration is optional, we strongly recommend disabling debug mode for production environments.

Setting up the scheduler
For scheduled tasks to operate correctly, you should add the following Cron entry to your server. Editing the crontab is commonly performed with the command crontab -e.

* * * * * php /path/to/artisan schedule:run >> /dev/null 2>&1
Be sure to replace /path/to/artisan with the absolute path to the artisan file in the root directory of October. This Cron will call the command scheduler every minute. Then October evaluates any scheduled tasks and runs the tasks that are due.

Note: If you are adding this to /etc/cron.d you'll need to specify a user immediately after * * * * *.

Setting up queue workers
You may optionally set up an external queue for processing queued jobs, by default these will be handled asynchronously by the platform. This behavior can be changed by setting the default parameter in the config/queue.php.

If you decide to use the database queue driver, it is a good idea to add a Crontab entry for the command php artisan queue:work --once to process the first available job in the queue.