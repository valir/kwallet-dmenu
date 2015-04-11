# kwallet-dmenu
Use kwallet-query and this script to interface KWallet with I3 or other tiling
WM via dmenu.

## Purpose of these scripts
This repo contains a couiple of scripts that leverage the kwallet-query tool
and is mainly intended for the tiling window managers users. For instance I
used these scripts with i3wm.

The first script uses kwallet-query tool and dmenu to enable quick querying of
your main KDE wallet. You'll want to bind this script to a key and become able
to get any of your passwords from the KDE wallet in a matter of a few
keystrokes. You'll no longer need KWalletManager5. Ok, you'll still need it
when managing the passwords, but you'll forget it when querying the wallet.

The second script uses zenity to prompt you for the entry name and user name,
the it calls pwgen to generate a new password for you and finally it calls
kwallet-query to write these values into a new kwallet entry.  You'll want to
bind this script to another key and become able to quickly define a new
password in the clipboard when regsitering a new accound, for instance.

## Before installing
Please ensure you have the kwallet-query tool present on your system and
accessible on the system $PATH. This tool can be found in KDE playground/utils
https://projects.kde.org/projects/playground/utils/kwallet-query

kwallet-query is compatible with the KWallet from the KDE Frameworks
repositories (KF5). Let me know if you'd like to have it working with the old
KDE4 KWallet too.

## Installing
Clone this repository somewhere on your system.

Modify the WALLET variable to let it hold your actual KDE Wallet name.

Make the kwallet-dmenu script accessible on the system $PATH. For example:

    ln -s <path to kwallet-dmenu> ~/bin/
    ln -s <path to kwallet-pwgen> ~/bin/

Create a shortcut for it on your system. For example, you can add this to your
I3 configuration file ~/.config/i3/config :

    bindsym $mod+p exec kwallet-dmenu
    bindsym $mod+Shift+p exec kwallet-pwgen

Do not forget to reload the WM's configuration to get it working.

## Usage
KDE Wallet is quite flexible and each user organizes passwords as he likes.
This script and kwallet-query tool try not to interfere too much. A typical
usage might be like this:

* For each website or other secured access you might have, use KWallet Manager
  to create an entry under the Passwords/Passwords folder, in your wallet of
  choice. Please note kwallet-query ignores the other folders under Passwords
  folder, such as Maps, Binary data or Unknown
* In the contents box enter each piece of information in per-line fashion

The kwallet-pwgen script will orgnaniser the information exactly like that.
So, when registering somewhere, you'll be expected to enter user name and
password. When entering the password field, just press $mod+Shift+p and enter
the service name, then enter the user name you just defined then press
Shift+Insert to put the password on the register form. That's it!

Later-on, when visiting the same service, use $mod+p to invoke kwallet-dmenu
script. This script will call kwallet-query two times:

* First time, it'll query all entries under the Passwords/Passwords folder,
  and feed dmenu with them ; you'll see dmenu displaying them and you'll be
  able to navigate and select one entry
* The second time kwallet-query is asked to get the lines of text from the
  entry you selected during the previous step. It'll split the lines of text
  and once again feed them to dmenu to let you choose the one you want
* If a selection is made, then it's copied to the clipboard. The script uses
  xclip by default. You can adjust it if you're using some other tool to write
  to the clipboard.

Enjoy!

