---
title:  "Hacking my \"smart\" toothbrush"
date:   2023-05-24 10:17:21 +0100
permalink: /toothbrush/

gallery:
  - url: /assets/images/toothbrush/brush_head.jpg
    image_path: /assets/images/toothbrush/brush_head.jpg
    alt: "Brush head"
    title: "Brush head"
  - url: /assets/images/toothbrush/nfc_chip.jpg
    image_path: /assets/images/toothbrush/nfc_chip.jpg
    alt: "Antenna"
    title: "Antenna"

gallery_nfc_tools:
- url: /assets/images/toothbrush/nfc_info.png
  image_path: /assets/images/toothbrush/nfc_info.png
  alt: "NFC Tools tag info"
  title: "NFC Tools tag info"

gallery_nfc_lab:
- url: /assets/images/toothbrush/nfc_lab.png
  image_path: /assets/images/toothbrush/nfc_lab.png
  alt: "Decoded traffic"
  title: "Decoded traffic"

gallery_nfc_update:
- url: /assets/images/toothbrush/nfc_before.png
  image_path: /assets/images/toothbrush/nfc_before.png
  title: "Before: 10s on timer"
- url: /assets/images/toothbrush/nfc_during.png
  image_path: /assets/images/toothbrush/nfc_during.png
  title: "Applying the update"
- url: /assets/images/toothbrush/nfc_after.png
  image_path: /assets/images/toothbrush/nfc_after.png
  title: "After: 0s on timer"


---

