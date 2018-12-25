
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
