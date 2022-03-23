## Jeale's CTF Writeup
### allinone [challenge]
Hello! This is the writeup for the Jeal CTF, let's jump straight into it!<br>
<br><img width="443" alt="image" src="https://user-images.githubusercontent.com/79892065/159476078-fd5c6711-9124-4aed-b470-58a9db9ef09b.png">
<br>Going to https://jeale.ml/ and reading the rules, We know that the flag format is CTF{}, and there are 3 flags in the website.
<br>So let's head off to the challenge section. Once we clicked on it, it redirects us to challenge.html.<br>
<br><img width="892" alt="image" src="https://user-images.githubusercontent.com/79892065/159476255-a0835576-6e2c-47fe-b178-9ad5f93a57cf.png">
<br>So this must be the challenge description
<br>After we clicked "Start from here", we were redirected to /ctf/home.
<br>So let's try to curl /ctf and see if there's anything interesting
<br>After we curl /ctf, we got this<br>
``<!--Jeale: Phew, nobody can see this. Do you know what is steganography?-->``
<br>Looks like there's something to do with steganography, keep that in mind

* Hints
    * Steganography

<br>Alright, let's move back to the website<br>
<br>We are greeted with a login page, checking the source code it looks like it's using StatiCrypt to encrypt the website<br>
<br><img width="735" alt="image" src="https://user-images.githubusercontent.com/79892065/159641014-7f035ae6-f3de-4f62-9cec-0502e06bef5c.png">
<br>There's a C program called "Login C Program", let's check it out<br>
<br><img width="498" alt="image" src="https://user-images.githubusercontent.com/79892065/159641137-b2c429e0-04d8-4f56-82a1-dd5eeafb3b7e.png">
<br>This is easy... Just do some maths 
<br>(4719 + 821) / 2 = 2770
<br>So we got the password to login to the website, now we see this<br>
<br><img width="943" alt="image" src="https://user-images.githubusercontent.com/79892065/159477088-365b2050-de70-4ddc-a0b9-e066239cfaa2.png">
<br>Hm, right click, f12, CTRL+SHIFT+I and CTRL+SHIFT+J is disabled, but we can still view the source using view-source:
<br>This is a StatiCrypt obfuscated html code so we can't view the source code but there's something in there...
<br><img width="439" alt="image" src="https://user-images.githubusercontent.com/79892065/159641527-faa50208-ad06-4da6-990a-177f21ada9ea.png">
<br>``<!-- Source is at /ctf/home/source :> -->``
<br>Let's go to /ctf/home/source and we see the source code!<br>
<br><img width="752" alt="image" src="https://user-images.githubusercontent.com/79892065/159641643-8ce7f2b0-226b-4ee4-9c14-97e4822fd967.png">
<br>Looking into the source code we find /ctf/style.css, maybe there's something in there?<br>
<br><img width="405" alt="image" src="https://user-images.githubusercontent.com/79892065/159477654-84cfba4c-5f9f-4b10-9805-f09e2cd70bfb.png">
<br>Favicon.... Maybe that's something related to steganography. And indeed there is a favicon.jpg file on their website<br>
<br><img width="537" alt="image" src="https://user-images.githubusercontent.com/79892065/159478555-0a4ca0ad-5fec-410a-a63d-48268faf330a.png">

* Hints
    * Steganography
        * favicon.jpg

<br><img width="639" alt="image" src="https://user-images.githubusercontent.com/79892065/159642212-6835613f-55ca-4d1f-bd53-e800e39a0d72.png">
<br>Looking deeper into the source code, there is a hint telling us to stop right there. So let's stop looking down to the source code.<br><br>
<br>The source code we just looked into is the /home, why not we try to look into the /ctf/about, /ctf/testimonials/ and /ctf/contacts/ since their source code is not obfuscated
<br>All of them says that "Coming Soon' and a number showing the days left<br>
<br><img width="711" alt="image" src="https://user-images.githubusercontent.com/79892065/159479389-40ad7952-a68b-49f4-9042-1001f32b55d0.png">
<br>Inspect element is also disabled so let's try looking into the source code
<br>In /ctf/about we see
<br>``<!-- I remember Jeale names her folder with some numbers and hides it after the page url, and each of the numbers are different every time. Where does these numbers come from?-->``
<br>Damn, remember the numbers we got earlier that says "" days left? We can do something like https://[url]/about/[number]
<br>We can also look into /ctf/testimonials/ and /ctf/contacts and try like https://[url]/testinomails/[number] and https://[url]/contacts/[number]
<br>Remember that the numbers are different so change different numbers in different pages would get you the third flag!

