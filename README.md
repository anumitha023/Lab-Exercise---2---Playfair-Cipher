# Lab-Exercise---2---Playfair-Cipher

# Aim:
To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

# Procedure:
# Step 1:
Design of PlayFair Cipher algorithm
# Step 2:
Implementation using C or Python Code.
# Step 3:
Testing algorithm with different key values.ayfair Cipher using with different key values.

# ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre. The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1. If both letters are the same(or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2. If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively(wrapping around to the left side of the row if a letter in the original pair was on the right side pf the row).
3. If the letters are not on the same column of your table, replace them with the letters immediately below respectively(wrapping around to the top side of the column if a letter in the original pair was on the bottom side of the column).
4. If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair. The order is important - the first letter of the encrypted pair is the one that lies on the same row as the first letter of the plaintext pair. To decrypt, use he INVERSE(opposite) of the last 3 rules and the 1st as-is(dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).

# Program:
```
def generate_matrix(key):
    key = key.upper().replace("J", "I")
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
    text = ""

    for ch in key + alphabet:
        if ch.isalpha() and ch not in text:
            text += ch

    return [list(text[i:i+5]) for i in range(0, 25, 5)]

def prepare_text(text):
    text = text.upper().replace("J", "I")
    text = ''.join(ch for ch in text if ch.isalpha())
    pairs = []
    i = 0

    while i < len(text):
        a = text[i]

        if i + 1 == len(text):
            b = 'X'
            i += 1
        elif text[i] == text[i + 1]:
            b = 'X'
            i += 1
        else:
            b = text[i + 1]
            i += 2

        pairs.append(a + b)

    return pairs

def playfair_encrypt(plaintext, key):
    matrix = generate_matrix(key)
    pairs = prepare_text(plaintext)
    ciphertext = ""

    def position(ch):
        for r in range(5):
            for c in range(5):
                if matrix[r][c] == ch:
                    return r, c

    for pair in pairs:
        r1, c1 = position(pair[0])
        r2, c2 = position(pair[1])

        if r1 == r2:  # Same row
            ciphertext += matrix[r1][(c1 + 1) % 5]
            ciphertext += matrix[r2][(c2 + 1) % 5]

        elif c1 == c2:  # Same column
            ciphertext += matrix[(r1 + 1) % 5][c1]
            ciphertext += matrix[(r2 + 1) % 5][c2]

        else:  # Rectangle rule
            ciphertext += matrix[r1][c2]
            ciphertext += matrix[r2][c1]

    return matrix, pairs, ciphertext

key = input("Enter the key: ")
plaintext = input("Enter the plaintext: ")

matrix, pairs, encrypted = playfair_encrypt(plaintext, key)

print("\nKey Matrix:")
for row in matrix:
    print(' '.join(row))

print("\nPlaintext pairs:", ' '.join(pairs))
print("Encrypted text:", encrypted)
```
# Output:
<img width="527" height="441" alt="image" src="https://github.com/user-attachments/assets/3eb3ab84-baaa-4238-8945-92093f4163cf" />

# Result:
The Program is executed successfully.
