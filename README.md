# Cryptography---19CS412-classical-techqniques


# Caeser Cipher
Caeser Cipher using with different key values

# AIM:To encrypt and decrypt the given message by using Ceaser Cipher encryption 
algorithm.

To develop a simple C program to implement Caeser Cipher.

## DESIGN STEPS:

1. In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number 
of positions down the alphabet. 
2. For example, with a left shift of 3, D would be replaced by A, E would become B, and 
so on. 
3. The encryption can also be represented using modular arithmetic by first transforming 
the letters into numbers, according to the scheme, A = 0, B = 1, Z = 25. 
4. Encryption of a letter x by a shift n can be described mathematically as, 
En(x) = (x + n) mod26 
5. Decryption is performed similarly, 
Dn (x)=(x - n) mod26 

## PROGRAM:
#include <stdio.h>
#include <string.h>
// Function to perform Caesar cipher encryption
void encrypt(char message[], int key) {
 int i;
 char ch;
 
 for(i = 0; message[i] != '\0'; ++i) {
 ch = message[i];
 
 if(ch >= 'a' && ch <= 'z') {
 ch = ch + key;
 
 if(ch > 'z') {
 ch = ch - 'z' + 'a' - 1;
 }
 
 message[i] = ch;
 }
 else if(ch >= 'A' && ch <= 'Z') {
 ch = ch + key;
 
 if(ch > 'Z') {
 ch = ch - 'Z' + 'A' - 1;
 }
 
 message[i] = ch;
 }
 }
}
// Function to perform Caesar cipher decryption
void decrypt(char message[], int key) {
 int i;
 char ch;
 
 for(i = 0; message[i] != '\0'; ++i) {
 ch = message[i];
 
 if(ch >= 'a' && ch <= 'z') {
 ch = ch - key;
 
 if(ch < 'a') {
 ch = ch + 'z' - 'a' + 1;
 }
 
 message[i] = ch;
 }
 else if(ch >= 'A' && ch <= 'Z') {
 ch = ch - key;
 
 if(ch < 'A') {
 ch = ch + 'Z' - 'A' + 1;
 }
 
 message[i] = ch;
 }
 }
}
int main() {
 char message[100];
 int key;
 
 printf("Enter a message: ");
 gets(message); // Input message
 
 printf("Enter key: ");
 scanf("%d", &key); // Input key
 
 // Encrypt the message
 encrypt(message, key);
 printf("Encrypted message: %s\n", message);
 
 // Decrypt the message
 decrypt(message, key);
 printf("Decrypted message: %s\n", message);
 
 return 0;
}
## OUTPUT:
Enter a message: AJAY
Enter key: 3
Encrypted message: dmdb
Decrypted message: AJAY
## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair 
Cipher substitution technique. 

## DESIGN STEPS:

The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, 
first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with 
the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; 
other versions put both "I" and "J" in the same space). The key can be written in the top rows of the 
table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand 
corner and ending in the centre. 
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. 
To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for 
example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then 
apply the following 4 rules, to each pair of letters in the plaintext: 
1. If both letters are the same (or only one letter is left), add an "X" after the first letter. 
Encrypt the new pair and continue. Some variants of Playfair use "Q" instead of "X", but 
any letter, itself uncommon as a repeated pair, will do. 
2. If the letters appear on the same row of your table, replace them with the letters to their 
immediate right respectively (wrapping around to the left side of the row if a letter in the 
original pair was on the right side of the row). 
3. If the letters appear on the same column of your table, replace them with the letters 
immediately below respectively (wrapping around to the top side of the column if a letter in 
the original pair was on the bottom side of the column). 
4. If the letters are not on the same row or column, replace them with the letters on the same 
row respectively but at the other pair of corners of the rectangle defined by the original pair. 
The order is important – the first letter of the encrypted pair is the one that lies on the same 
row as the first letter of the plaintext pair. 
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra 
"X"s, or "Q"s that do not make sense in the final message when finished).

## PROGRAM:

 #include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <stdlib.h>

