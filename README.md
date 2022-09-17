# A2Stream
A2Stream simultaneously receives an **.a2stream** file from any HTTP server and plays it through the stock Apple II speaker circuit using pulse-width modulation at 22050Hz sample rate with ~5.17 bit resolution.

Hardware requirements:
* Enhanced //e (with extended 80 column card) or IIgs
* [Uthernet II](https://a2retrosystems.com/products.htm) Ethernet card in any slot

To view demo videos:
* Visit https://youtu.be/YcYEnYsI-_M
* Visit https://youtu.be/7kxo0l3t1LM

To stream an **.a2stream** file:
* Run **A2STREAM.SYSTEM** and enter the URL of an **.a2stream** file
* Use these shortcuts to make use of persistent history and auto-complete:
  | Key    | Function                       |
  |--------|--------------------------------|
  | Left   | Move cursor one char left      |
  | Right  | Move cursor one char right     |
  | Ctrl+A | Move cursor to start of line   |
  | Ctrl+E | Move cursor to end of line     |
  | Delete | Delete char left of cursor     |
  | Ctrl+D | Delete char under cursor       |
  | Up     | Move to previous history entry |
  | Down   | Move to next history entry     |
  | Tab    | Move to next auto-complete     |
  | Ctrl+C | Quit from auto-complete        |
* Find sample **.a2stream** files at these (case-sensitive) URLs:
  * `http://a2retro.de/a2s/Levels.a2stream`
  * `http://a2retro.de/a2s/Sandstorm.a2stream`
  * `http://a2retro.de/a2s/1984.a2stream`
  * `http://a2retro.de/a2s/HighwayToHell.a2stream`
  * `http://a2retro.de/a2s/HowsItSupposedToFeel.a2stream`
  * `http://a2retro.de/a2s/TimeToSayGoodbye.a2stream` - shows progress bar
  * `http://a2retro.de/a2s/HealYou.a2stream` - shows default cover art
* Find URLs of **.a2stream** files for all Open Apple podcast episodes on https://www.open-apple.net/a2stream/
* Use `Esc` to quit at any point
* Use `1`-`9` to fast-forward 1-9 minutes
* Use any other key to pause streaming

To config the Ethernet slot:
* Put the Uthernet II in slot 3 <ins>or</ins>
* Create a file named **ETHERNET.SLOT**. Only the first byte of that file is relevant. This byte can either represent your Uthernet II slot as binary value (e.g. $04 for slot 4), as ASCII digit (e.g. $34 for slot 4) or as Apple TEXT digit (e.g. $B4 for slot 4).

To get decent sound quality on the enhanced //e:
* Create a file named **OUTPUT.TAPE**. The content of that file is not relevant.
* Connect an amplifier/speaker to the tape output jack

To get decent sound quality on the IIgs:
* Connect an amplifier/speaker to the headphone jack

To prepare an **.a2stream** file for streaming:
* Create a header-less **.raw** file with 22050Hz mono 32-bit-float PCM data (e.g. with [Audacity](https://www.audacityteam.org/))
* Generate an **.a2stream** file from the **.raw** file with **gena2stream** ([source code](https://github.com/oliverschmidt/A2Stream/blob/main/gena2stream.c))
  * Put a standard 16kB **.dhgr** file beside the **.raw** file for custom cover art (optional)
  * Use the option to `-p` switch the visualization from *level meter* to *progress bar*
* Put the **.a2stream** file onto any HTTP (not HTTPS) server
  * Run a simple local HTTP server on Windows
    * Run the [HTTP File Server](http://www.rejetto.com/hfs/) and drop the file you want to stream in its _Virtual File System_
  * Run a simple local HTTP server on Linux
    * `cd` to the directory containing the file you want to stream and enter `python -m SimpleHTTPServer` or `python3 -m http.server` depending on the Python version you want to use

To play an internet live stream:
* Install [MPlayer](https://mplayerhq.hu/)
* Run **srva2stream** ([source code](https://github.com/oliverschmidt/A2Stream/blob/main/srva2stream.c))
  * Provide on the command line the TCP port number you want to use
  * Put a standard 16kB **srva2stream-&lt;port number&gt;.dhgr** file beside **srva2stream** for custom cover art (optional)
* Wait for the message `waiting on pipe <pipe name>`
* Run MPlayer
  * Provide command line parameters
  * `-nocache -af pan=1:0.5:0.5,volnorm=1:0.8,resample=22200:0:1,format=floatne -ao pcm:nowaveheader:file=<pipe name> <stream url>`
* Wait for the message `connect on pipe`
* Run **A2STREAM.SYSTEM** and enter as URL `<name or address of machine running srva2stream>:<port number>`
* Use the [step-by-step guide](LIVE.md) to install **srva2stream** on a Raspberry Pi (optional)
