title: Perl

The default version of Perl on Katana is 5.26.3, which is provided by Rocky 8.9 and can be found at `#!bash /usr/bin/perl`.

Perl 5.36.0 is also installed as an environment module. 

It is common for Perl scripts to begin with:

``` bash
#!/usr/bin/perl
```

However, that will restrict you to the default version of Perl supplied with the Linux distribution.  For greater flexibility, try using the following: 

``` bash
#!/usr/bin/env perl
```
Then if you choose to use a different version of Perl (by loading an environment module) it will not be necessary to modify your scripts.
