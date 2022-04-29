## Jeale's CTF Writeup
### jeale [challenge]
Hello! This is the writeup for the Jeale CTF, let's jump straight into it!<br>
<br><img width="443" alt="image" src="https://user-images.githubusercontent.com/79892065/159476078-fd5c6711-9124-4aed-b470-58a9db9ef09b.png">
<br>Going to https://jeale.ml/ and reading the rules, We know that the flag format is CTF{}, and there are 3 flags in the website.
<br>So let's head off to the challenge section. Once we clicked on it, it redirects us to https://jealectf.ml/challenge.<br>
<br><img width="892" alt="image" src="https://user-images.githubusercontent.com/79892065/159476255-a0835576-6e2c-47fe-b178-9ad5f93a57cf.png">
<br>So this must be the challenge description
<br>After we clicked "Start from here", we were redirected to /home.
<br>Let's check the robots.txt in /robots.txt and we see this<br>
<br><img width="455" alt="image" src="https://user-images.githubusercontent.com/79892065/162562368-d1d1f7ff-94ec-4451-ab4d-fc9d87d7f902.png">
<br>It says ``# Is this really the only robots.txt in this website?``
<br>Maybe there is another robots.txt file in this website?
<br>There is also two disallows ``Disallow: /index.html`` and ``/mayb3_th4_s3cr4t_fl4g.html``
<br>Checking /mayb3_th4_s3cr4t_fl4g.html leads us to a rickroll 
<br><img width="648" alt="image" src="https://user-images.githubusercontent.com/79892065/160245429-f640fc7f-4a8e-451c-846a-9313af6ded5c.png">
<br>Let's curl /index.html and see if there's anything interesting
<br><img width="879" alt="image" src="https://user-images.githubusercontent.com/79892065/160223929-0045424b-09b2-4c3c-ae0b-ea0bd0dd5c6f.png">
<br>It says it's Jeale's Personal Page, scrolling down, we see a ``l0gs.html`` and ``<!--Jeale: Phew, nobody can see this. Do you know what is steganography?-->``
<br>Curling l0gs.html, we see a s3rv7r.l0g file<br>
<br><img width="540" alt="image" src="https://user-images.githubusercontent.com/79892065/162562400-48a972dd-1c72-4957-a420-feb0db556b21.png">
<br>Download the ``/s3rv7r.l0g`` file
<br><img width="955" alt="image" src="https://user-images.githubusercontent.com/79892065/162562464-9df0934a-13b2-421f-a68c-cea3b049cef7.png">
<br>We see ``https://jealectf.ml/3ncrypt3d%5Fr0bot%2Etxt``, following the url leads us to a page that shows base64 encoded text<br>
<br><img width="959" alt="image" src="https://user-images.githubusercontent.com/79892065/162562484-ebc40718-972a-4479-8b68-50a132d949e1.png">
<br>Decoding it with base64 we get the second robots.txt file!<br>
<br><img width="480" alt="image" src="https://user-images.githubusercontent.com/79892065/162562505-28ae7629-a801-4339-9870-14c66c74cf57.png">
<br>There is /jeales_vigenere and /W4ht_a3di0_1s_th1s, let's check it later.
<br>There's also another hint when we curl /index.html, and that is the steganography, keep that in mind

* Hints
    * Steganography

