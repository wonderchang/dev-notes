# Debian

  - [Debian \-\- The Universal Operating System](https://www.debian.org/)

      - Debian is a free operating system (OS) for your computer. An operating system is the set of basic programs and utilities that make your computer run.
      - Debian provides more than a pure OS: it comes with over 51000 PACKAGES, precompiled software bundled up in a nice format for easy installation on your machine.

  - [Debian \-\- About Debian](https://www.debian.org/intro/about) #ril

## Release, Life Cycle ??

  - [Debian \-\- Debian Releases](https://www.debian.org/releases/) #ril
  - [DebianReleases \- Debian Wiki](https://wiki.debian.org/DebianReleases) #ril

## Locale ??

  - [Locale \- Debian Wiki](https://wiki.debian.org/Locale)

      - Programs that support LOCAL TECHNOLOGY use ENVIRONMENT VARIABLES to determine the CONVENTIONS to use for date and time formatting, character display, currency display and CODEPAGE selection.

      - The following environment variables affect locale related behaviour of the system:

        原始為只規範數字、日期時間、金額，沒想到連紙張大小、電話號碼都有。

          - `LANG` - Determines the DEFAULT LOCALE in the absence of other locale related environment variables
          - `LANGUAGE` - List of FALLBACK message translation languages (GNU only) ??
          - `LC_ADDRESS` - Convention used for formatting of street or postal addresses

          - `LC_ALL` - OVERRIDES all other locale variables (except `LANGUAGE`)

            Using `LC_ALL` is STRONGLY DISCOURAGED as it overrides everything. Please use it only when TESTING and never set it in a STARTUP FILE.

          - `LC_COLLATE` - Collation order
          - `LC_CTYPE` - Character classification and case conversion
          - `LC_MONETARY` - Monetary formatting
          - `LC_MEASUREMENT` - Default measurement system used within the region
          - `LC_MESSAGES` - Format of interactive words and responses
          - `LC_NUMERIC` - Numeric formatting
          - `LC_PAPER` - Default paper size for region
          - `LC_RESPONSE` - Determines how responses (such as Yes and No) appear in the local language
          - `LC_TELEPHONE` - Conventions used for representation of telephone numbers
          - `LC_TIME` - Date and time formats

      - Normally how it works is you set `LANG` to your PREFERRED LOCALE. If there are SPECIFIC ASPECTS of your PRIMARY LOCALE that you don't like (e.g. date formats), then you set the specific variables to override those features only.

        原來正確的用法以 `LANG` 為大方向，再用 `LC_XXX` 細部微調。

        End users should never set `LC_ALL`, at least not permanently. `LC_ALL` is reserved for programs or situations where you need to ENFORCE a specific locale (usually "`C`" ??) on a TEMPORARY BASIS. One example of this would be REPORTING ERROR MESSAGES on an English-speaking mailing list; you can use  `LC_ALL=C` your command to ensure that the errors are in English, and follow all the POSIX norms.

    Standard

      - Get root and type `dpkg-reconfigure locales` and select the locale(s) you want to GENERATE. At the end, you'll be asked which one should be the DEFAULT.

        If you have users who access the system through ssh, it is recommended that you choose None as your default locale.

        下面提到，default locale 會覆寫 SSH 提供的 local，對連線的人不方便? 覺得還好，使用者再自訂 local 即可。

      - This changes `/etc/default/locale` and `/etc/locale.gen` (in older versions of Debian, also `/etc/environment`). If you chose a default locale other than None above, it will be in `/etc/default/locale` and will OVERRIDE the `LANG` variable supplied by ssh. This is highly inconvenient.

            $ cat /etc/default/locale
            #  File generated by update-locale
            LANG=en_US.UTF-8

            $ head /etc/locale.gen
            # This file lists locales that you wish to have built. You can find a list
            # of valid supported locales at /usr/share/i18n/SUPPORTED, and you can add
            # user defined locales to /usr/local/share/i18n/SUPPORTED. If you change
            # this file, you need to rerun locale-gen.


            # aa_DJ ISO-8859-1
            # aa_DJ.UTF-8 UTF-8
            # aa_ER UTF-8
            # aa_ER@saaho UTF-8

        產生 locale 的工具是 `locale-gen`，它會讀取 `/etc/locale.gen` 來決定要產生哪些 locale；至於系統可支援的 locale 有哪些，則要看 `/usr/share/i18n/SUPPORTED`。

            $ grep -v '^#' /etc/locale.gen


            en_US.UTF-8 UTF-8
            lzh_TW UTF-8
            zh_TW.UTF-8 UTF-8

      - If you've upgraded to Lenny from an older version of Debian and have leftover `LANG=...` content in `/etc/environment`, you should comment it out (type `editor /etc/environment` and put a `#` character in front of the line, and then save it).

    Manually

      - Edit the file `/etc/locale.gen` and add your locale settings (one set per line), e.g.:

            de_DE.UTF-8 UTF-8
            de_DE ISO-8859-1
            de_DE@euro ISO-8859-15

      - The supported locales are listed in `/usr/share/i18n/SUPPORTED`.
      - Run the command `locale-gen`
      - Run the command `locale -a` to verify the list of AVAILABLE locales; (也就是 `locale-gen` 產生，真正可以用的 locales) note that the spellings change.
      - If you've upgraded to Lenny and you have leftover `LANG=...` content in `/etc/environment`, you should comment it out.
      - To use the new settings with your programs, log out and back in.

  - [environment variables \- What does C in LC\_ALL=C mean? \- Ask Ubuntu](https://askubuntu.com/questions/801933/)

      - Sergiy Kolodyazhnyy: I know very well that to override locale settings we can use `LC_ALL` prepended to the command one wants to run. I also know `C` uses default locale of a system. But what does `C` stand for ?

      - user4556274: `C` stands for the C programming language. It is a synonym for the `POSIX` locale.

        > The POSIX locale can be specified by assigning to the appropriate environment variables the values "C" or "POSIX".
        >
        > http://pubs.opengroup.org/onlinepubs/009695399/basedefs/xbd_chap07.html#tag_07_02

        原來 `POSIX` 也是一種 locale，呼應了上面 "follow all the POSIX norms" 的說法。

## Package ??

  - [Debian \-\- Packages](https://www.debian.org/distrib/packages) #ril

## 安裝設定 {: #installation }

直接下載 amd64 的 image (`debian-<version>-amd64-netinst.iso`) 即可，約 250 MB。例如：

    $ curl -LO https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-9.8.0-amd64-netinst.iso

參考資料：

  - [Debian \-\- Getting Debian](https://www.debian.org/distrib/)

    Download an installation image

      - Depending on your Internet connection, you may download either of the following:

          - A small installation image: can be downloaded quickly and should be recorded onto a removable disk. To use this, you will need a machine with an Internet connection.

            下載 image 本身很快，但安裝過程中還要從網路下載其他東西。如果網路不慢的話，這會是較好的選擇 -- 只下載需要用到的東西。

          - A larger complete installation image: contains more packages, making it easier to install machines WITHOUT an Internet connection.

    Use a Debian cloud image

      - An official cloud image: can be used directly on your cloud provider, built by the Debian Cloud Team.

    Try Debian live before installing

      - You can try Debian by booting a live system from a CD, DVD or USB key without installing any files to the computer. When you are ready, you can run the included installer. PROVIDED the images meet your size, language, and package selection requirements, this method MAY BE suitable for you. Read more information about this method to help you decide. 預裝某些東西，但不一定符合需求

  - [Installing Debian via the Internet](https://www.debian.org/distrib/netinst)

      - This method of installing Debian requires a functioning Internet connection during installation. Compared to other methods you end up DOWNLOADING LESS DATA as the process will be TAILORED to your requirements. Ethernet and wireless connections are supported. Internal ISDN cards are unfortunately not supported.

    Small CDs or USB sticks

      - The following are image files. Choose your processor architecture below.
      - For details, please see: Network install from a minimal CD

    Tiny CDs, flexible USB sticks, etc.

      - You can download A COUPLE OF image files of small size, suitable for USB Sticks and similar devices, write them to the media, and then start the installation by booting from that. ...

    Network boot

      - You set up a TFTP and a DHCP (or BOOTP, or RARP) server which will serve the installation media to machines on your local network. If your client machine's BIOS supports it, you can then boot the Debian installation system from the network (using PXE and TFTP), and proceed with installing the rest of Debian from the network.

        所以 server 上要提供完整的版本?? 因為要安裝的機器只要有連到 intranet 即可，不一定要有 internet?

      - Not all machines support booting from the network. Because of the additional work required, this method for installing Debian is not recommended for novice users.

  - 記錄幾個安裝 Debian 9.8.0 過程中被問及的問題：

      - Set up users and passwords 這個步驟要求輸入 root 的密碼，沒有提供的話就不會有 `root` 使用者。

        > If you leave this empty, the root account will be disabled and the system initial user account will be given the power to become root using the "sudo" command.

      - Software selection 在安裝 core 完後，可以選擇要再加裝什麼，預選了 Debian desktop environment (但沒有勾選 GNOME 或 KDE 等任何一項)、print server 及 standard system utilities；沒選 GNOME 或 KDE 好像會自動裝 GNOME。

  - [64-bit PC (amd64) - Debian GNU/Linux Installation Guide](https://www.debian.org/releases/stable/amd64/) #ril

### VirtualBox ??

  - 記錄幾個安裝 Debian 9.8.0 過程中被問及的問題：

      - 視窗開全螢幕時，桌面不會跟著改變?

        [Debian 9\.8 Installation in Virtualbox \+ Guest Addition Installation to get full screen \- YouTube](https://www.youtube.com/watch?v=WgTFow66Yps) 先裝 `build-essential` 及 `linux-headers-$(unmae -r)` 套件，再用 `sudo sh VBoxLinuxAdditions.run` 安裝 Guest Additions，重開機就可以成功切換到全螢幕。

        另外發現 VM 設定 Display > Graphics Controller 要維持預設的 VMSVGA 才行；桌面有點頓頓的，改用 Ubuntu 會不會好很多 (有專用的 driver)??

      - 裝了 SSH server，但預設的 NAT mode 會連不到? 透過 Network > Advanced > Port Forwarding，例如 Guest 22 --> Host 22。

  - [VirtualBox \- Debian Wiki](https://wiki.debian.org/VirtualBox) 為何 Debian 的 wiki 要花時間解釋如何安裝 VirtualBox，而非如何在 VirtualBox 裡安裝 Debian? #ril

## 參考資料 {: #reference }

  - [Debian](https://www.debian.org/)

文件：

  - [Debian Documentation](https://www.debian.org/doc/)

手冊：

  - [Debian Manpages](https://manpages.debian.org/)

相關：

  - [VirtualBox](virtualbox.md)