After buying a new [Philips Sonicare](https://www.philips.ch/c-p/HX6851_53/sonicare-protectiveclean-5100-elektrische-schallzahnbuerste) toothbrush I was surprised to see that it reacts to the insertion of a brush head by blinking an LED. 
A quick online search reveals that the head communicates with the toothbrush handle to remind you when it's time to buy a new one.
{:refdef: style="text-align: center;"}
![0](/assets/images/toothbrush/smart.png)  
*From the Philips product page: seems to be REALLY smart!*
{: refdef}

## Reverse Engineering

Looking at the base of the head shows that it contains an antenna and a tiny black box that is presumably an IC.
The next hint can be found in the manual where it says that: "Radio Equipment in this product operates at 13.56 MHz", which would indicate that it is an [NFC tag](https://en.wikipedia.org/wiki/Near-field_communication). 
And indeed when holding the brush head to my phone it opens a link to a product page: [https://www.usa.philips.com/c-m-pe/toothbrush-heads](https://www.usa.philips.com/c-m-pe/toothbrush-heads).
{% include gallery caption="" %}

Using the [NFC Tools](https://play.google.com/store/apps/details?id=com.wakdev.nfctools.pro) app we can learn a lot about this tag:

![](/assets/images/toothbrush/nfc_info.png){: .align-center width="50%"}

- It is an [NTAG213](https://www.nxp.com/products/rfid-nfc/nfc-hf/ntag-for-tags-labels/ntag-213-215-216-nfc-forum-type-2-tag-compliant-ic-with-144-504-888-bytes-user-memory:NTAG213_215_216)
- It uses NfcA
- It is password protected
- We can see the link to the Philips webpage

Also using NFC Tools, the memory and memory access conditions can be read:

|Address|Data|Type|Access|
|--|--|--|--|
|0x00|04:EC:FC:9C|UID0-UID2/BCC0|Read-Only|
|0x01|A2:94:10:90|UID3-UDI6|Read-Only|
|0x02|B6:48:FF:FF|BCC1/INT./LOCK0-LOCK1|Read-Only|
|0x03|E1:10:12:00|OTP0-OTP3|Read-Only|
|0x04|03:20:D1:01|DATA|Read-Only|
|0x05|1C:55:02:70|DATA|Read-Only|
|0x06|68:69:6C:69|DATA|Read-Only|
|0x07|70:73:2E:63|DATA|Read-Only|
|0x08|6F:6D:2F:6E|DATA|Read-Only|
|0x09|66:63:62:72|DATA|Read-Only|
|0x0A|75:73:68:68|DATA|Read-Only|
|0x0B|65:61:64:74|DATA|Read-Only|
|0x0C|61:70:FE:00|DATA|Read-Only|
|0x0D...|00:00:00:00|DATA|Read-Only|
|0x1F|00:01:07:00|DATA|Readable, write protected by PW|
|0x20|00:00:00:02|DATA|Read-Only|
|0x21|60:54:32:32|DATA|Read-Only|
|0x22|31:32:31:34|DATA|Read-Only|
|0x23|20:31:32:4B|DATA|Read-Only|
|0x24|B3:02:02:00|DATA|Readable,write protected by PW|
|0x25|00:00:00:00|DATA|Readable,write protected by PW|
|0x26|00:00:00:00|DATA|Readable,write protected by PW|
|0x27|00:00:00:01|DATA|Readable,write protected by PW|
|0x28|00:03:30:BD|LOCK2 - LOCK4|Readable,write protected by PW|
|0x29|04:00:00:10|CFG 0|Read-Only|
|0x2A|43:00:00:00|CFG 1|Read-Only|
|0x2B|00:00:00:00|PWD0-PWD3|Write-Only|
|0x2C|00:00:00:00|PACK0-PACK1|Write-Only|

I repeated this process for one black and two white [W DiamondClean](https://www.usa.philips.com/c-p/HX6062_65/sonicare-w-diamondclean-standard-sonic-toothbrush-heads) brush heads and learned the following:
- Address 0x00-0x02 contains a unique ID and its checksum
- Address 0x04-0x0C contains the link to the Philips store
- Address 0x22 is 31:32:31:34 for black and 31:31:31:31 for white heads respectively
- Address 0x24 contains the __total brush time__
- All other readable data is identical between all heads

### Decoding the stored time
Let's do an experiment to see what changes happen to the tag when using the toothbrush:
1. Read the tag
  - When reading a new brush head that has never been in contact with the data at addr. 0x24 is 00:00:02:00.
  - Simply attaching it to the handle (without brushing) changes nothing
2. Brush for some time
  - In this case, I let the toothbrush run for 5s
3. Read the tag again
  - The data at addr. 0x24 is now 05:00:02:00
4. Observe the difference 
  - Looks like addr. 0x24 saves the number of seconds that the brush head was in use

When the brush is used for more than 255s, this timer rolls over to the second bit (02:01:02:00 -> 258s).

Trying to overwrite the stored time is unfortunately unsuccessful, as this memory address is password protected.

## Sniffing the password
Luckily it turns out that the required password is sent over plain text! So all I need to do is to sniff the communication between the toothbrush and the head.
After digging out my [HackRF](https://greatscottgadgets.com/hackrf/) [software defined radio](https://en.wikipedia.org/wiki/Software-defined_radio) and some trial and error, I came up with the following workflow.

### Record RF signal
![](/assets/images/toothbrush/sniffing_in_progress.jpg){: .align-center width="100%"}

When opening [gqrx](https://gqrx.dk/) and tuning it to 13.736 MHz while holding the toothbrush close to the antenna, it is visible that the head gets polled multiple times a second. It is a welcome surprise that my simple monopole antenna gets a signal that is strong enough for this purpose. You can download the relevant gqrx configuration file [here](/assets/files/gqrx.conf).

![](/assets/images/toothbrush/gqrx.png){: .align-center width="100%"}

While brushing, the NFC polling takes a brief pause and the first burst of packets that follows updates the time counter. 
With the ability of gqrx to make I/Q recordings, we can capture the password RF signals like this:
1. Turn on the toothbrush
2. Start recording
3. Turn off the toothbrush
4. Stop the recording

The first packets in the file should now contain the password in plain text.

### Convert recording
![](/assets/images/toothbrush/gnuradio.png){: .align-center width="100%"}

Before this raw I/Q file can be decoded it needs to be converted into a slightly different format to be read by the decoding program.  
I created a small [gnuradio](https://www.gnuradio.org/) companion script that applies a lowpass filter and converts the data into a wav file with two channels that contain the real and imaginary components of the complex signal.  
Make sure to substitute the correct paths in the source/sink blocks and check the sampling frequency (I used 2MHz).  
You can download the script [here](/assets/files/sniff_NFC.grc).

### Decode recording
{% include gallery id="gallery_nfc_lab"%}
I found the perfect tool for this task called [NFC-laboratory](https://github.com/josevcm/nfc-laboratory). 
After opening the newly created WAV file, it should look something like the picture above. In this case, the recording is only good enough to see the communication that goes from host to tag (green arrow). But to sniff the password this is perfect.  
When looking at the [datasheet](https://www.nxp.com/docs/en/data-sheet/NTAG213_215_216.pdf#page=32) for the NTAG213, we can see what is happening:
- Line #0-#6: communication is established with the tags' unique ID
- Line #7: The toothbrush sends the __password__ (command 0x1B = PWD_AUTH)
- Line #9: The time counter is updated to the new value (command 0xA2 = WRITE)
- All lines below are repeated polling without password authentication or writing anything

So the password for this brush head is __67:B3:8B:98__ (underlined in the picture).

## Writing to the brush
With the password successfully acquired, it's now possible to set the counter on the brush head to anything we want by sending the relevant bytes over NFC.  
NFC Tools comes to the rescue again: 
1. Go to Other -> Advanced NFC commands 
2. Set the I/O Class to NfcA
3. Set the data to 1B:67:B3:8B:98,A2:24:00:00:02:00
4. Enjoy a factory-new brush head (at least as far as the time counter is concerned)

Here is the breakdown of the command in step 3:

|Command|Explanation|
|--|--|
|1B|PWD_AUTH|
|67:B3:8B:98|The password|
|,|Package delimiter|
|A2|WRITE|
|24|To address 0x24|
|00:00:02:00|Timer set to 0s|

Below you can see the memory of the brush head before and after the custom NFC commands:

{% include gallery id="gallery_nfc_update" caption="Observe how the timer at address 0x24 changes"%}

With this, the toothbrush is now __successfully hacked__ and we can play around with the timer as we wish.

Here are some interesting observations:
- Only the first two bytes at address 0x24 are used for timekeeping.  Once the counter reaches FF:FF:02:00 it stops going up (18 hours of continuous brushing).  
- When the stored time is greater than 0x5460 the toothbrush blinks the LED to notify you to change heads. This corresponds to 21’600s -> 180 x 2min -> 3 months of brushing twice a day, which is exactly in line with Philips recommendation to change heads every 3 months.

## Final Remarks

### Password verification protection
You might have noticed the color of the brush head changing throughout of this post. This is because I had to run out and buy a new one after getting locked out of the first one.  
When having a close look at the contents of address 0x2A which is 43:00:00:00 and [page 18](https://www.nxp.com/docs/en/data-sheet/NTAG213_215_216.pdf#page=18) of the datasheet, we can see that the tag is configured to permanently disable all write access after three wrong password attempts. (Which I promptly exceeded when playing around) This means that not even the toothbrush handle itself can write to this head again.

### Password generation
Unfortunately, the password of every brush head is unique and this process of extracting it with an SDR is quite involved and requires special hardware. 
At the bottom of page 30 in the datasheet, NXP recommends generating the password from the 7-byte UID. Below are all the UID - password pairs I obtained from my 3 heads:

|UID|Password|
|--|--|
|04:79:CF:7A:89:10:90|FF:34:CE:4C|
|04:EC:FC:A2:94:10:90|61:F0:A5:0F|
|04:D7:29:0A:94:10:90|67:B3:8B:98|

All my tries to guess to one-way function for generating the passwords failed. Depending on the care that the Philips engineers took, guessing this function could be almost impossible. 
But if you manage to solve this puzzle, feel free to hit me up with an E-mail.

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-JCFYDY59EG"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-JCFYDY59EG');
</script>