* Flag
    * Third flag acquired
* Hints
    * Steganography
        * favicon.jpg

<br>So now our goal is to get the first and the second flag.
<br>Let's try checking the robots.txt in /ctf/robots.txt and we see two disallows<br>
<br><img width="658" alt="image" src="https://user-images.githubusercontent.com/79892065/159481578-a0dc2877-bc95-48aa-8bc8-0a013cc30189.png">
<br>Hm, /jeales_vigenere.html and /W4ht_a3di0_1s_th1s.html
<br>Let's visit /jeales_vigenere.html first on /ctf/jeales_vigenere.html
<br>We get:<br> ``cipher = Vcsft xrdogu xh wej AOD r ztgdy pkf, klg jebs: "Hwdhc, Z'yq faxsie ec yzjb eakkscig ul s biqc xmnv gtalwv s1t1_g@veu0jlS." M ewif psng hwdl hf kqjh gt jzfh jvv pxfa hwgjzapv, jairki...
encodedkey = VFZSRk0xbFVWWGxOUjBacldXMVJlRTlYVm0xYWFsVjRUVlJCZDAxcVJURlpWMFV6V1ZSa2JWbHRXVDA9``
<br>As we can see there is a cipher text and an encodedkey, so you need to decipher the cipher text using the key
<br>This is a vigenere cipher according to /jeales_vigenere.html
<br>Opening cyberchef and decode the key with base64 we get a hash<br>
<br><img width="761" alt="image" src="https://user-images.githubusercontent.com/79892065/159482474-0709df60-170d-4205-9774-849a46bd34bb.png">
<br>Googling the hash we get to know that the key is "mysupersecretpassword"
<br>So since it is vigenere cipher we can decipher it on cyberchef<br>
<br><img width="767" alt="image" src="https://user-images.githubusercontent.com/79892065/159483063-b67b595d-6c0c-4eac-b734-097edc7a7a71.png">
<br>We decipherd it!
<br>``Jeale talked to her ISP a while ago, she said: "Hello, I've hidden my wifi password in a html file called w1f1_p@ssw0rrD." I need your help to help me find her wifi password, please...``
<br>Aight, let's go to /ctf/w1f1_p@ssw0rrD.html<br>
<br><img width="954" alt="image" src="https://user-images.githubusercontent.com/79892065/159483338-71a77cc0-b40e-482a-b37a-6356c5085f7a.png">
<br>This looks like rot13 so let's decipher it<br>
<br><img width="767" alt="image" src="https://user-images.githubusercontent.com/79892065/159483434-74da27c8-7c0c-4b29-98ab-99f2c9aea9b8.png">
<br>``Jeale t01d m3 th@t sh3 u3e h3r p@s$w0rD m0st1y on 1mag3s 3nd n3tw0rk1ng stU77, b4t sh3 ha3 f0rgo773n it n0w. Sh3 t0ld m3 to f1nd it b3fore the w3bs1te is f1nish3d,or 3lse...... Anyw@y3, can y0u h3lp m3 f1nd 1t?``
<br>0_0 it says that Jeale uses her password mostly on images and networking
<br>So maybe the password could be the password to solve the steganography image?

* Flag
    * Third flag acquired
* Hints
    * Steganography
        * favicon.jpg
            * wifi_password
           
