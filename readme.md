# AimeIO CardReader

This allows you to a smartcard reader (specifically the acr122u) with segatools
Fixed a few issues with the [AkaiiKitsune's aimeio-cardreader](https://github.com/AkaiiKitsune/aimeio-cardreader):
- Works with Chunithm Luminous Plus
- Fixed double scanning issue (sometimes the card would be polled twice and read by segatools twice, causing a ghost login when you finish a credit/cancel a login)

Also brought in some functionality from [raviddog's aimeio-pcsc](https://github.com/raviddog/aimeio-pcsc), to quote:

```
If the game doesn't know what to do with the FeLiCa IDm, you can convert it into an access code by creating a file called aimeio_felicadb.txt and adding a line containing the FeLiCa IDm code, and the access code to substitute it with. 0123456789ABCDEF 12345678901234567890
```

# Acknowledgments

This is a plugin destined to be used with [Dniel97](https://gitea.tendokyu.moe/Dniel97)'s [segatools fork](https://gitea.tendokyu.moe/Dniel97/segatools) (but should work on others too).

SmartCard implementation taken from [spicetools](https://github.com/spicetools/spicetools)

Initial project made by [nat](https://gitea.tendokyu.moe/nat/aimeio-pcsc)

All the logic for reading cards from a file has been taken from [CrazyRedMachine](https://github.com/CrazyRedMachine)'s [RedBoard](https://github.com/CrazyRedMachine/RedBoard) aimeio.

# Usage

Make sure to install the ACR122U's [driver](https://www.acs.com.hk/en/driver/3/acr122u-usb-nfc-reader/) before using this.

To use it with a game, copy `aimeio.dll` to your `segatools` folder and add the following to your `segatools.ini`:

```ini
[aimeio]
path=aimeio.dll    ;Path to aimeio.dll
scan=0x0D          ;Sets the key which will be used to insert a card in game. The default is 'Return'


;Everything below this line is optional.

readerOptional=1            ;Make reader optional, so that you can still use the keyboard
readerName=ACS ACR122 0     ;Manually select which reader to use
disableBuzzer=1             ;Disable the buzzer 
;aimePath=""                ;Manually specify an aime.txt file
;felicaPath=""              ;Manually specify a felica.txt file
;debug=0                    ;Display function calls
```

## Scanning cards

If you scan a newer AIC-based Aime, its FeliCa IDm will be provided to the game. The game will not see the correct "access code," but the IDm should be unique to each card so that particular card can still track your plays.  

## Inserting cards

By pressing the key you have set in segatools.ini and holding it for a few moments, you will insert a card set in either aime.txt or felica.txt

You can have multiple cards in those files!  
By holding a number from 0 to 9 on your keypad, and then holding enter, you can choose what card to insert.

For example...

```text
aime.txt
---

1a7f3e925cb866d45a3b
a5d04d668bc529e35aaa
```

By only pressing the insert key, you will insert card0, "1a7f3e925cb866d45a3b".  
If you hold 1 on your numpad and press the insert key, card1, "a5d04d668bc529e35aaa" will be inserted !  

This works for up to 10 cards.

## Felica to Mifare

Create the file `aimeio_felicadb.txt` in the bin/ folder, where `Segatools.ini` is, and populate it in the following format:
`<felica_serial_code> <aic_access_code>`

Eg:
`12345678 1234567890`

One per line.

# Build

On linux:

```
meson setup --cross cross-mingw-64.txt b64
ninja -C b64
```