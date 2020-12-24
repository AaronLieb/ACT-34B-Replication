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
 - How to reprogram another remote to send that same data

# Starting out
I started out by analyzing the remote and seeing if I could find any useful information on the remote itself.
After taking a look at the outside, the remote actually states that the frequency is 318 MHz. There was also two numbers numbers on the inside of the remote, 
a 5 digit sticker reading 17728, and a 4 digit sticker reading 0634.

After the easy part was done, I had to find out what information was encoded when sent.
I took a couple hours to read up on any literature I could find on garage door openers, specifically the ACT-34B transmitter.

[Installation manual](https://www.linearproaccess.com/wp-content/uploads/ACT-31B_ACT-34B.pdf)

[promotion literature](https://www.linearproaccess.com/wp-content/uploads/ACT_Family_Compatibility_of_TRANSPROX.pdf)

While doing so, I found out that the transmitter encodes data by using Amplitude Modulation.
Amplitude Modulation encodes information in a wave by switching between two different amplitudes to encode binary data.
The documentation for the ACT-34B only mentions that the format of the data is MegaCode format, which is Linear Corp's proprietary format that can have 1 million unique encoded values. This means that the data could be stored at least in a 20 bit unsigned integer. The documentation for the ACT-34B also lists this model as being "Factory Block Coded with facility and ID codes" meaning that multiple pieces of information are being sent together in blocks, and that the information being sent was determined at the factory and can not be changed. The two pieces of information being sent can be assumed to be the facility and ID code.

After digging around on the internet a bit more I found a useful review on the [Walmart listing of this product](https://www.walmart.com/ip/MegaCode-ACT-34B-Keyfob-Transmitter/170976222), stating that the 5 digit sticker found inside the transmitter is actually a unique 16 bit ID code, which is part of the MegaCode. But we know that the number must be at least 20 bits, so that leaves the remaining 4 bits to be a facility code. The review also mentions that the default facility code is 0, and that there is no way of finding the facility code for sure without having the box that the transmitters were shipped in. However, since the facility code is only 4 bits, there are only 16 (2^4) combinations for the code.

# How to program a remote 
I did some research on how I could program a ACT-34B and I discovered that I would have to use the specific linear corp receiver that the community owns, or use my own linear corp receiver along with a ACT-34C that would allow me to change the facility ID codes. Since I am not sure what receiver the community uses, I would not want to take take this route, as it would also be expensive. There are also not any other remotes I could find that would be able to encode blocked 20 bit code with the amount of customization I would need.

# Creating a remote
Because any remote is just a radio wave with encoded data, I can create my own remote with an arduino nano and tranceiver module. I found some projects online of others creating similar remotes. [1](https://www.youtube.com/watch?v=-BDCmwNssiw) [2](https://www.youtube.com/watch?v=I6TKGMbHcfo)

I'm going to need the following parts for each remote:
 - [Arduino Nano](https://www.amazon.com/LAFVIN-Board-ATmega328P-Micro-Controller-Arduino/dp/B07G99NNXL/ref=sr_1_6?dchild=1&keywords=arduino+nano&qid=1608851423&sr=8-6)
 - [A transceiver module](https://www.amazon.com/MakerFocus-NRF24L01-Transceiver-Antistatic-Compatible/dp/B01IK78PQA/ref=as_li_ss_tl?ie=UTF8&qid=1547396728&sr=8-4&keywords=nrf24l01+pa+lna&linkCode=sl1&tag=howto045-20&linkId=2633b70ca0153dc90317bd6f33b40079&language=en_US)
 - [a power regulator for the transceiver]( https://www.amazon.com/Aideepen-Wireless-Transceiver-NRF24L01-Antenna/dp/B01ICU18XC/ref=as_li_ss_tl?ie=UTF8&qid=1533398618&sr=8-8&keywords=NRF24L01&linkCode=sl1&tag=howto045-20&linkId=6a0d99d36f781543f23f967281c11309)
 - [2 buttons]
 
# Receiving the data
In order to continue, I had to find out what data was being sent from the original remote. I knew what comprised most of the MegaCode, but there were too many unknowns. I was not sure if the 2 blocks of data were all sent at once, or which block of data went first, along with other complications. I would have to use a receiver to read what data was being sent from the original remote. I can actually use the parts I ordered to accomplish this because I had ordered transceivers, which are a combination of transmitters, and receivers. This allows me not only to send data, but also receive it. Since I am ordering two of them, this allows me to test the message being sent from one of the remotes once I create them as well.
