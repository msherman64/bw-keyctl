# bw-keyctl
This is a short shell script to wrap linux keyctl around the bw cli utility.

Modified from, and inspired by:
https://github.com/bitwarden/cli/issues/65#issuecomment-683385208 by xPMo
https://github.com/xPMo/bwutil (zsh only as of 12/11/2020)

It stores the bw session key in the user keyring, using the linux kernel's secret storage functions.

The goal is to be able to share a session between different shells of the same user, 
similar to ssh-agent or gpg-agent.

Logic:
If the bw session key is already in the keyring, and readable by the current process, and can unlock the vault, 
then use it to run the bw command.

If the key in the keyring is not usable, 
e.g. does not exist, is not readable, or is not correct to unlock the vault, 
then let the user unlock the vault, and use the new session key to both update the keyring, and run the bw command.

If unlocking fails, then pass directly to bw, as the user most likely needs to log in, or change api keys, or similar.