<br>Alright, let's move back to the website first<br>
<br>We are greeted with a login page, checking the source code it looks like it's using StatiCrypt to encrypt the website<br>
<br><img width="449" alt="image" src="https://user-images.githubusercontent.com/79892065/160218959-2a8e22ef-f3cd-4ffd-980c-fed093ab2935.png">
<br>There's a batch program called "Login Batch File", let's check it out<br>
<br><img width="655" alt="image" src="https://user-images.githubusercontent.com/79892065/161390355-d97d3d68-703b-4401-af55-c453fd79d7af.png">
<br>Inside the batch file, it has some chinese characters, hm...<br>
<br><img width="370" alt="image" src="https://user-images.githubusercontent.com/79892065/161390383-4a1811c6-8161-4d4f-9164-98f8284b82df.png">
<br>Running the program we get to know that it's a batch obfuscation.
<br>This may help ``https://github.com/BiggerDABOSS/BatchObfuscator/blob/master/Deobfuscation.cmd``
<br>We can run `` ./deobfuscator.cmd login.bat`` and we got a deobfuscated batch file!<br>
<br><img width="454" alt="image" src="https://user-images.githubusercontent.com/79892065/161390522-5d75769e-104b-43fa-ad2d-55418ea432b2.png">
<br>The password to login into the website seems to be ``M0stS3cur3P@@sw0rD1nth3w0r1d``
<br>We got the password to login to the website, we see this<br>
<br><img width="943" alt="image" src="https://user-images.githubusercontent.com/79892065/159477088-365b2050-de70-4ddc-a0b9-e066239cfaa2.png">
<br>Right click, f12, CTRL+SHIFT+I and CTRL+SHIFT+J is disabled, but we can still view the source using view-source:
<br>This is a StatiCrypt obfuscated html code so we can't view the source code but there's something in there...<br>
<br><img width="462" alt="image" src="https://user-images.githubusercontent.com/79892065/162562935-5a61f841-0600-4a4c-b7da-27d0c0291543.png">
<br>``<!-- Source is at /home/source :> -->``
<br>Going to /home/source and we see the source code<br>
<br><img width="673" alt="image" src="https://user-images.githubusercontent.com/79892065/162562988-7db55fd5-6c5d-415f-8e8c-734746a680c5.png">
<br>Checking into the source's source code, we see another hint<br>
<br><img width="830" alt="image" src="https://user-images.githubusercontent.com/79892065/162562996-03020007-0842-4177-8dd9-a2e9986ce525.png">
<br>``<!--Jeale loves styling the website! -->``
<br>Check the css file of this website. it's located in /style.css<br>
<br>In the css file we see this<br>
<br><img width="745" alt="image" src="https://user-images.githubusercontent.com/79892065/162563026-8b4e9f1b-4265-4bde-a40e-b86d5b0b4c4a.png">
<br>``/* Hint: I made a favicon for Jeale's website and she likes it! There' nothing inside the image, for real!*/``
<br>Favicon... maybe it's related to steganography.
<br>The favicon file is in /favicon.jpg according to the source code
<br><img width="537" alt="image" src="https://user-images.githubusercontent.com/79892065/159478555-0a4ca0ad-5fec-410a-a63d-48268faf330a.png">

* Hints
    * Steganography
        * favicon.jpg