// Function to generate the Playfair cipher key table
void generateKeyTable(char key[], char keyTable[][5]) {
    int i, j, k, len;
    int flag = 0, *dicty;

    // Initialize the dictionary array
    dicty = (int *)calloc(26, sizeof(int));

    len = strlen(key);

    // Mark all characters in the key as not present in dictionary
    for (i = 0; i < len; ++i) {
        dicty[key[i] - 97] = 2;
    }

    // Inserting the key
    i = 0;
    j = 0;

    for (k = 0; k < len; ++k) {
        if (dicty[key[k] - 97] == 2) {
            dicty[key[k] - 97] -= 1;
            keyTable[i][j++] = key[k];

            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }

    // Fill the remaining elements of the table with the remaining characters
    char start = 'a';

    for (k = 0; k < 26; ++k) {
        if (dicty[k] == 0) {
            keyTable[i][j++] = start++;

            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }

    free(dicty);
}

// Function to search for the characters in the key table and return their positions
void search(char keyTable[][5], char a, char b, int arr[]) {
    int i, j;

    if (a == 'j')
        a = 'i';
    if (b == 'j')
        b = 'i';

    for (i = 0; i < 5; ++i) {
        for (j = 0; j < 5; ++j) {
            if (keyTable[i][j] == a) {
                arr[0] = i;
                arr[1] = j;
            } else if (keyTable[i][j] == b) {
                arr[2] = i;
                arr[3] = j;
            }
        }
    }
}

// Function to encrypt the message using the Playfair cipher
void encrypt(char str[], char keyTable[][5]) {
    int i, a[4];
    char a1, a2;

    for (i = 0; i < strlen(str); i += 2) {
        a1 = str[i];
        a2 = str[i + 1];

        if (a1 == a2)
            a2 = 'x';

        search(keyTable, a1, a2, a);

        // Case 1: When both the characters are in the same row
        if (a[0] == a[2]) {
            str[i] = keyTable[a[0]][(a[1] + 1) % 5];
            str[i + 1] = keyTable[a[0]][(a[3] + 1) % 5];
        }
        // Case 2: When both the characters are in the same column
        else if (a[1] == a[3]) {
            str[i] = keyTable[(a[0] + 1) % 5][a[1]];
            str[i + 1] = keyTable[(a[2] + 1) % 5][a[1]];
        }
        // Case 3: When both the characters are in different rows and columns
        else {
            str[i] = keyTable[a[0]][a[3]];
            str[i + 1] = keyTable[a[2]][a[1]];
        }
    }
}

// Function to decrypt the message using the Playfair cipher
void decrypt(char str[], char keyTable[][5]) {
    int i, a[4];
    char a1, a2;

    for (i = 0; i < strlen(str); i += 2) {
        a1 = str[i];
        a2 = str[i + 1];

        search(keyTable, a1, a2, a);

        // Case 1: When both the characters are in the same row
        if (a[0] == a[2]) {
            str[i] = keyTable[a[0]][(a[1] - 1 + 5) % 5];
            str[i + 1] = keyTable[a[0]][(a[3] - 1 + 5) % 5];
        }
        // Case 2: When both the characters are in the same column
        else if (a[1] == a[3]) {
            str[i] = keyTable[(a[0] - 1 + 5) % 5][a[1]];
            str[i + 1] = keyTable[(a[2] - 1 + 5) % 5][a[1]];
        }
        // Case 3: When both the characters are in different rows and columns
        else {
            str[i] = keyTable[a[0]][a[3]];
            str[i + 1] = keyTable[a[2]][a[1]];
        }
    }
}

int main() {
    char key[25], keyTable[5][5], str[100];
    int i, len;

    printf("Enter key: ");
    scanf("%s", key);

    printf("Enter a message: ");
    getchar(); // Consume the newline character left in the buffer
    fgets(str, sizeof(str), stdin);

    // Convert key to lowercase
    len = strlen(key);
    for (i = 0; i < len; ++i) {
        key[i] = tolower(key[i]);
    }

    // Generate key table
    generateKeyTable(key, keyTable);

    // Convert message to lowercase
    len = strlen(str);
    for (i = 0; i < len; ++i) {
        if (isalpha(str[i]))
            str[i] = tolower(str[i]);
    }

    // Encrypt the message
    encrypt(str, keyTable);
    printf("Encrypted message: %s\n", str);

    // Decrypt the message
    decrypt(str, keyTable);
    printf("Decrypted message: %s\n", str);

    return 0;
}

## OUTPUT:
Enter key: playfair
Enter a message: AJAY
Encrypted message: ikbaba
Decrypted message: mjarar

## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

 
To implement a program to encrypt and decrypt using the Hill cipher substitution 
technique. 

## DESIGN STEPS:

The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented 
by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible 
n × n matrix, again modulus 26. 
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. 
The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of 
invertible n × n matrices (modulo 26). 
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be 
done modulo the number of letters instead of modulo 26.

## PROGRAM:

#include<stdio.h>
#include<string.h>
int main() {
 unsigned int a[3][3] = { { 6, 24, 1 }, { 13, 16, 10 }, { 20, 17, 15 } };
 unsigned int b[3][3] = { { 8, 5, 10 }, { 21, 8, 21 }, { 21, 12, 8 } };
 int i, j;
 unsigned int c[20], d[20];
 char msg[20];
 int determinant = 0, t = 0;
 ;
 printf("Enter plain text\n ");
 scanf("%s", msg);
 for (i = 0; i < 3; i++) {
 c[i] = msg[i] - 65;
 printf("%d ", c[i]);
 }
 for (i = 0; i < 3; i++) {
 t = 0;
 for (j = 0; j < 3; j++) {
 t = t + (a[i][j] * c[j]);
 }
 d[i] = t % 26;
 }
 printf("\nEncrypted Cipher Text :");
 for (i = 0; i < 3; i++)
 printf(" %c", d[i] + 65);
 for (i = 0; i < 3; i++) {
 t = 0;
 for (j = 0; j < 3; j++) {
 t = t + (b[i][j] * d[j]);
 }
 c[i] = t % 26;
 }
 printf("\nDecrypted Cipher Text :");
 for (i = 0; i < 3; i++)
 printf(" %c", c[i] + 65);
 return 0;
}

## OUTPUT:
Enter plain text: AJAY
32 41 32
Encrypted Cipher Text : MOX
Decrypted Cipher Text : GPG

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:
 
To implement a program for encryption and decryption using vigenere cipher substitution 
technique

## DESIGN STEPS:

The Vigenere cipher is a method of encrypting alphabetic text by using a series of different 
Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic 
substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere 
table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted 
cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar 
ciphers. At different points in the encryption process, the cipher uses a different alphabet from one 
of the rows used. The alphabet at each point depends on a repeating keyword.

## PROGRAM:
#include<stdio.h>
#include<string.h>

// Function to perform Vigenere encryption
void vigenereEncrypt(char* text, const char* key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];

        if (c >= 'A' && c <= 'Z') {
            // Encrypt uppercase letters
            text[i] = ((c - 'A' + key[i % keyLen] - 'A') % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Encrypt lowercase letters
            text[i] = ((c - 'a' + key[i % keyLen] - 'A') % 26) + 'a';
        }
    }
}

// Function to perform Vigenere decryption
void vigenereDecrypt(char* text, const char* key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];

        if (c >= 'A' && c <= 'Z') {
            // Decrypt uppercase letters
            text[i] = ((c - 'A' - (key[i % keyLen] - 'A') + 26) % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Decrypt lowercase letters
            text[i] = ((c - 'a' - (key[i % keyLen] - 'A') + 26) % 26) + 'a';
        }
    }
}

