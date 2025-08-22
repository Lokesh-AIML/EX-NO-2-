## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:

```

Developed By : Lokeshwaran S
Register Number : 212224240080

#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define SIZE 5
char keyMatrix[SIZE][SIZE];
void prepareKeyMatrix(char key[]) {
    int alpha[26] = {0}; 
    int i, j, k = 0;
    alpha['J' - 'A'] = 1;
    for (i = 0; key[i] != '\0'; i++) {
        char ch = toupper(key[i]);
        if (ch < 'A' || ch > 'Z') continue;
        if (alpha[ch - 'A'] == 0) {
            keyMatrix[k / SIZE][k % SIZE] = ch;
            alpha[ch - 'A'] = 1;
            k++;
        }
    }
    for (i = 0; i < 26; i++) {
        if (alpha[i] == 0) {
            keyMatrix[k / SIZE][k % SIZE] = 'A' + i;
            k++;
        }
    }
}
void findPosition(char ch, int *row, int *col) {
    if (ch == 'J') ch = 'I'; 
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (keyMatrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}
int prepareText(char text[], char prepared[][2]) {
    int len = strlen(text), i, j = 0;
    for (i = 0; i < len; i++) {
        char ch = toupper(text[i]);
        if (ch < 'A' || ch > 'Z') continue;
        if (ch == 'J') 
            ch = 'I';
        if (j % 2 == 1 && prepared[j / 2][0] == ch) 
        {
            prepared[j / 2][1] = 'X';
            j++;
        }
        prepared[j / 2][j % 2] = ch;
        j++;
    }
    if (j % 2 == 1) 
    {
        prepared[j / 2][1] = 'X';
        j++;
    }
    return j / 2; 
}
void encrypt(char text[], char result[]) {
    char pairs[100][2];
    int numPairs = prepareText(text, pairs);
    int idx = 0;

    for (int i = 0; i < numPairs; i++) {
        int r1, c1, r2, c2;
        findPosition(pairs[i][0], &r1, &c1);
        findPosition(pairs[i][1], &r2, &c2);

        if (r1 == r2) 
        {
            result[idx++] = keyMatrix[r1][(c1 + 1) % SIZE];
            result[idx++] = keyMatrix[r2][(c2 + 1) % SIZE];
        } else if (c1 == c2) 
        {
            result[idx++] = keyMatrix[(r1 + 1) % SIZE][c1];
            result[idx++] = keyMatrix[(r2 + 1) % SIZE][c2];
        } else 
        {
            result[idx++] = keyMatrix[r1][c2];
            result[idx++] = keyMatrix[r2][c1];
        }
    }
    result[idx] = '\0';
}
void decrypt(char text[], char result[]) {
    int len = strlen(text);
    int idx = 0;

    for (int i = 0; i < len; i += 2) {
        int r1, c1, r2, c2;
        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2) 
        {
            result[idx++] = keyMatrix[r1][(c1 - 1 + SIZE) % SIZE];
            result[idx++] = keyMatrix[r2][(c2 - 1 + SIZE) % SIZE];
        } else if (c1 == c2) 
        {
            result[idx++] = keyMatrix[(r1 - 1 + SIZE) % SIZE][c1];
            result[idx++] = keyMatrix[(r2 - 1 + SIZE) % SIZE][c2];
        } else 
        {
            result[idx++] = keyMatrix[r1][c2];
            result[idx++] = keyMatrix[r2][c1];
        }
    }
    result[idx] = '\0';
}

int main() {
    char key[50], text[100], encrypted[200], decrypted[200];

    printf("Enter key: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0';

    prepareKeyMatrix(key);

    printf("\nPlayfair Key Matrix:\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf("%c ", keyMatrix[i][j]);
        }
        printf("\n");
    }

    printf("\nEnter text to encrypt: ");
    fgets(text, sizeof(text), stdin);
    text[strcspn(text, "\n")] = '\0';

    encrypt(text, encrypted);
    printf("\nEncrypted: %s\n", encrypted);

    decrypt(encrypted, decrypted);
    printf("Decrypted: %s\n", decrypted);

    return 0;
}

```




Output:

<img width="446" height="624" alt="image" src="https://github.com/user-attachments/assets/a6336858-5d9d-48bb-846e-7ab445e93e14" />


