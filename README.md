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