int main() {
    const char key[] = "KEY"; // Replace with your desired key
    char message[100]; // Replace with an appropriate size for your message

    printf("Enter a message: ");
    fgets(message, sizeof(message), stdin);
    message[strcspn(message, "\n")] = '\0'; // Remove the newline character from fgets result

    // Encrypt the message
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);

    // Decrypt the message back to the original
    vigenereDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);

    return 0;
}
## OUTPUT:
Input Message : AJAY
Encrypted Message : knyi
Decrypted Message : AJAY
## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To implement a program for encryption and decryption using rail fence transposition 
technique. 

## DESIGN STEPS:

In the rail fence cipher, the plaintext is written downwards and diagonally on successive 
"rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach 
the top rail, the message is written downwards again until the whole plaintext is written out. 
The message is then read off in rows.. 

## PROGRAM:
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
// Function to perform rail fence encryption
void encrypt(char str[], int rails) {
 int i, j, len, count = 0;
 int code[100][1000];
 len = strlen(str);
 
 for(i = 0; i < rails; i++) {
 for(j = 0; j < len; j++) {
 code[i][j] = 0;
 }
 }
 
 j = 0;
 while(j < len) {
 if(count % 2 == 0) {
 for(i = 0; i < rails; i++) {
 code[i][j] = (int)str[j];
 j++;
 }
 } else {
 for(i = rails - 2; i > 0; i--) {
 code[i][j] = (int)str[j];
 j++;
 }
 }
 count++;
 }
 
 printf("Encrypted message: ");
 for(i = 0; i < rails; i++) {
 for(j = 0; j < len; j++) {
 if(code[i][j] != 0)
 printf("%c", (char)code[i][j]);
 }
 }
 printf("\n");
}
// Function to perform rail fence decryption
void decrypt(char str[], int rails) {
 int i, j, len, count = 0;
 int code[100][1000];
 len = strlen(str);
 
 for(i = 0; i < rails; i++) {
 for(j = 0; j < len; j++) {
 code[i][j] = 0;
 }
 }
 
 j = 0;
 while(j < len) {
 if(count % 2 == 0) {
 for(i = 0; i < rails; i++) {
 code[i][j] = 1; // Indicator for filled cell
 j++;
 }
 } else {
 for(i = rails - 2; i > 0; i--) {
 code[i][j] = 1; // Indicator for filled cell
 j++;
 }
 }
 count++;
 }
 
 j = 0;
 for(i = 0; i < rails; i++) {
 for(j = 0; j < len; j++) {
 if(code[i][j] == 1)
 code[i][j] = (int)str[j];
 }
 }
 
 printf("Decrypted message: ");
 for(i = 0; i < rails; i++) {
 for(j = 0; j < len; j++) {
 if(code[i][j] != 0)
 printf("%c", (char)code[i][j]);
 }
 }
 printf("\n");
}
int main() {
 char str[1000];
 int rails;
 
 printf("Enter a Secret Message\n");
 gets(str);
 
 printf("Enter number of rails\n");
 scanf("%d", &rails);
 
 // Encrypt the message
 encrypt(str, rails);
 
 // Decrypt the message
 decrypt(str, rails);
 
 return 0;
}
## OUTPUT:
Enter a Secret Message
AJAY
Enter number of rails
3
Encrypted message: 
ajya
Decrypted message: 
AJAY

## RESULT:
The program is executed successfully
