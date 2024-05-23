# PClub_230217_Task8

## Flag 1 

The hint for the first flag is mentioned in the question itself. It suggests that in the Directory of the website, there exists a route.txt file that stores list of all routes to further penentrate the site. Hence we can use path traversal to read the `route.txt` file. 

Now the challenge is to get the path to any of the file present in the directory of website which we can grab from any image of the gallery . 

*FOR EG : <http://74.225.254.195:2324/getFile?file=/home/kaptaan/PClub-DVWA/static/images/gallery/5.jpeg>*

Now for accessing `routes.txt` I modified the link to : *<http://74.225.254.195:2324/getFile?file=/home/kaptaan/PClub-DVWA/routes.txt>*

![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/772d23ed-8076-41d2-b74b-d445c1d61bd4)

**Hence I get the the flag : _pclub{path_trav3rsa1_1s_fun}_**

------------------------------------------------

## Flag 2 

The hint for flag 2 is mentioned in routes.txt. Use command injection . On directing to the paths I guessed that the ip details to be vulnerable to command injection. I personally don't have idea about `js` but it was the only page that looked susciptable hence I tried with a random IP followed by `; ifconfig`  and worked . 

![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/f36bddbb-8818-41d2-be25-5e517f0e03aa)

**Flag 2 is : _60:45:bd:af:3f:e5_**

-----------------------------------------------------------------------------------

## Flag 3 

On trying some more commands injections I was able to retrieve a database of usernames and password . The command I used was `more *` . 


![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/929de9ee-487d-4561-8e59-e327b8ff1e36)


![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/278bb1d8-65fe-48c6-8e01-d95e578a28a9)

 
Now I as I have attended to spring-camp so it was a bit intuitive for me to log into Aman S.Gill and Ritvik account' s first. From there I obtain 


![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/d8a058aa-e2fa-4ecb-a707-4108bf924b6c)


ariitk.jpeg and

![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/d35376bd-c6dc-4847-b4d1-aec8bd58413b)


pwn_challenge_link.txt 

Now I performed steg on ariitk.jpeg . After trying several tools like , exiftool , binwalk I finally suceeded on steghide .

![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/6459679d-7b6e-491a-a014-ad70c24b6102)

I got a qr_code. I scanned the qr and I got a string which looked like a b64 encoded .

The string was __Y3B5aG97cW5nbl8xZl8zaTNlbGd1MWF0fQ==__

![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/0bb10a5f-548e-4976-a5d1-533a44de7775)

First I used base64 decode and then I observed it to be rot cipher so I calculated the rot and used translate comamnd .

The command used  : `echo Y3B5aG97cW5nbl8xZl8zaTNlbGd1MWF0fQ== | base64 -d | tr [a-zA-Z] [n-za-mN-ZA-M] `

The flag 3 : pclub{data_1s_3v3ryth1ng}

---------------------------------------------------------------------------

## Flag 4 : 

For the fourth flag it seemed that we were required to connect to MySQL database but before trying anything regarding I tried the username:kaptaan and password: 0123456789 in secy login page which was mention in the create_data.py 

![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/6e550000-6e16-4ae2-839f-0f3c5e758ed9)


From there I got `flag.txt` :

![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/24dbf596-fcd9-4b9b-9fb9-7fdfd7d49b97)

**Hence the fourth flag was : pclub{01d_1s_g01d_sql1}** 

*Website is Damn Vulnerable* 

----------------------------------------------------------------------------------------------

## Flag 5 : 

For the last flag required to pwn the login system

From the pastebin link we get the login system binary and the netcat command to conenct .

On decompilig with Ghidra we got a pretty clean decompiled code . After seeing the hint provided in pastebin link that something is overflowing , I quickly looked for address in stack where each variable is being stored and that is where I found the vulnerablitiy. 

![image](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/1e5b453d-20d5-4b7c-9f22-414999bcd42e)

local_20 which is only 18 byte long is actually using 20 bytes of memory i.e. is it overwriting 2 bytes of loacl_e. 

Hence if a string whose hash will end with 0000 i.e. 4 zeroes will set out local_e to 0 and hence bypassing the check .

For finding such strings I took help from internet and cooked a python script that hashes all passwords in rockyou.txt 
and only stores those passwords whose hash ends with 4 zeroes : 

```python
import hashlib

with open('rockyou.txt', 'r', encoding='latin-1') as f:
    words = [line.strip() for line in f.readlines()]

sha1_hashes = []
for word in words:
    sha1_hash = hashlib.sha1(word.encode()).hexdigest()
    if sha1_hash.endswith('0000'):
        sha1_hashes.append((word, sha1_hash))

with open('rockyou_sha1_four_zeroes.txt', 'w') as f:
    for word, sha1_hash in sha1_hashes:
        f.write(f"{word}:{sha1_hash}\n")

```

Hence I get huge list of passwords which will be qualified to break through the login system .

Hence using one of them I was able to retrive the flag :

![Screenshot from 2024-05-22 23-01-25](https://github.com/fidgetaryan445/PClub_230217_Task8/assets/148867576/9976cdbd-ac90-481e-ad72-807ff7803418)



**Flag 5 : _pclub{d0_y0u_kn0w_7h47_h0w_much_1_5truggl3d_w1th_0p3n55l_wtf_rockyou}_*

---------------------------------------------------------------------------------------------
