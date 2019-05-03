# COMP28512 - Mobile Systems
## Exercise 5 - Communicating with Android Smartphones
> May 2019
>
> Javier Pacheco Rodriguez

- - -

### Part 5.1 [4 marks]
Modify the application you developed for the previous Task such that it
converts a short one-line text-message to a bit-string and sends it to
`BarryBot` using `<mode>MSG`.
The app should allow you to specify the mode. Initially specify mode 0 so that
the simulated return channel should not introduce any bit-errors. Convert the
received bit-string back to a text-string and display it on the emulated or
real smartphone screen.
Send your message several times to barryBot using modes 1, then 2, then 3.
Comment briefly on what you receive back.
Note that the simulated return channel only ever changes ‘0’ to ‘1’ or
vice-versa.


#### 1. Explain and demonstrate the code for the conversion and re-conversion.

```java
// Get the send button as defined in the Res dir xml code
Button sendButton1 = findViewById(R.id.MSG1);
// Make the send button receptive to being clicked
// Button click handler
sendButton1.setOnClickListener(new View.OnClickListener() {
    // onClick method implementation for the sendButton object
    public void onClick(View v) {
        // OnClick actions here
        // Instantiate the transmitter passing the output stream and text to it
        EditText editText = (EditText)findViewById(R.id.editText);
        String editTextStr = editText.getText().toString();
        String arr[] = editTextStr.split(" ", 2);
        String name = arr[0];   //usr
        String text = arr[1];   //msg
        char[] textArray = text.toCharArray();
        int[] textArrayInBytes = new int[textArray.length];
        /*Loop: for 'index', while 'index < textArray.length()', in steps of 'index++'*/
        for (int index = 0; index < textArray.length; index++)
        {
            textArrayInBytes[index] = textArray[index];
        } //for

        String textBytesToSend = IntA2bitS(textArrayInBytes, text.length());

        transmitterText = "1MSG " + name + " " + textBytesToSend + "MSG";
        Log.d(LOGTAG, "onClick: Message received from the textbox");
        if (networkConnectionAndReceiver.getStreamOut() != null)
        { // Check that output stream has be setup
            Transmitter transmitter = new Transmitter(networkConnectionAndReceiver.getStreamOut(), transmitterText);
            transmitter.start(); // Run on its own thread
        }
    }
});
```
*Figure 1: `MainActivity` implementation.*

The modifications are stripping the part of the string that belongs to the actual text message and not the username, then converting that string to an array of chars, then to an array of ints and then to an array of `16bit` integers that represent the value of those chars. And those are finally sent as a String to the server.

For illustration:

    "AB...C"
    ['A', 'B', ..., "C"]
    [26, 27, ..., 28]
    [1, 1, 0, 1, 0, 1, 1, 0, 1, ..., 1]
    "110101101...1"


```java
String notMessageContent = message.substring(0, message.lastIndexOf(" ") + 1);
String actualMessageContent = message.substring(message.lastIndexOf(" ") + 1, message.length() - 3);
int[] messageBits = new int[actualMessageContent.length() / 16];
bitS2IntA(actualMessageContent, actualMessageContent.length() / 16, messageBits);
char[] messageArray = new char[messageBits.length];
/*Loop: for 'index', while 'index < textArray.length()', in steps of 'index++'*/
for (int index = 0; index < messageBits.length; index++) {
    messageArray[index] = (char) messageBits[index];
} //for
String text = new String(messageArray);
message = notMessageContent + text;
// If the message has text then display it
if (message != null && message != "") {
    Log.i(LOGTAG, "MSG recv : " + message);
    Log.i(LOGTAG, "Actual MSG recv ' " + text + "'");
    //Get the receiving text area as defined in the Res dir xml code
    receiverDisplay = parentRef.findViewById(R.id.txtServerResponse);
    //Run code in run() method on UI thread
    parentRef.runOnUiThread(new Runnable() {
        @Override
        public void run() {
            // Display message, and old text in the receiving text area
            receiverDisplay.setText(message + "\n" + receiverDisplay.getText());
        }
    });
}
```
*Figure 2: `NetworkConnectionAndReceiver` implementation.*

In the receiver, as we aren't receiving the messages in text, but in its byte representation, we have to reverse the operation that we did earlier. However, this time there may have been bit errors that alter some of those binary bits and produce a totally different character.

