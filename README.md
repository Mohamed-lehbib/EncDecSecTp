# EncDecSecTp
## Tp1 
1. To verify that i have openssl 
```
openssl version
```
2. To install openssl
```
sudo apt-get -y install openssl
```
I got an error saying this message
```
E: dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem
```
3. I run this command to config
```
sudo dpkg --configure -a
```
then i pressed esc
4. Then i installed the openssl and everything is just fine
5. Then i have Moved to the Documents directory to run all the commands there
```
cd Documents
```
6. Then i have generated a random key with a size of 192 bit or 24 octets
```
openssl rand -out secret.key -hex 24
```
7. Then i have created a file with a content
```
echo "I want this to be encrypted" > message.txt
```
8. Show some content 
```
cat message.txt
```
9. Then i have encrypted the message without password
```
openssl enc -aes-128 -in message.txt -out message.enc
```
Then i got an error saying: `unkown cipher: aes-128` To see all the encryption option
```
openssl --help
```
and then i have found them 
```
openssl enc -aes-128-cbc -in message.txt -out message.enc
```
10. Then i have displayed the conent of this message.enc
```
cat message.enc
```
or 
```
hexdump -c message.enc
```
11. Then i have create a file called "fst.txt"
```
echo "Master informatique FST" > fst.txt  
```
12. Then i have encrypted this file using the secret.key
```
openssl enc -aes-128-cbc -in fst.txt -out fst.enc -pass file:./secret.key
```
when i have run this command i got a œarning saying `*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.`
to make it more secure i can do this
```
openssl enc -aes-128-cbc -in fst.txt -out fst.enc -iter 10000 -pass file:./secret.key
```
13. Then i have decrypted the file using the generated key
```
openssl enc -d -aes-128-cbc -in fst.enc -out dec.txt -iter 10000 -pass file:./secret.key
```
14. Then i have created a folder and copied the file in it
```
mkdir -p Tp1 | cp fst.txt 'TP1 - SI.pdf' Tp1
```
15. Then i have ziped the folder using this command
```
tar -czvf archive.tar.gz Tp1
```
16. THen i have encrypted this zip using des3
```
openssl enc -des3 -in archive.tar.gz -out archive.tar.des3 -pass pass:1234 -iter 100
```
17. Then i have decrypted the zip
```
openssl enc -d -des3 -in archive.tar.des3 -out DecArchive.tar.gz -pass pass:1234 -iter 100
```
18.Then i have deziped it
```
tar -xzvf DecArchive.tar.gz
```
19. In this part i have generated the private key 
```
openssl genrsa -des3 -out key_kali.priv 4096
```
20. To generate the public key 
```
openssl rsa -pubout -in key_kali.priv -out key_kali.pub
```
21. Then i have encrypted the private key RSA using des3
```
openssl rsa -in key_kali.priv -des3 -out private_key_des3.des3
```
22. Then i have encrypted a file using the rsa key <br>
- First i have created a file using this command
```
echo "hello world" > file.txt 
```
or 
```
cat <<q>> message.txt
```
- Then i have encrypted it using the rsa public Key
```
openssl pkeyutl -encrypt -in file.txt -pubin -inkey key_kali.pub -out file_encrypted.txt -pkeyopt rsa_padding_mode:oaep
```
or and it the one used by the teacher
```
openssl pkeyutl -encrypt -pubin -inkey key_kali.pub -in message.txt -out encrypted_messaga.txt
```
23. I have shared the public key to my ubuntu user using ssh
```
scp key_kali.pub mohamed@192.168.137.242:/home/mohamed/Documents/Tp
```
24. I have created a file the ubuntu user
```
echo "Hello from ubuntu" > hello.txt
```
25. I have encrypted the file using the public key sent by the kali user
```
openssl pkeyutl -encrypt -in file.txt -pubin -inkey key_kali.pub -out file_encrypted.txt -pkeyopt rsa_padding_mode:oaep
```
or and it the one used by the teacher
```
openssl pkeyutl -encrypt -pubin -inkey key_kali.pub -in message.txt -out encrypted_messaga.txt
```
26. Then i have sent the encrypted file using ssh 
```
scp hello_encrypted.txt kali@192.168.137.242:/home/kali/Documents/tpSec
```
27. Then i have decrypted the file using the rsa private key
```
openssl pkeyutl -decrypt -in hello_encrypted.txt -inkey key_kali.priv -out hello_decrypted.txt -pkeyopt rsa_padding_mode:oaep
```
or and it suggested by the teacher
```
openssl pkeyutl -decrypt -inkey key_kali.priv -in hello_encrypted.txt -out hello_decrypted.txt
```
28. Then i have tried to encrypt a large file using the rsa Key
```
openssl pkeyutl -encrypt -in 'TP1 - SI.pdf'  -pubin -inkey key_kali.pub -out tp_encrypted.pdf -pkeyopt rsa_padding_mode:oaep
```
Then i got this error message `Public Key operation error
20E0348AFFFF0000:error:0200006E:rsa routines:ossl_rsa_padding_add_PKCS1_OAEP_mgf1_ex:data too large for key size:../crypto/rsa/rsa_oaep.c:87:
`
29. i have switch to the ubuntu user and generated an aes 256 secret key
```
openssl rand -out secret.key -hex 32
```
30. then i have encrypted this key using my rsa public key
```
openssl pkeyutl -encrypt -in secret.key -pubin -inkey key_kali.pub -out secret_encrypt.key -pkeyopt rsa_padding_mode:oaep
```
31. Then i have sent the encrypted secret key to my kali user
```
scp secret_encrypt.key kali@192.168.100.156:/home/kali/Documents/tpSec
```
32. Then i have decrypted the secret encrypted key using the private key
```
openssl pkeyutl -decrypt -in secret_encrypt.key -inkey key_kali.priv -out secret.key -pkeyopt rsa_padding_mode:oaep
```
33. Then i have encrypted a large file using it
```
openssl enc -aes-256-cbc -in 'TP1 - SI.pdf' -out tp_enc.pdf -pass file:./secret.key
```
34. Then i have sent the file to my ubuntu user
```
scp tp_enc.pdf mohamed@192.168.100.140:/home/mohamed/Documents/Tp
```
35. Then i have decrypted the file in the ubuntu user
```
openssl enc -aes-256-cbc -d -in tp_enc.pdf -out tp.pdf -pass file:./secret.key
```
## Transfer files using ssh

- to transfer files we need to install ssh in case of ubuntu
```
sudo apt install openssh-server
```
- then to see the status of ssh
```
sudo systemctl status ssh
```
- then i enabled ssh
```
sudo systemctl enable ssh
```
- then i have started ssh
```
sudo systemctl start ssh
```
- then to transfer file
```
scp message.txt mohamed@192.168.137.242:/home/mohamed/Desktop
```
