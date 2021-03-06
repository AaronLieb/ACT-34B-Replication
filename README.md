# ACT-34B Replication
This repo is going to be documentation of my attempt to learn how to duplicate the ACT-34B transmitter by Linear Corp.

# The problem
My parents just moved into a community with a front gate. This community only provides two gate openers per household and would not allow my parents to get any more. I'm attempting to replicate the ones they currently have.

# What I knew
I knew that a transmitter of any sort (garage door opener, gate opener) transmit a radio wave at a certain frequency, with data encoded.

# What I didn't know
 - How the data was encoded in the radio wave
 - What frequency the remote was transmitting at
 - How to find what data was being encoded in the wave
 - How to reprogram / create another remote to send that same data

# Research
I started out by analyzing the remote and seeing if I could find any useful information on the remote itself.
After taking a look at the outside, the remote actually states that the frequency is 318 MHz. There was also two numbers on the inside of the remote, 
a 5 digit sticker reading 17728, and a 4 digit sticker reading 0634.

After the easy part was done, I had to find out what information was encoded when sent.
I took a couple hours to read up on any literature I could find on garage door openers, specifically the ACT-34B transmitter.

[Installation manual](https://www.linearproaccess.com/wp-content/uploads/ACT-31B_ACT-34B.pdf)

[promotion literature](https://www.linearproaccess.com/wp-content/uploads/ACT_Family_Compatibility_of_TRANSPROX.pdf)

While doing so, I found out that the transmitter encodes data by using Amplitude Modulation.
Amplitude Modulation encodes information in a wave by switching between two different amplitudes to encode binary data.
The documentation for the ACT-34B only mentions that the format of the data is MegaCode format, which is Linear Corp's proprietary format that can have 1 million unique encoded values. This means that the data could be stored at least in a 20 bit unsigned integer. The documentation for the ACT-34B also lists this model as being "Factory Block Coded with facility and ID codes" meaning that multiple pieces of information are being sent together in blocks, and that the information being sent was determined at the factory and can not be changed. The two pieces of information being sent can be assumed to be the facility and ID code.

After digging around on the internet a bit more I found a useful review on the [Walmart listing of this product](https://www.walmart.com/ip/MegaCode-ACT-34B-Keyfob-Transmitter/170976222), stating that the 5 digit sticker found inside the transmitter is actually a unique 16 bit ID code, which is part of the MegaCode. But we know that the number must be at least 20 bits, so that leaves the remaining 4 bits to be a facility code. The review also mentions that the default facility code is 0, and that there is no way of finding the facility code for sure without having the box that the transmitters were shipped in. However, since the facility code is only 4 bits, there are only 16 (2^4) combinations for the code.

The last piece of information that I found was the most useful information I had found thus far. I was watching a presentation from [AppSec California 2016 by Samy Kamkar](https://www.youtube.com/watch?v=1RipwqJG50c), and he mentions that the FCC requires that all devices that transmit in RF must be registered with the FCC. This means that all the innerworkings of the device must be publically available, including the frequency, what data is being sent, how the data was encoded, and any other specifications like the timing of the data encoding. This was very helpful to me, and I will touch on it more when I get to creating the remote. You can find the information on any RF device by inputting the FCC ID found on the device into [fcc.io](https://fcc.io). Link to the [ACT 34B fcc info](https://fcc.io/EF4ACP00872)

# Programming a remote
I did some research on how I could program a ACT-34B and I discovered that I would have to use the specific linear corp receiver that the community owns, or use my own linear corp receiver along with a ACT-34C that would allow me to change the facility and ID codes. Since I am not sure what receiver the community uses, I would not want to take take this route, as it would also be expensive. There are also not any other remotes I could find that would be able to encode blocked 20 bit code with the amount of customization I would need.

# Creating a remote
Because any remote is just a radio wave with encoded data, I can create my own remote with an arduino nano and tranceiver module. I found some projects online of others creating similar remotes. [1](https://www.youtube.com/watch?v=-BDCmwNssiw) [2](https://www.youtube.com/watch?v=I6TKGMbHcfo)

These projects helped a lot with understanding what I would be doing in the future, but I am going to need a different transmitter, because the ones used in those examples are both 2.4G Ghz. In the [Samy Kamkar video](https://www.youtube.com/watch?v=1RipwqJG50c), he mentions the term, 'sub-1Ghz transmitter' which are small, low voltage transmitters that can transmit a range of frequencies under 1 Ghz. One of these would be perfect for what I would need, and I quickly found one online that ranges from 240 to 480 MHz. The transmitter requires an input voltage of 1.8 to 3.6V, so I needed a way to regulate the voltage for the transmitter. After taking a look at a diagram for the arduino nano, it turns it has a digial output pin that can output at 3.3V, which eliminates the need for me to use a voltage regulator. As for the rest of the parts, they are pretty standard for an arduino project like this.

I'm going to need the following parts for the remotes:
 - [Arduino Nano](https://www.amazon.com/LAFVIN-Board-ATmega328P-Micro-Controller-Arduino/dp/B07G99NNXL/ref=sr_1_6?dchild=1&keywords=arduino+nano&qid=1608851423&sr=8-6)
 - [Sub-1Ghz transmitter](http://www.hoperf.com/modules/rf_transmitter/RFM110.html) [RFM110W](https://www.digikey.com/en/products/detail/rf-solutions/RFM110W-433S1/6564916)
 - [9V battery connector](https://www.banggood.com/5Pcs-175mm-9V-T-Type-I-Type-Battery-Buckle-Connector-Snap-Clip-Lead-Cable-p-1313739.html?utm_source=google&utm_medium=organic&utm_content=-&utm_campaign=none_pps_copy&_branch_match_id=853025810499564735&cur_warehouse=CN&ID=554248)
 - [momentary buttons](https://www.amazon.com/Gikfun-12x12x7-3-Tactile-Momentary-Arduino/dp/B01E38OS7K/ref=sr_1_6?dchild=1&keywords=tactile+buttons&qid=1608856496&sr=8-6)
 
# Receiving the data
In order to continue, I had to find out what data was being sent from the original remote. I knew what comprised most of the MegaCode, but there were too many unknowns. I was not sure which block of data went first, along with a couple other of small details. I would have to use a receiver to read what data was being sent from the original remote. I decided to order a [RTL-SDR](https://www.amazon.com/NooElec-NESDR-Smart-Bundle-R820T2-Based/dp/B01GDN1T4S/), which is a RealTek Software Defined Radio, which allows me to receive radio waves on a large range of frequencies, and view them on software. This, combined with the information from the FCC tests would allow me to get a precise reading of the data the transmitter was encoding

# Updates
I am currently waiting on the parts to be delivered. I will update this document as the project progresses!
