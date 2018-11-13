# console-login-helper-messages

Uses `motd`, `issue`, and `profile` to show helper messages before/at login.

## Internal operation

Let `x` denote `{motd,issue}`.

- `x`gen scripts source files from `/etc/console-login-helper-messages/x.d`, `/run/console-login-helper-messages/x.d`, and `/usr/lib/console-login-helper-messages/x.d`, and generate a file at `/run/x.d/40_console-login-helper-messages.x`
- A symlink `/etc/issue.d/console-login-helper-messages.issue -> /run/issue.d/console-login-helper-messages.issue` is created as agetty will only look for files in `/etc/issue.d` as of today, so a symlink to the generated one is required
- Users may continue to add their own `x`s by placing files in `/etc/x.d/`
- Users may also drop files into `/etc/console-login-helper-messages/x.d/` to have the `x`gen services append their files to the generated `x`. This is to preserve Container Linux functionality where appending messages to the overall generated message was available, not just placing a file into a public directory then searched by programs like PAM/sshd and agetty.

## Next steps
- [ ] testing that the info we need shows
  - [ ] a "you should not be sshing into this OS" message in motd **[coreos specific]**
  - [ ] a "dev info" message (motd and issue) **[coreos specific]**
  - [x] ssh keys in issue and motd (NOTE: ssh-keygen functionality will not be handled here)
  - [x] added users in issue and motd
  - [x] ip address in issue
  - [x] some info  on updates (booting, pending, etc) from rpm-ostree status --json? in motd (db, upgrade, status, version) (see https://github.com/rtnpro/motdgen/blob/master/motdgen-cache-updateinfo)
  - [x] failed units on login
- [ ] check installation against FCOS, F29, F29AH, Rawhide
- [ ] ensure licensing is correct
- [ ] %check
- [ ] CI for master branch
- [ ] tidy up code, comments
- [ ] user manual (brief)
- [ ] support yum and rpm

## Enhancements for future
- [x] have upstream PAM include the "trying" functionality, use this config rather than symlinks
- [ ] have upstream PAM search issue.d with pam_issue.so (rather than agetty, go through one interface - PAM)
- [ ] automake @macros@