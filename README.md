# Next Active Directory Integration
Next Active Directory Integration allows WordPress to authenticate, authorize, create and update users against Microsoft Active Directory. Next ADI ist a complete rewrite of its predecessor [Active Directory Integration](https://wordpress.org/plugins/active-directory-integration/). You can easily import users from your Active Directory into your WordPress instance and keep both synchronized through Next Active Directory Integration's features.

If you like this plug-in we'd like to encourage you to purchase a support plan from [https://active-directory-wp.com/](https://active-directory-wp.com/shop-overview/) to support the ongoing development of this plug-in.

## Important requirement changes
As of *2021-12-09* NADI did *no* longer support PHP version *< 7.4*. The reason is that security support for PHP 7.3 and below has beeen dropped by the maintainers as you can see in the official PHP documentation http://php.net/supported-versions.php. 
For security reasons and in order to use NADI in 2022 we hereby politely encourage you to migrate your environments to at least PHP 7.4 until then.

Thank you all for your support and understanding.

## Running
You can download the ready-to-use version from the [WordPress.org Plugin Directory](https://wordpress.org/plugins/next-active-directory-integration) or from the [SVN repository](https://plugins.svn.wordpress.org/next-active-directory-integration) hosted by WordPress.org.

## Running (for developers)
Clone this Git repository inside the *wp-content/plugins* directory of your WordPress environment.

After the cloning you have to update the dependencies with help of *Composer* (execute `composer install` inside the cloned repository folder).
To install composer follow the instructions on [https://getcomposer.org/download/](https://getcomposer.org/download/).
	
### Testing
Tests are made with PHPUnit 9.5 Get PHPUnit 9.5 with

```shell
# get PHPUnit
wget https://phar.phpunit.de/phpunit-9.5.phar
```

#### Running unit tests

```shell
cd active-directory-integration2
# run unit tests with default PHPUnit configuration
php ./vendor/bin/phpunit --testsuite "unit" --configuration phpunit.xml
``` 

#### Running integration tests 

```shell
cd active-directory-integration2
# running integration test against a local install Active Directory instance
# executing the ITs with PHP binary is required for of passing environment variables to the test
php -d AD_ENDPOINT=127.0.0.1 -d AD_PORT=389 -d AD_USERNAME=username@domain.com -d AD_PASSWORD=Password -d AD_USE_TLS='' -d AD_SUFFIX=@domain.com -d AD_BASE_DN='DC=domain,DC=com' path/to/phpunit.phar --testsuite "integration" --no-coverage
```

#### Running all tests

```shell
cd active-directory-integration2
# running integration test against a local install Active Directory instance
# executing the ITs with PHP binary is required for of passing environment variables to the test
php -d AD_ENDPOINT=127.0.0.1 -d AD_PORT=389 -d AD_USERNAME=username@domain.com -d AD_PASSWORD=Password -d AD_USE_TLS='' -d AD_SUFFIX=@domain.com -d AD_BASE_DN='DC=domain,DC=com' path/to/phpunit.phar --no-coverage
```

#### Running all tests in PhpStorm

Run > Edit Configurations > Defaults > PHPUnit
	
- Test Runner options: `--test-suffix Test.php,IT.php`
- Interpreter options: `-d AD_ENDPOINT=127.0.0.1 -d AD_PORT=389 -d AD_USERNAME=Administrator -d AD_PASSWORD=Pa$$w0rd -d AD_USE_TLS='' -d AD_SUFFIX=@test.ad -d AD_BASE_DN='DC=test,DC=ad'`

#### Update translation

After changing the next_ad_int-de_DE.po you have to build the `next_ad_int-de_DE.mo` and `next_ad_int-de_DE_formal.mo` file.
```shell
# Execute this command inside the plugin root folder (with the index.php)
ant compile-all-languages
# or execute this:
ant -Dmsgfmt=/path/to/gettext/msgfmt compile-all-languages
```
Make sure that you have GNU gettext with *msgfmt* installed.

It is also possible to generate the `next_ad_int-de_DE.mo` with Poedit (or some other .po tool). You can create a copy from the `next_ad_int-de_DE.mo` file and name it `next_ad_int-de_DE_formal.mo`.

### Continuous Integration
We are using GitHub Action for the CI/CD process. You can find everything related to CI/CD inside `.github/workflows`.

The branches

- *master*/*main*
- and *develop*

will be automatically tested.

### Release process
Every pushed tag will be automatically tested. After a succesful test, it gets uploaded to wordpress.org.

To create a new release:

```bash
git tag -a ${VERSION} -m "release of ${VERSION}"
git push origin ${VERSION}
```