<br>Looking deeper into the source code, there is a hint telling us to stop right there. So let's stop looking down to the source code.<br>
<br><img width="702" alt="image" src="https://user-images.githubusercontent.com/79892065/162563342-833ea150-6dbc-4001-8609-62cd8595c53c.png">
<br>The source code we just looked into is the /home, so let's try lookingw into the /about, /testimonials/ and /contacts/
<br>All of them says that "Coming Soon' and a number showing the days left<br>
<br><img width="711" alt="image" src="https://user-images.githubusercontent.com/79892065/159479389-40ad7952-a68b-49f4-9042-1001f32b55d0.png">
<br>Inspect element is also disabled so let's look into the source code
<br>In /about we see
<br>``<!-- I remember Jeale names her folder with some numbers and hides it after the page url, and each of the numbers are different every time. Where does these numbers come from?-->``
<br>Remember the numbers we got earlier that says "" days left? We can do something like https://jealectf.ml/about/[number]
<br>We can also look into /testimonials/ and /contacts and try https://jealectf.ml/testinomails/[number] and https://jealectf.ml/contacts/[number]
<br>Remember that the numbers are different so change different numbers in different pages
<br>Let's go to /about/365 and see what it shows us<br>
<br><img width="904" alt="image" src="https://user-images.githubusercontent.com/79892065/165931695-7b2f2dee-a295-4344-90d8-7db44dcfede8.png">
<br>In /testimonials/253
<br>``GO TO``<br>
<br><img width="767" alt="image" src="https://user-images.githubusercontent.com/79892065/165931778-6a1b795b-c1d4-4eed-9c0b-cac1c2c83e62.png">
<br>``https://jealectf.ml/``
<br>In /contacts/60
<br>``cipher_time``<br>
<br><img width="707" alt="image" src="https://user-images.githubusercontent.com/79892065/165931943-5fe980c4-c108-4de9-9142-50a13250f380.png">
<br>It says ``Go to https://jealectf.ml/cipher_time`` so let's go there and check it out<br>
<br><img width="784" alt="image" src="https://user-images.githubusercontent.com/79892065/165932535-fb2fba79-8ed1-485a-a5ca-86f93d1f6fbb.png">
<br>Downloading the files on Kali Linux and let's check the hintdumped.txt file first<br>
<br><img width="591" alt="image" src="https://user-images.githubusercontent.com/79892065/165934769-cf247e1f-2069-49b0-aec5-a62d40cb51e5.png">
<br>That looks like hexdump, run ``xxd -r hintdumped.txt > hint.txt`` to convert into plain text<br>
<br><img width="948" alt="image" src="https://user-images.githubusercontent.com/79892065/165934855-cad82a53-1868-4885-a773-a0fde35521b6.png">
<br>We need to unescape the unicode characters, here again I'm using cyberchef :><br>
<br><img width="769" alt="image" src="https://user-images.githubusercontent.com/79892065/165934971-1da88fce-791b-457a-a116-cfcb7a530532.png">
<br>We get a set of numbers, let's try to decode them with ascii<br>
<br><img width="765" alt="image" src="https://user-images.githubusercontent.com/79892065/165935165-c23ed194-6772-41c6-9d7d-76590c7a20e5.png">
<br>Magic pen appears and tells us to use A1Z26 Cipher Decode, and the result is ``hintwhatisthefilesignatureofajpgfile``
<br>Since the hint tells us what is the file signature of a jpg file, googling it tells us the result<br>
<br><img width="706" alt="image" src="https://user-images.githubusercontent.com/79892065/165946393-1b911168-9d21-44b6-916b-870ad99be103.png">
<br>Going back to Kali Linux, we can use a tool called hexeditor, do ``hexeditor unknownfile`` 
<br><img width="955" alt="image" src="https://user-images.githubusercontent.com/79892065/165946162-5251c3c6-4e7e-473a-8033-08e6c3f1467b.png">
<br>It looks like the jpg file is corrupted and the header has been changed to "JEALE"
<br>We can also see that its jfif, and according to the file signature above tells us that its original hex is ``FF D8 FF E0 00 10 4A 46
49 46 00 01``
<br><img width="945" alt="image" src="https://user-images.githubusercontent.com/79892065/165946771-58252f76-2365-4426-9641-b932fd7c7362.png">
<br>Modify it in hexeditor and save the file<br>
<br>Now rename the extension to jpg by doing ``mv unknownfile unknownfile.jpg``, view the image and we get the third flag!<br>

* Flag
    * Third flag acquired
* Hints
    * Steganography
        * favicon.jpg

<br>Alright, let's continue the ``/jeales_vigenere`` and ``/W4ht_a3di0_1s_th1s`` we got from the second robots.txt file earlier
<br>We'll visit ``/jeales_vigenere`` first
<br>We get:<br> ``cipher = Qmpas ioeyqr gg avf AWH q qpxai dgz, avt zojl: "Hltad, W'ks awprrf fp kaja fuahlsud tv o waam nism rpzasw k1r1_d@fkp0ifV." M fuyl ndyu hptd iv vftp tm uxbs vxf iwsa irgkaghx, xateve...``
<br>The encoded key is hexdump and it looks like png<br>
<br><img width="599" alt="image" src="https://user-images.githubusercontent.com/79892065/161654076-deeaa4f6-ebf4-4c53-84c2-f433ae4cea9e.png">
<br>As we can see there is a cipher text and an encodedkey, so we need to decipher the cipher text using the key
<br>This is a vigenere cipher according to /jeales_vigenere
<br>It also gives us hint to use cyberchef<br>
<br><img width="398" alt="image" src="https://user-images.githubusercontent.com/79892065/162563463-698b9e2d-7d6e-4321-9401-305bf81b9c5f.png">
<br>Opening cyberchef and decode the key with the magic pen and we get a hash<br>
<br><img width="770" alt="image" src="https://user-images.githubusercontent.com/79892065/161654388-70a39fbf-7c18-4d22-8278-1b45a3949a9f.png">
<br><img width="763" alt="image" src="https://user-images.githubusercontent.com/79892065/161654448-2b480058-e9b9-4a3d-a80e-153bc8a5b99c.png">
<br>Go to https://crackstation.net, enter the hash and we got the key!<br>
<br><img width="827" alt="image" src="https://user-images.githubusercontent.com/79892065/161654502-d2f9e786-c0b5-40ef-b68d-79a427026030.png">w
<br>So since it is vigenere cipher we can decipher it on cyberchef<br>
<br><img width="756" alt="image" src="https://user-images.githubusercontent.com/79892065/161654678-c7d2aa95-4724-48b6-a7e3-38011f24e456.png">
<br>``Jeale talked to her ISP a while ago, she said: "Hello, I've hidden my wifi password in a html file called w1f1_p@ssw0rrD." I need your help to help me find her wifi password, please...``
<br><br>Alright, now let's go to /w1f1_p@ssw0rrD<br>
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
           
