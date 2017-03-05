javad
=====

A foundation for a maximally restricted and sandboxed Java service under systemd.
You can take some or all of the directives found here and apply them to your own service.

Files
-----

`javad.service` itself contains only directives for a sham Java service that does nothing.
All the sandboxing directives are found in drop-in files in `javad.service.d`,
split up by the systemd versions that introduced them.
`xyz.conf` contains directives that are available starting with systemd v*xyz*;
`pre-xyz.conf` emulates some of these directives for older systemd versions;
and `other.conf` contains some tweaks not related to systemd versions, but to other system factors.
You should not keep these options split when you assemble your service;
you will know the systemd version you’re targeting,
and can pick the right directives and put them all in the main service file.

`test` is a script that installs the service
(taking into account the systemd version and other system factors to include the appropriate drop-in files),
runs it and then uninstalls the service again.
If the service fails, the script exits with a nonzero exit code and shows the journal output of the service.

Usage
-----

Inspect the directives in the drop-in files and add those that seem useful to your own service.
You will almost certainly need to tweak them at least a little bit;
for instance, `RestrictAddressFamilies=` prohibits all network access, and the `SystemCallFilter` does not include `write`.

Restrictions
------------

A short, incomprehensive list of the restrictions that the service imposes upon the Java process:

- It has a read-only view of the entire file system, and doesn’t see home directories at all.
- It can’t access the network.
- It can’t `write` at all, not even “Hello, World!” to stdout, nor create or remove files or directories.
- It can’t see any real devices of the system.
- It can’t see other services’ temporary files.
- It can’t spawn other processes.
- It can’t gain new privileges (potentially evading these restrictions) via setuid binaries or other mechanisms.

License
-------

I believe the only parts of this repository that reach the threshold of originality and are therefore copyrightable
are the text parts of it: this README file and the comments in the service file and drop-in files.
I place these under [CC BY 4.0], requiring attribution if you include those comments in your service file.
If you just use the directives to build your service file, but discard the comments,
I would appreciate some form of attribution, but do not require it.

[CC BY 4.0]: https://creativecommons.org/licenses/by/4.0/

