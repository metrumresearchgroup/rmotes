
# rmotes

This is an experimental fork of r-lib/remotes with the intent of leveraging
the remotes tooling to download the required remote packages, without doing any
actual installations.

This will allow alternative, downstream tools, to use the downloaded repositories.

## why?

remotes has extremely robust handling from repositories from all over the place
with many settings. Rather than re-inventing the wheel, these modifications
will allow as close to compatibility with remotes, while clearly deliniating 
what it is doing.

## what's changed?

The biggest change is hooking into the install command(s) to not actually install,
rather download and get to the point of installation but return there, without
cleaning up the temporarily downloaded folders.

## will this get merged in some way back to remotes?

Definitely not! The intent is not to add functionality, but rather to completely change
what the package is doing. This wouldn't really make sense to fold back into remotes
as it is a quite hacky.


## how does it work?

For example, querying to download pkgman and credentials from github:

```
> rmotes::install_github(c("r-lib/pkgman", "r-lib/credentials"), force = TRUE, upgrade = FALSE)
Downloading GitHub repo r-lib/pkgman@master
Downloading GitHub repo r-lib/pkgdepends@master
Downloading GitHub repo r-lib/pkgcache@master
Downloading GitHub repo r-lib/pkginstall@master
Downloading GitHub repo r-lib/credentials@master
Installing 1 packages: sys
Installing package into ‘/Users/devinp/Rpkgs/3.5’
(as ‘lib’ is unspecified)
trying URL 'https://cran.rstudio.com/bin/macosx/el-capitan/contrib/3.5/sys_2.1.tgz'
Content type 'application/x-gzip' length 68633 bytes (67 KB)
==================================================
downloaded 67 KB


The downloaded binary packages are in
	/var/folders/km/ljmnf0bx73l9m4zdygvb6q_80000gn/T//RtmpjsAoeq/downloaded_packages
Downloading GitHub repo jeroen/askpass@master
```

Now when checking the pkg src's, the folder where the downloaded
package was unpacked is shown. As you can see, the dependencies
are also captured.

```
> get_pkg_srcs()
$pkgman
$pkgman$b0b9fd8e316807a28b0ca5ae97275c5f722f5c8a
[1] "/var/folders/km/ljmnf0bx73l9m4zdygvb6q_80000gn/T//RtmpjsAoeq/remotes12ef368fc236/r-lib-pkgman-b0b9fd8"


$cliapp
$cliapp$`35b184ad35b392da469df54acc701f12470129cb`
[1] "/var/folders/km/ljmnf0bx73l9m4zdygvb6q_80000gn/T//RtmpjsAoeq/remotes12ef3b53f249/r-lib-cliapp-35b184a"


$pkgdepends
$pkgdepends$c3147b6a6573675b27beb858599d5ee277a6a2dd
[1] "/var/folders/km/ljmnf0bx73l9m4zdygvb6q_80000gn/T//RtmpjsAoeq/remotes12ef58a4ed2b/r-lib-pkgdepends-c3147b6"


$pkgcache
$pkgcache$a9721a912975b35cfca2a268a58392d4269c61eb
[1] "/var/folders/km/ljmnf0bx73l9m4zdygvb6q_80000gn/T//RtmpjsAoeq/remotes12ef93abab8/r-lib-pkgcache-a9721a9"


$pkginstall
$pkginstall$`7ec008799daf9823fbaf42314a5ac6972b3a0af7`
[1] "/var/folders/km/ljmnf0bx73l9m4zdygvb6q_80000gn/T//RtmpjsAoeq/remotes12ef4d6f2258/r-lib-pkginstall-7ec0087"


$credentials
$credentials$`28a7cf7d418e43ad94cc860bef61ebf9dd732526`
[1] "/var/folders/km/ljmnf0bx73l9m4zdygvb6q_80000gn/T//RtmpjsAoeq/remotes12ef6015767d/r-lib-credentials-28a7cf7"


$askpass
$askpass$`45bfd76551dadfff8090177ac25e2cbabffe438e`
[1] "/var/folders/km/ljmnf0bx73l9m4zdygvb6q_80000gn/T//RtmpjsAoeq/remotes12ef53814e88/jeroen-askpass-45bfd76"
```

If multiple configurations are specified for a given package, such as
one dependency requiring a specific branch/pull request, whereas others
just can take the tip, this will show up as multiple hashes for the given package.

As this does not do any futher solving to determine compatibility, 
that is left up to the user/admin to determine the subsequent course 
of action.
