# Nguyễn Minh Phú Quang
# MSSV: 22110064
# Encrypting-large-message_LAB
##  Tasks:
### 1. Encrypt and Decrypt Text file:
- First i create a 'plain.txt' file with the following contetn:

![image](https://github.com/user-attachments/assets/fa07c8ef-7e7e-4206-97d5-e465aec10ef8)

- Encrypted this file using aes-256-ecb:
````
openssl enc -aes-256-ecb -nosalt -in plain.txt -out ecb_encrypted.txt -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF
````
- With the encyption key -K, now view ecb_encrypted.txt:
````
xxd ecb_encrypted.txt
````
![image](https://github.com/user-attachments/assets/bf022dcc-1d5e-4b4c-873a-39a1969352b9)

- Decrypted the file using the same key:
````
openssl enc -d -aes-256-ecb -nosalt -in ecb_encrypted.txt -out ecb_decrypted.txt -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF
````
- Vỉew ecb_decypted.txt:

![image](https://github.com/user-attachments/assets/307d7ed5-9bd1-4ea5-88da-74eb97eb3be8)

### 2. Encryption Mode – ECB vs. CBC 
- Download a bitmap file coming along with this lab, save as origin.bmp, view this file using display:
````
display origin.bmp
````

![image](https://github.com/user-attachments/assets/bdd9fac3-86e8-42ac-ad7b-ed376e319344)

- In BMP files, the first 54 bytes typically contain the header information, which includes metadata about the image such as file size, image dimensions, and color information.This command extracts the first 54 bytes from origin.bmp and saves them into header.bin:
````
dd if=origin.bmp of=header.bin bs=1 count=54
````

![image](https://github.com/user-attachments/assets/871d9650-7f04-4f2f-a168-eb34708d7800)

- This command skips the first 54 bytes of origin.bmp (the header) and saves the rest of the file into body.bin. This part of the file contains the actual image data (the pixel information):
````
dd if=origin.bmp of=body.bin bs=1 skip=54
````

![image](https://github.com/user-attachments/assets/f5baf22a-cc97-4669-b7b2-2bd131626731)

- Now we have to encrypted the file using encryption key and initialization vector:
 - K: This option allows you to provide the encryption key directly in hexadecimal format.
 - [KEY]: This is the actual encryption key, which should be provided as a hexadecimal string.
 - iv: This option allows you to provide the initialization vector directly in hexadecimal format.
 - [IV]: This is the actual initialization vector, which should be provided as a hexadecimal string. The IV is used to add randomness to the encryption process and ensure that the same plaintext encrypted with the same key does not produce the same ciphertext.

![image](https://github.com/user-attachments/assets/f3f2c6b7-51ee-46c5-9157-346dfd9c9ddd)

- Now, we have all the K and IV, encrypted 'body.bin' using an encryption algorithm (e.g., AES) without encrypting the header.
````
openssl enc -aes-256-cbc -nosalt -in body.bin -out encrypted_body.bin -K b72b6fcf6afb0ba0c2ec96ed259b5e7b79e752b905995c0da05161b69fa4e7df -iv 8751e81cd3bbb2f8da9ca0e442f19193
````
- Combine the original header with the encrypted body to form a new BMP file.
````
cat header.bin encrypted_body.bin > partially_encrypted.bmp
````
- Let's view partially_encrypted.bmp:
````
display partially_encrypted.bmp
````

![image](https://github.com/user-attachments/assets/77b6f40c-274c-4ea7-be19-377612e922b1)

- Overview observation:
 - It doesn't have a structure or a pattern related to the origin picture
 - When an image is encrypted using ECB mode, any repetitive patterns in the image will still be visible in the encrypted image. This is because identical blocks of the image data will be encrypted to identical blocks of ciphertext, producing a visually discernible pattern.
### 3.  Encryption Mode – Corrupted Cipher Text 



  

