# IMG `basename`

## Summary

The IMG `basename` attribute is an attribute specified on the IMG element. It
indicates the basename of the filename to use when the user saves the image,
either via "Save Image" dialog option or by drag-and-dropping the image to the
system directory.

## Details

* [Initial proposal and discussion]([initial proposal](https://github.com/whatwg/html/issues/2722)

It is common for images to be served from servers that obfuscate filenames to
optimize storage. In this case, the User Agent uses the obfuscated filename as
the suggested filename to use when saving the file.

It is also possible that images come from a `data:` url, meaning that they
don't have a filename associated with them. In this case, the User Agent
selects a filename arbitrarily, with `download` and `unknown` being common
choices.

In either case, a better user experience would be to have developers specify
the basename to use, since developers have contextual information for the type
of image being downloaded. This is the proposal for the `basename` attribute.

Note that since the extension of the filename can be determined automatically
by the User Agent, we feel that the choice should not be in the developer's
options to avoid security problems (e.g. developer picking `.exe` as an
extension for the image which can cause inadvertent code execution by the
user).