<br>Downloading the wifi .cap file we can brute force it with rockyou.txt :>
<br>To make sure it's a handshake file run eapol on wireshark and yes it is!<br>
<br><img width="430" alt="image" src="https://user-images.githubusercontent.com/79892065/159484318-b91a22d0-891a-4635-a4f5-66319f3639c1.png">
<br>The source code of ``/w1f1_p@ssw0rrd`` also says
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

<br>Now we can on another html file ``/W4ht_a3di0_1s_th1s``<br>
<br><img width="938" alt="image" src="https://user-images.githubusercontent.com/79892065/165947795-89ccdafa-4cf2-4b56-9eac-23d6264bc808.png">
<br>``qlz (apqzgpmqawpmn kfmhz kqmqawp) sgwmehmkqze m kzhgzq vzkkmoz qw bzmnz. kaphz qlz vzkkmoz umk szapo sgwmehmkqze izgr (knwunr), kwvzwpz (khmppze) qlz vzkkmoz xkapo qlzag izgr (wne qznziakawp) sxq mnn qlzr hwxne lzmg uzgz kwvz eakqwgqze pwakzk….. fzglmfk rwx hmp lznf xk kwniz aq?``
<br>Hm, checking the source code it says ``<!--What is substitution cipher?-->``
<br>We can use online tools like ``https://www.guballa.de/substitution-solver`` to auto decipher it for us<br>
<br><img width="485" alt="image" src="https://user-images.githubusercontent.com/79892065/165948147-19233351-37e8-48d3-9b06-677732d821a1.png">
<br>`the (international space station) broadcasted a secret message to feale. since the message was being broadcasted very (slowly), someone (scanned) the message using their very (old television) but all they could hear were some distorted noises….. perhaps you can help us solve it?
``
<br>Description says (international space station), (slowly scanned) and (old television)
<br>Googling it tells us that is slow-scan television
<br><img width="569" alt="image" src="https://user-images.githubusercontent.com/79892065/165948349-1a6390c2-275c-456d-8b79-d001d58d78c6.png">
<br>Downloading the audio file we hear some sort of radio voice going on
<brAccording to description, we can use tools like MMSSTV (windows) and QSSTV (linux) to decode<br>
<br><img width="958" alt="image" src="https://user-images.githubusercontent.com/79892065/165948722-eca78803-3c5b-4649-a1cf-a318ed87405a.png">
<br>We got 6 hashes! Remember to write it down into a text editor
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
<br><img width="555" alt="image" src="https://user-images.githubusercontent.com/79892065/159833812-984efa3a-b5a3-44f9-9f21-58862fc810f5.png">
<br>The source code gives us hint ``<!-- What is ASCII? -->``
<br>That tells one of the following challenge might be relatable to ASCII
<br>Let's download the Jeale's Obfuscated Conversation first
<br>It's a .bat file, opening it we see a bunch of chinese characters
<br>Hmm, let's run the .bat file first, we get a conversation.mp3 and hint file<br>
<br><img width="527" alt="image" src="https://user-images.githubusercontent.com/79892065/160264904-1c90cb07-7857-4166-be45-4b364ae762ff.png">
<br>convesation.mp3 seems to be corrupted, let's open it with a text editor<br>
<br><img width="887" alt="image" src="https://user-images.githubusercontent.com/79892065/159836603-0df1b566-efda-47e1-95d2-9457100e2751.png">
<br>Holy! That's a lot of text over there, let's view the "hint" file first
<br>It says ``//Jeale loves certutil!``
<br>Googling certutil embed file, we get to know that certutil is a built-in windows feature. We can decode it by using the following command
<br>``certutil -f -decode conversation.mp3 conversationputput.mp3``
<br>After that, we get to hear the audio, it's a morse code!
<br>https://morsecode.world/international/decoder/audio-decoder-adaptive.html
<br>Decoding it we get:
<br>``JEALE IS EVIL. I INTERCEPTED A RADIO OF A MESSAGE OF HER TALKING TO SOME GOVERNMENT OFFICIALS. JEALE:" I AM GOING TO TRANSFER MY BLACK MONEY TO SEPARATED BANK ACCOUNTS IN DIFFERENT COUNTRIES. TSEE PASSWORD FOR ALL OF THE BANK ACCOUK IS GOING TO BE THE MOST COMMON PASSWORD FOR PROTECTING A MALWARE SAMPLE ZIP FILE... DO NOT INTERRUPT OR INVESTIGATE ANYTHING ABOUT ME, THANK YOU.``
<br>The most common password for protecting a malware sample zip file is "<b>infected</b>"
<br>Alright, so we now have a password called "infected"
<br>Now let's download the second file "Money.zip"
<br>Unzip the file using the password given "infected"
<br>There are files from Money 1 -> Money32, and there's also a money.html file<br>
<br><img width="757" alt="image" src="https://user-images.githubusercontent.com/79892065/161438020-410768dc-dd16-4444-a9ce-e664697c6c19.png">
<br>Let's check the html file first<br>
<br><img width="822" alt="image" src="https://user-images.githubusercontent.com/79892065/161438047-e04c9450-9d24-4543-89d7-bfa02a1ae14e.png">
<br>Looks like we need to do some osint...
<br>In Instagram searching for the name Jeale Saints we see this<br>
<br><img width="646" alt="image" src="https://user-images.githubusercontent.com/79892065/161498871-b2b939a0-340c-4c69-8e0d-add2952af8b0.png">
<br>Looking into the youtube channel's about page we see this<br>
<br><img width="848" alt="image" src="https://user-images.githubusercontent.com/79892065/161677895-6452e61d-fc34-462b-b494-0d4e0b853212.png">
<br>We can use a website called wayback machine to check if anyone has had archived the about page before<br>
<br><img width="818" alt="image" src="https://user-images.githubusercontent.com/79892065/161678012-0035917d-3e5f-457b-8a46-6436436753c1.png">
<br>Yup! Clicking into April 4th we see this<br>
<br><img width="939" alt="image" src="https://user-images.githubusercontent.com/79892065/161678225-a4df89f3-cf3d-45ae-8fcb-cbf55a09282a.png">
<br>Let's send an email to ``jealesaints@gmail.com``<br>
<br><img width="422" alt="image" src="https://user-images.githubusercontent.com/79892065/162563771-a137cc0b-d508-4c1b-a813-cfc748a2ddbf.png">
<br>It worked! Checking into the url we see ``https://jealectf.ml/youaresokind.zip``
<br>Download the ``youaresokind.zip`` file and we got a money.jpg file
<br>In Kali Linux, running the command ``binwalk -e money.jpg`` tells us that there's a hidden zip archieve file<br>
<br><img width="949" alt="image" src="https://user-images.githubusercontent.com/79892065/159521380-4c6c6a40-2ae0-499b-8c18-0981d14176fa.png">
<br>Changing the money.jpg file to money.zip file and extract them
<br>We get a folder called truth and a truth.txt file inside the folder<br>
<br><img width="797" alt="image" src="https://user-images.githubusercontent.com/79892065/159521739-6a09ef38-874e-4236-ab16-bbc3a885424a.png">
<br>Looks like a conversation between Jeale and a police officer, it seems like the police officer received a ciphered text message
<br>So number 32 could be base32, let's decipher it with base32
<br>Deciphering it results in the first flag!
<br>We got the first and the third flag
<br>Now, our goal is to get the 2nd flag
<br>There are a bunch of Money folders inside the Money.zip file
<br>The hint given to us earlier is the source code ``<!-- What is ASCII? -->``
<br>Looking into some of the folders there are money images
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