<br>Downloading the wifi .cap file we can try to brute force it with rockyou.txt :>
<br>To make sure it's a handshake file run eapol on wireshark and yes it is!<br>
<br><img width="430" alt="image" src="https://user-images.githubusercontent.com/79892065/159484318-b91a22d0-891a-4635-a4f5-66319f3639c1.png">
<br>The source code of /ctf/w1f1_p@ssw0rrd also says this
<br>``<!--Is wifi password crackable?-->``
<br>Running the code ``aircrack-ng jeale_s4cret_n4tw0rk_data.cap -w /usr/share/wordlists/rockyou.txt`` on Kali Linux and it would take a while to brute force with rockyou.txt so be patient :S<br>
<br><img width="699" alt="image" src="https://user-images.githubusercontent.com/79892065/159494753-552bbbdc-0ce7-44d9-bc4d-3a042ad8ff0c.png">
<br>We got it! It took nearly 1 hour but it was worth it XD.
<br>So the password is "imsohandsome"

* Flag
    * Third flag acquired
* Hints
    * Steganography
        * favicon.jpg
            * imsohandsome

<br>Now, let's check on another html file /ctf/W4ht_a3di0_1s_th1s.html<br>
<br><img width="903" alt="image" src="https://user-images.githubusercontent.com/79892065/159495211-858fb0b0-1f47-403c-92e2-0fb7e86522d6.png">
<br>``w6JP xVG6 DE@=6? D@>6 2F5:@ 7:=6D 7C@> y62=6VD 4@>AFE6C 3FE x 92G6 23D@=FE6=J ?@ 4=F6 23@FE :E[ 42? J@F 96=A >6 @FEn``
<br>Hm, checking the source code it says ``<!--Jeale loves many numbers, one of them is 47-->``
<br>ROT47 is the only solution so let's decipher the message
<br><img width="766" alt="image" src="https://user-images.githubusercontent.com/79892065/159495488-3970bdb5-b205-4c02-8960-1d2271bb9164.png">
<br>``Hey! I've stolen some audio files from Jeale's computer but I have absolutely no clue about it, can you help me out?``
<br>Just a description... Let's download the audio files and analyze them
<br> Maybe its audio spectrogram? <br>
<br><img width="316" alt="image" src="https://user-images.githubusercontent.com/79892065/159496205-36c2acd7-d698-469f-81ac-45f2eacdbf92.png"><br>
<br><img width="317" alt="image" src="https://user-images.githubusercontent.com/79892065/159496261-261f214c-15a8-4824-bb7b-f3e129323ba9.png"><br>
<br><img width="320" alt="image" src="https://user-images.githubusercontent.com/79892065/159496307-62b7da83-c7ea-4173-9e54-a2f73793c9a9.png">
<br>We got 6 hashes! Let's write it down to a text editor
<br>``b27cac074cc5c17f83c7c9e8071002e3
639bae9ac6b3e1a84cebb7b403297b79
383c9968e79cb9a21d3cdde052a86acf
8bf8854bebe108183caeb845c7676ae4
5292c527e0e77b38813bc63bdac7ddd5
d1457b72c3fb323a2671125aef3eab5d``
<br>Using https://crackstation.net we got the plain text!<br>
<br><img width="788" alt="image" src="https://user-images.githubusercontent.com/79892065/159497245-2ebef95a-b4fc-40e3-b950-ea952c3a6be4.png">
<br>``Have you heard of steghide?`` tells us that we can use steghide to extract the data with the password ``imsohandsome``!

* Flag
    * Third flag acquired
* Hints
    * Steganography
        * favicon.jpg
            * imsohandsome

<br>Let's download the favicon.jpg using wget in Kali Linux
<br>Using the command ``steghide extract -sf favicon.jpg`` <br>
<br><img width="392" alt="image" src="https://user-images.githubusercontent.com/79892065/159498011-7901fb8c-c594-4bef-a2d7-30e5601e9641.png">
<br>Entering passphrase "imsohandsome" we extracted a textfile called "jeale.txt"!
<br>Reading the content inside jeale.txt we get<br>
<br><img width="945" alt="image" src="https://user-images.githubusercontent.com/79892065/159500688-d90882fc-36ae-43a9-af60-23ad27eea937.png">
<br>``//Jung vf ornyr pvcure?``<br>
``//9 2 5 12 9 5 22 5 10 5 1 12 5 9 19 1 13 1 12 1 25 19 9 1 14``<br>
``//Have you ever heard of a chef named cyber?``<br>
``1 41 418 125 565 102 95 403 150 304 403 34 46 177 34 125 477 477 403 565 34 403 443 177 403 7 443 403 41``<br>
``477 46 41 46 125 565 177 125 477 403 46 34 41 380 304 142 125 304 403 565 46 380 403 477 177 403 165 443``<br>
``403 41 380 414 565 403 {01011001}``
<br>Alright... Let's decipher the first text ``//Jung vf ornyr pvcure?``
<br>Deciphering using ROT13 and we get<br>
<br><img width="696" alt="image" src="https://user-images.githubusercontent.com/79892065/159498838-1296b2dc-7945-4ca8-b4e3-d48f1f935349.png">
<br>//What is beale cipher?
<br>Looking up beale cipher in google we see that they are 3 sets of ciphered text and only one of them is deciphered
<br>Let's see how it was deciphered<br>
<br><img width="805" alt="image" src="https://user-images.githubusercontent.com/79892065/159499174-03b48bcb-e8e9-47e5-bfaf-b462aa2d1561.png">
<br>Ther wikipedia says: "To decrypt it, one finds the word corresponding to the number (e.g., the first number is 115, and the 115th word in the United States Declaration of Independence is "instituted"), and takes the first letter of that word (in the case of the example, "I").
<br>Hmm ok... Let's see the second line
<br>``//9 2 5 12 9 5 22 5 10 5 1 12 5 9 19 1 13 1 12 1 25 19 9 1 14``
<br>I'm not so sure what this ciphered text means.. Let's see the third line
<br>``//Have you ever heard of a chef named cyber?``
<br>Oh, cyberchef. We fire up cyberchef
<br>We try pasting the second line of ciphered text inside:<br>
<br><img width="481" alt="image" src="https://user-images.githubusercontent.com/79892065/159501634-74fd3936-9b9c-40af-832a-f1439643da77.png">
<br>A1Z26 Cipher Decode... and since we deciphered it, we get ``ibelievejealeisamalaysian``
<br>Hmm, since above has already mentioned that the beale cipher uses the Unites States Declaration of Independence, let's try the Malaysia Declaration of Independence!
<br>Googling Malaysia Declaration of Independence we get this.... 
<br>https://en.wikipedia.org/wiki/Malayan_Declaration_of_Independence
<br>Alright, so now let's look at the rest of the cipher text in ``jeale.txt``. It's a beale cipher :>
<br>Paste the english version of Malaysan Declaration of Independence into https://wordcounter.net<br>
<br><img width="421" alt="image" src="https://user-images.githubusercontent.com/79892065/159502880-f8b102d7-dd4a-4753-b552-9ab0a0c82398.png">
<br>So we got 565 words in total. We can now decipher it according to the ciphered text in jeale.txt
<br>``1 41 418 125 565 102 95 403 150 304 403 34 46 177 34 125 477 477 403 565 34 403 443 177 403 7 443 403 41``
<br>``477 46 41 46 125 565 177 125 477 403 46 34 41 380 304 142 125 304 403 565 46 380 403 477 177 403 165 443``
<br>``403 41 380 414 565 403 {01011001}``
<br>Let's say 1 is "In", so the first deciphered letter should be I
<br>41 is "the", so the second deciphered letter should be t<br>
<br><img width="652" alt="image" src="https://user-images.githubusercontent.com/79892065/159503675-e4886623-cbc9-4a32-aeeb-eff30f2ad7e7.png">
<br>And so on...
<br>So the deciphered text is:
<br>``i think jeale has hidden her secret data inside a html file named sEcREtmonEy``
<br>Note: The last cipher text {01011001} is a binary and converting to text is a Y
<br>Now we got the clue to go to sEcREtmonEy.html, let's see whats inside!
<br>We see two things:<br>
<br><img width="663" alt="image" src="https://user-images.githubusercontent.com/79892065/159514395-dbeae3fc-6aae-44bc-941e-bbcab0664f25.png">
<br>The source code gives us hint ``<!-- What is ASCII? -->``
<br>That tells is the following challenge might be relatable to ASCII
<br>Let's download the Jeale's Conversation file first
<br>Listening to it, it's definately morse code, let's try to decode it
<br>https://morsecode.world/international/decoder/audio-decoder-adaptive.html
<br>Decoding it we get:
<br>``JEALE IS EVIL. I INTERCEPTED A RADIO OF A MESSAGE OF HER TALKING TO SOME GOVERNMENT OFFICIALS. JEALE:" I AM GOING TO TRANSFER MY BLACK MONEY TO SEPARATED BANK ACCOUNTS IN DIFFERENT COUNTRIES. TSEE PASSWORD FOR ALL OF THE BANK ACCOUK IS GOING TO BE THE MOST COMMON PASSWORD FOR PROTECTING A MALWARE SAMPLE ZIP FILE... DO NOT INTERRUPT OR INVESTIGATE ANYTHING ABOUT ME, THANK YOU.``
<br>The most common password for protecting a malware sample zip file is "<b>infected</b>"
<br>Alright, so we now have a password called "infected"
<br>Now let's download the second file "Money.zip"
<br>Unzip the file using the password given "infected"
<br>There are files from Money 1 -> Money32, and there's also a money.jpg<br>
<br><img width="634" alt="image" src="https://user-images.githubusercontent.com/79892065/159520716-3596e185-4fd0-4c67-b899-d68aa54a1bdf.png">
<br>Let's look into money.jpg first
<br>In Kali Linux, running the command ``binwalk -e money.jpg`` tells us that there's a hidden zip archieve file<br>
<br><img width="949" alt="image" src="https://user-images.githubusercontent.com/79892065/159521380-4c6c6a40-2ae0-499b-8c18-0981d14176fa.png">
<br>Changing the money.jpg file to money.zip file and extract them
<br>We get a folder called truth and a truth.txt file inside the folder<br>
<br><img width="797" alt="image" src="https://user-images.githubusercontent.com/79892065/159521739-6a09ef38-874e-4236-ab16-bbc3a885424a.png">
<br>Looks like a conversation between Jeale and a police officer, it seems like the police officer received a ciphered text message
<br>So number 32 could be base32, let's try deciphering it with base32
<br>Deciphering it results in the first flag!
<br>Now, our goal is the 2nd flag
<br>There are a bunch of Money[num] folders inside the zip file
<br>The hint given to us earlier is the source code ``<!-- What is ASCII? -->``
<br>Looking into some ofthe folders there are money images
<br>So the solution to this is that you can add up the money in the folder and turn them from ASCII to text using websites like https://www.duplichecker.com/ascii-to-text.php
<br>Let's say the first folder "Money1" has 1 RM50, 1 RM10, 1 Rm5 and 2 RM1
<br>Adding them up equals to RM67
<br>So turning ascii 67 to text is C<br>
<br><img width="591" alt="image" src="https://user-images.githubusercontent.com/79892065/159523651-a60103bd-2265-4ea7-8a47-3b3547351931.png">
<br>We'll try two more
<br>The second folder "Money2" has 1 RM50, 1 RM20, 1RM10 and 4 RM1
<br>Adding them up equals to 84
<br>Turning ascii 84 to text is T<br>
<br><img width="559" alt="image" src="https://user-images.githubusercontent.com/79892065/159523997-959e7ed3-7dde-4fa5-a475-89072b6e6ee8.png">
<br>Last one
<br>The third folder "Money3" has 1 RM50 and 1 RM20
<br>Adding them up equals to RM70
<br>Turning ascii 70 to text is F<br>
<br><img width="556" alt="image" src="https://user-images.githubusercontent.com/79892065/159524191-2730e05c-fb07-4b5a-8ace-a1340e987e13.png">
<br>And so on...
<br>So after doing all the "Money" you'll end up getting the 2nd flag
<br>So now you've got the 1st flag, 2nd flag and 3rd flag!
<br>Congratulations! You just solved the Jeale CTF!