For illustration:

    "110101111...1"
    [1, 1, 0, 1, 1, 1, 1, 0, 1, ..., 1]
    [26, 26, ..., 28]
    ['A', 'A', ..., "C"]
    "AA...C"

Real messages being transmitted really look like this:

    [21:24:25] '2MSG BarryBot 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:25] '0MSG Javier BarryBot (via channel): 101100100000000010100110000000000100111000000100010011100000000010011110000000000000010000000
    [21:24:26] '3MSG BarryBot 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:26] '0MSG Javier BarryBot (via channel): 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:31] '1MSG BarryBot 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:31] '0MSG Javier BarryBot (via channel): 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:32] '2MSG BarryBot 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:32] '0MSG Javier BarryBot (via channel): 101100100000000010100110000000000100111000000000010011100000000010011110000000010000010000000
    [21:24:34] '3MSG BarryBot 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:34] '0MSG Javier BarryBot (via channel): 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:36] '1MSG BarryBot 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:36] '0MSG Javier BarryBot (via channel): 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:37] '2MSG BarryBot 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:37] '0MSG Javier BarryBot (via channel): 101100100000000010100110000000000100110000000000010011100000000010011110000000000000010010000
    [21:24:38] '3MSG BarryBot 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
    [21:24:38] '0MSG Javier BarryBot (via channel): 101100100000000010100110000000000100111000000000010011100000000010011110000000000000010000000
*Figure 3: Terminal server log.*


#### 2. How did you enter the message and did you get back the same message exactly?
The message is sent through the textbox, obtained in the code through the listener and then processed and sent to the server.
Because it is being sent to BarryBot we can see the resulting string directly from the app. Using mode `0MSG` we always get the same string we entered, however with the other modes there is some chance of it having bit errors. 
More on that discussed later.

#### 3. Although the simulated channel should not deliberately introduce any bit-errors in mode 0, what would happen if a real bit error occurred due, perhaps, to a malfunction of the mobile phone? How would your program deal with this occurrence?
If this bit error occurred, it could happen anywhere in the string, not specifically in the message substring, so it may even send the message to another user or change the mode. However, this is unlikely to happen, but we could implement a CRC check to identify bit errors should they ever happen.

#### 4. How does this form of transmission increase the required bit-rate over the simulated channel?
We are wasting otherwise useful bytes by sending plain ones and zeros as characters over the communication with the server. As the connection supports a wide range of characters, most likely `2^16`, and we are only sending two `2^1` possible values, the rest of allocated space `15 bits` per character is wasted and could be optimised by sending actual ones or zeros and not their character representation. Also bit errors change the characters to other values from the one or zero, and we wouldn't want this to happen in a real application.

#### 5. Why is this form of transmission useful for experimental purposes?
Because it is still employing single bits and, thus, being able to see which are affected by errors and which not.

#### 6. What are your brief observations about the effect of bit-errors?
From the different bit-error generators `0MSG`, `1MSG`, `2MSG` and `3MSG` they generate them with different methods.
  * `0MSG`: None at all.
  * `1MSG`: Eventual errors over multiple messages and in most cases only once in each for reasonably short messages.
  * `2MSG`: More frequent errors and some affect multiple characters because they occur in small bursts.
  * `3MSG`: Many errors occur, affecting multiple characters and effectively making the message more unreadable that the previous options.

However, so far we've only worked with strings. In other forms of media, depending on whether compressed or not, they may affect the original transmission so much that it corrupts completely (different colours, horrible sound, ...).

#### 7. How realistic is it to assume that bit-errors will normally be evenly spread out in time?
Not realistic in a world where we know noise and influence from other transmissions can happen at any time, randomly. However we can agree that, as seen, they happen over bursts and if one bit is affected the closest ones could also be affected too.

### Part 5.2 [4 marks]
Investigate the `ENCRYPT` special command sent to `BarryBot` as detailed in Section 4 above.
Send an `ENCRYPT` message repeatedly to `BarryBot` using `0MSG` on Port 9999. Use mode 0 to avoid any bit-errors.
The encrypted messages arrive in bit-string form.
If they are converted to ordinary text string form you will not be able to read them.
Process the bit-string responses to determine (crack) the encryption key.
Then show that you can decipher all subsequent secret messages until `BarryBot` is restarted and chooses a new key.
Remember that if messages `A1` and `A2` are encrypted by the same key `K` to obtain `S1 = A1 XOR K` and `S2 = A2 XOR K`, where XOR is *exclusive or*, then:
`S1XOR S2 = (A1 XOR K) XOR (A2 XOR K) = (A1 XOR A2) XOR (K XOR K) = A1 XOR A2`.
If you have some way of knowing or guessing message `A1` say, then you can get `A2` as `(A1 XOR A2) XOR A1`.
You will know when you have guessed `A1` correctly when you can read message `A2`.
If you know `A2`, since you have `S2`, you can get the key `K` and decipher all subsequent messages.

