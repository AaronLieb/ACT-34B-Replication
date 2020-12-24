# ACT-34B Duplication
This repo is going to be documentation of my attempt to learn how to duplicate the ACT-34B transmitter by Linear Corp.

# The problem
My parents just moved into a community with a front gate. This community only provides two gate openers per household and would not allow my parents to get any more. I'm attempting to replicate the ones they currently have.

# What I knew going in
I knew that a transmitter of any sort (garage door opener, gate opener) transmit a radio wave at a certain frequency, with data encoded.

# What I didn't know
 - How the data was encoded in the radio wave
 - What frequency the remote was transmitting at
 - How to find what data was being encoded in the wave
 - How to reprogram another remote to send that same data

# Starting out
I started out by analyzing the remote and seeing if I could find any useful information on the remote itself.
After taking a look at the outside, the remote actually states that the frequency is 318 MHz. 

After the easy part was done, I had to find out what how I was going to find out what information was encoded when sent.
I took a couple hours to read up on any literature I could find on garage door openers, specifically the ACT-34B transmitter. 
(Installation manual)[https://www.linearproaccess.com/wp-content/uploads/ACT-31B_ACT-34B.pdf] and (promotion literature)[https://www.linearproaccess.com/wp-content/uploads/ACT_Family_Compatibility_of_TRANSPROX.pdf]

While doing so, I found out that the transmitter encodes data by using Amplitude Modulation.
Amplitude Modulation encodes information in a wave by switching between two different amplitudes to encode binary data.
The documentation for the ACT-34B only mentions that the format of the data is MegaCode format, which is Linear Corp's proprietary format that can have 1 million unique encoded values. This means that the data could be stored at least in a 20 bit unsigned integer. The data 
