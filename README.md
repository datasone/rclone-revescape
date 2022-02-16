# rclone-revescape
A patch for changing rclone's filename escaping behavior

# Filename escaping
As many cloud drives allows special characters invalid for local filesystems in cloud filenames, rclone utilizes a character-replacing technique to replace those invalid characters to their full-width unicode forms. And to avoid conflictions, those fullwidth characters are prepended with a quotation mark: ‛.

E.g., for cloud file named after `a/b`, rclone will name it with `a／b`. And for `a／b`, its name in rclone will be `a‛／b`

# About this patch
In some (or many) circumstances, the cloud file is directly uploaded by end users, and won't be named using invalid characters. Using full-width characters to avoid invalid filenames is also a common practice, especially in regions already using them (i.e. CJK). The current behavior of rclone causes filename mismatch for filenames with both invalid half-width characters and valid full-width characters, causing problems in file display, exchanging and syncing.

This patch reverses the above behavior, which is: `a/b` changed to `a‛／b`, and `a／b` stays untouched, to better protect filenames with full-width characters. The patch changes how rclone encodes filename, thus is incompatible with the original rclone.

The current patch is made on the `v1.57.0` tag of rclone's git repository.