Comments:
In this part we figure out a way to decrypt messages, provided they have the same key and one of them repeats over time. With some thought we realise that the signature being added every once in a while is `"Sent by BarryBot, School of Computer Science, the University of Manchester"`. We want to find the key that produced the encrypted message so we can inverse it to decrypt every other message.
We take two messages, and apply the XOR operation to them. If we use this result with the guessed signature, then we have successfully applied the key to the message we didn't know and find what it decrypts into. We can do the same with the remaining messages, or we can find the actual key value that will decrypt all of them.

#### 1. What were the messages?
  * `"Sent by BarryBot, School of Computer Science, The University of Manchester"`
  * `"Government's export restrictions on cryptographic technology limited the key size. Once the restrictions "`
  * `"ifted, manufacturers of access points implemented an extended 128-bit WEP"`

*It only makes sense that lifted is what links the sentences.*

#### 2. Assuming this experiment demonstrates that using the same key more than once is dangerous, how could you avoid this danger while still allowing the receiver to decrypt your messages?
There are many strategies for achieving this. One that I like and is common nowadays is establishing a double ratchet encryption, like WhatsApp (explained here: [https://youtu.be/9sO2qdTci-s](https://youtu.be/9sO2qdTci-s)) that derives new keys for every new message that don't have any relationship with the previous keys so any attacker that figures out one key cannot use it to read previous or new messages because they wouldn't know how it was created or what the secret input to the key derivation was to create the key for the next message.
A good way of exchanging keys is using a Diffie-Hellman strategy: [https://youtu.be/NmM9HA2MQGI](https://youtu.be/NmM9HA2MQGI).

#### 3. Briefly summarize your decryption method and indicate whether it really worked.
I'll only add that the method I used did work because I was able to crack the key that could be used to decrypt any message sent by BarryBot. The following is a copy of the above:

With some thought we realise that the signature being added every once in a while is `"Sent by BarryBot, School of Computer Science, the University of Manchester"`. We want to find the key that produced the encrypted message so we can inverse it to decrypt every other message.
We take two messages, and apply the XOR operation to them. If we use this result with the guessed signature, then we have successfully applied the key to the message we didn't know and find what it decrypts into. We can do the same with the remaining messages, or we can find the actual key value that will decrypt all of them.

#### 4. If you could not predict that an email signature will be sent regularly, what other weaknesses could make the encryption vulnerable to attack.
One I remember trying when I was younger for a maths problem was applying to know frequencies on natural English characters over a large text to figure out which symbols would correspond to which letters. However using a decent encryption we realise that all characters have an influence over the next ones as they are not individual any more.
A different strategy would be to try an catch the keys when they are being exchanged by the users. Again, if a decent double ratchet method is in place, this would only allow us to see one message, making this not effective.
And there's always the good old brute force that can make some sense to the encrypted text by iteration through all possible keys, most likely infeasible any way.

### Part 5.3 [4 marks]
Download a speech or music file from Blackboard and store it in `res/raw` as explained in Section 5 above.
Introduce a `stop` button as well as the `start` button for a sound player and check that the sound plays correctly.
Then download the `512x512x3` image files `peppers.bmp` and `peppers.jpg` to `res/raw` and allow the user to display either of these files.
Compare them.
To investigate how bit-errors would affect these images, you could transmit them to barryBot.
However, to save time, this has already been done for you.
The images damaged by bit-errors, were stored in a `bit-map` file called `damagedpeppers.bmp` and a `JPEG` file called `damagedpeppers.jpg`.
These files are provided on Blackboard.
The damage was caused by (a) bit-errors and (b) lost packets.
The packet size was `256 x 3 = 786 bytes` and packets start at sample `0`, `786`, `2x786`, `3x786`, etc.
Each lost packet was replaced by a sequence of `786` consecutive zero-valued bytes.
Bit-errors can occur anywhere except in lost packets.
Download the damaged `bmp` and `JPEG` files into the `raw` directory on your mobile phone, display them and observe the effect of the bit-errors.
The bit-error probability for `damagedpeppers.bmp` was `0.05` (which is 5%) whereas for `damagedpeppers.jpg`, it was `10^-4` (which is 0.01 %).
Increasing the bit-error rate closer to `1%` for `jpg` images made the images totally unreadable.
The techniques presented in this Task would allow you to process these images to hide some of the effects of bit-errors and lost packets, but this is not required for the marks.
You just need some ideas about what you would do.

#### 1. What happens to your sound player if you keep pressing the `start` button?
Pressing the start button toggles between `playing` | `paused` states. I avoided the application from crashing using the following simple code inside `onClick()`:


```java
// PLAY button code
public void onClick(View v)
{
  if (mySound.isPlaying())
  {
    mySound.pause();
  }
  else
  {
    mySound.start();
  }
  // more code
}

// STOP button code
public void onClick(View v)
{
  mySound.seekTo(0);
  mySound.pause();
}
```


#### 2. Why does a `wav` file have a `44 byte` header, and how are the sound samples themselves stored? Are the samples stored in text form?
The header, as in most formats that require, provides information about how to read the rest of the file. It contains metadata such as the sampling ratio, the number of samples and the bitrate. This data is necessary to be able to play the audio contained with the desired initial configuration.
The samples are saved as 16-bit integer values that represent the pulse code modulation (PCM) value of the original analogical signal that is then recreated when emitting the sound again.

#### 3. How did you display the original undamaged images?
With a new `ImageView` in the layout that references the location of the resource, in this case, the undamaged images.

Here is the relevant part of the `onCreate()` method used for images and audio:

```java
mySound = create(this, R.raw.damagedspeech);
//Get image display area as set in the xml file//
ImageView image = (ImageView) findViewById(R.id.imageView1);
//Display peppers.bmp from res/raw directory//
image.setImageResource(R.drawable.peppers);
//Call msImage method to process the image:
msImage("peppers");
```

#### 4. Did you detect any difference between the `bitmap` image and the `jpeg` version?
Not in the resulting image, but in the file itself, as the `bmp` format does not have compression and pixel values are stored one after the other in order that can be easily read by a human. On the contrary, the `jpeg` format compresses the pixels most commonly lossy, which means some information is lost and we cannot revert to the original, and can be done repeated times to achieve higher and higher compressions.

#### 5. What was the effect of the bit-errors and lost packets on the `bitmap` image?
In a pixel by pixel format bit-errors affect how the pixels in the image are (their value, brightness, alpha, etcetera). If the frequency of errors is high enough, some random pixels will be randomly coloured, but most probably the image will still be fine, even if they occur in big bursts, unless every pixel has a lot of significance to the image which is not that common.

Missing packets will cause missing pixels appear in the image, as we don't have the information about the pixels that were lost. In this case it isn't that it was corrupted and we should not invent our own replacement pixels.

#### 6. How could you conceal the distortion caused by these transmission-errors (if you had time)?
If possible, sending the pixel information alongside a CRC check and request those that don't match because either the check or the pixel would have suffered a bit error. And requesting again the packets that haven't been received. Both options have to work very well with the format and would only work provided we are able to understand information about pixels individually, which we can in bit maps.

#### 7. What was the effect of the transmission errors on the `JPEG` image?
Because the `jpeg` image is compressed, the significance of every bit is higher, thus the effect of bit errors or lost packets can be catastrophic. From altering the symbol used to represent a pixel value in the huffman table to affecting where it should have ended/started the effect can cause that all pixels from the one that is wrong will appear wrong too. This is why we see large chunks of affected pixels, when really, the cause could have ben a single bit error. It is always crucial to avoid compressed data from suffering errors.

#### 8. Why are `jpg` images much more sensitive to bit-errors and lost packets than `bmp` images?
I've just answered that above. In case it is not clear, it is because with compressed data, the significance of every bit increases compared to the original data as it carries information about symbol positions (start and end) and has to be translated back using a huffman table if it was converted this way. Also, imagine the effect that bit errors occurring on the table itself would mean.
There is also the fact that bitmaps allow us to know information about individual pixels, so if that information changes, it wouldn't affect other pixels unless it spread over to their information.

### Part 5.4 [4 marks]
A recording of monophonic narrowband speech file sampled at `8 kHz` with `16 bits` per sample has been stored in a wav file called `damagedspeech.wav` and is provided on Blackboard.
The damage was caused by (a) bit-errors and (b) lost packets.
The packet size was `200 samples` and packets start at sample `0`, `200`, `400`, `600`, etc.
Each lost packet was replaced by a sequence of `200` consecutive zero-valued `16-bit samples`.
Bit-errors can occur anywhere except in lost packets.
Download the wav file into your mobile phone and play it out using `AudioPlayer` as in Section 5.1.
Then read the file into a Java array and play it out again using `AudioTrack`. Ignore the header information stored in the first `44 bytes` of the `wav` file.
Develop Android code to recognize and deal with the bit-errors and lost packets in the speech transmission.
In this experiment, there are no `CRC checks`, therefore you have to find a way of identifying some of the more serious *spikes* caused by bit errors and smoothing them over.
The simplest way of dealing with packet loss is to replace each lost packet with a copy of the previous correct packet.

#### 1. How does the receiver know when a packet has been lost in these experiments, and how does the mechanism provided differ from that used by the real time transport protocol RTP?
If it knows the number of packages that should have been sent and it is not the amount that it has received, then there is definitely the difference lost (or in excess). I imagine that we are simulating lost packets by making them be all `0`.
RTP packets all carry information about the time they were sent, perhaps its order too, but essentially, just with the timestamp it should be possible to know when and which are likely to have been lost ans request them again.

#### 2. How successful is your simple method of dealing with lost packets in speech transmissions?
Replacing the holes created by the lost packets with zeros, creates those clicks in the audio that are produced by having non matching waves for in instant or silence during microseconds.

#### 3. How successful is your simple method of dealing with bit-errors in speech transmissions?
Smoothing the pairs that have a difference of some threshold reduces the effect of the bit errors, but doesn't eliminate them and affects how quick sounds or high frequencies are heard back.
A possibility could have been to implement a CRC check for packets, or multiple in every packet (we would just have to agree on this) and asking for retransmission in case of it failing. Any number of times, a limit or enough to check the majority vote.

### Part 5.5 [4 marks]
Complete the microphone application detailed in Section 5.4 and implement it on an Android Virtual Device (AVD) or a real smartphone or tablet.
Send a short narrowband voice recording (with `Fs = 8000 Hz`) to `BarryBot` and demonstrate the effect of the five possible bit-error rate modes.
Finally, play back the `8 kHz` sampled sound with a `12 kHz` sampling rate to observe the effect.

#### 1. What was, or could be, the effect of the design flaw?
The flaw is that the main thread is being held by the recording process and cannot be interacted with until it stops. No pausing the recording, by sliding the finger, for instance, nor doing anything else is possible. In fact the app appears to be in a frozen state.

#### 2. How could you correct the design flaw?
A naive option is to run both the main thread and the recording at the same time in the same loop, by switching from one to another in every iteration, and a more sophisticated way would be to allow the recording to have its own thread, or appear to, at least.

#### 3. Demonstrate the effect of the bit-errors and packet-loss in modes 0 to 5.
As more and more errors occur in the transmission, more and more difference and nastier the resulting sound is. Affecting both clicks and out of tone sounds and muffled parts.

#### 4. What was the effect of increasing the sampling rate to `12 kHz` on play-back?
It is ignorant to do this without changing the number of samples, as it just plays the same amount of samples quicker, thus increasing the pitch of the sound and making it sound like something out of *alvin and the chipmunks*.

#### 5. Why is it useful to be able to develop your own application for speech recording or processing, rather than relying on downloaded apps. Suggest a useful application that you now know how to develop.
With programming you unlock endless possibilities. If you have you own ideas, you can turn them into applications that will automate them for you, for example.
On the contrary, there exist a wide range of open source multimedia applications that are transparent, easy to use, modify and extend that, with the right knowledge, one could use to research different ideas on audio, video, or plain image. In fact this makes for good third year projects (such as the magic image filter, that can find a way to improve an image (presumably by having a large set of pairs of image and improved image)), but a simpler an more appropriate to the understanding we have of these topics could be to make a simple `Audacity`, `paint`, or combine them into video. If we include the communications part, which is also very important, then how to interactively modify and process multimedia is an idea. Again, endless possibilities :).
