What Is a Vanity .onion Address?

A **vanity address** is a custom `.onion` address that **starts with a specific pattern** of your choice, like:
`awesomeblogxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.onion`
Since v3 addresses are based on **ed25519 public keys**, they can't be simply edited. Instead, you have to **brute-force generate private keys** until one of them produces an address with your desired prefix.

For this we will be using this tool named:
[**`mkp224o`**](https://github.com/cathugger/mkp224o) — (one of the most popular)

NOTE: this tool can find urls easily till 5 characters using kali cpu after that you need to wait more or need your own gpu for more characters according to your choice


Step-by-Step: Install `mkp224o` on Linux
1st. Install Dependencies
```bash
sudo apt install git build-essential autoconf automake libtool libsodium-dev -y
```
2nd. Clone The Repo
```bash
git clone https://github.com/cathugger/mkp224o.git
cd mkp224o
```
3rd. run the autogen.sh script
```bash
./autogen.sh
```
This should generate the missing `Makefile.in`, `configure`, and related files.
4th. run the configure script
```bash
./configure
```
This checks your system and prepares the actual `Makefile`.
5th. make or say build the tool
```bash
make
```
6th. Now run it (i want url with ayush in starting)
```bash
./mkp224o ayush
```
and i got this 
![](https://i.imgur.com/qA9nWbl.png)

Now again repeat the steps from the [[Getting Started with Tor]]
but for this we have to do extra steps than the regular ones
1st. make a dir for the hidden service
![](https://i.imgur.com/P70GfdL.png)
2nd.then find open one of them that you want to use and then follow these commands:
```
ls ayush*

sudo cp ./ayushcahjt2jdi46ew74zxbp3gjaq62oqbxrn6x5alyl7eg43bezasqd.onion/hs_ed25519* /var/lib/tor/hidden_service_ayush/

sudo chown -R debian-tor:debian-tor /var/lib/tor/hidden_service_ayush/

sudo chmod 600 /var/lib/tor/hidden_service_ayush/hs_ed25519_public_key

sudo chmod 600 /var/lib/tor/hidden_service_ayush/hs_ed25519_secret_key

sudo chmod +rwx /var/lib/tor/hidden_service_ayush
```
![](https://i.imgur.com/aCTKraj.png)
3rd. Then give the debian tor permissions for this
```
sudo chown -R debian-tor:debian-tor /var/lib/tor/hidden_service_ayush/
```
4th. Now edit the torrc file with the new hidden service
```
HiddenServiceDir /var/lib/tor/hidden_service_ayush/
HiddenServiceVersion 3
HiddenServicePort 80 127.0.0.1:4444

```
![](https://i.imgur.com/xWSrPRl.png)
5th. Now restart the Tor
```bash
sudo systemctl restart tor
or you can also use 
sudo systemctl restart tor@default
```

here at this step i got so many errors which were mainly because of giving improper permissions to the folder /var/lib/tor/hidden_service_ayush/

so keep that in mind as well as also dont run the command systemctl restart tor with root user if you were working with the kali user 
it will create conflicts between the users and permissions and ownerships

also commands to check what is the error
```bash
sudo tor --verify-config -f /etc/tor/torrc

sudo journalctl -xeu tor@default.service
```

so before restarting the tor dont forget to start the server 
(python server in my case)
![](https://i.imgur.com/QefLC32.png)
also the above index.html file is different and the code snippet for it is below
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Manipulator</title>
    <style>
        body {
            margin: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1e1e2f, #2c3e50);
            color: #ecf0f1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            text-align: center;
        }
        h1 {
            font-size: 3em;
            margin-bottom: 0.5em;
            background: linear-gradient(to right, #6a11cb 0%, #2575fc 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        p {
            font-size: 1.2em;
            max-width: 600px;
            margin-bottom: 2em;
            line-height: 1.5;
        }
        .btn {
            background: #6a11cb;
            background: linear-gradient(to right, #6a11cb 0%, #2575fc 100%);
            padding: 15px 30px;
            border: none;
            border-radius: 50px;
            color: white;
            font-size: 1em;
            cursor: pointer;
            transition: transform 0.3s, background 0.3s;
            text-decoration: none;
        }
        .btn:hover {
            transform: scale(1.05);
            background: linear-gradient(to right, #2575fc 0%, #6a11cb 100%);
        }
        footer {
            position: absolute;
            bottom: 20px;
            font-size: 0.8em;
            color: #95a5a6;
        }
    </style>
</head>
<body>

    <h1>The Manipulator</h1>
    <p>Welcome to Manipulator — where innovation meets control. Discover the tools, strategies, and secrets behind the ultimate power of manipulation.</p>
    <a href="#learn-more" class="btn">Learn More</a>

    <footer>
        © 2025 Manipulator. All rights reserved.
    </footer>

</body>
</html>
```

after this and restarting the tor it should look like this 
![](https://i.imgur.com/aGFrZvy.png)

after getting this you can visit the url in the tor browser
(my url)
```
http://ayushcahjt2jdi46ew74zxbp3gjaq62oqbxrn6x5alyl7eg43bezasqd.onion/
```
![](https://i.imgur.com/6AUiv3K.png)

by this way we can get url with any keywords or characters in the beginning of the url by this tool.