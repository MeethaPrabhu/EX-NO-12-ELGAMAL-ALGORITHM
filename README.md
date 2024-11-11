# EX-NO-12-ELGAMAL-ALGORITHM

## AIM:
To Implement ELGAMAL ALGORITHM

## ALGORITHM:

1. ElGamal Algorithm is a public-key cryptosystem based on the Diffie-Hellman key exchange and relies on the difficulty of solving the discrete logarithm problem.

2. Initialization:
   - Select a large prime \( p \) and a primitive root \( g \) modulo \( p \) (these are public values).
   - The receiver chooses a private key \( x \) (a random integer), and computes the corresponding public key \( y = g^x \mod p \).

3. Key Generation:
   - The public key is \( (p, g, y) \), and the private key is \( x \).

4. Encryption:
   - The sender picks a random integer \( k \), computes \( c_1 = g^k \mod p \), and \( c_2 = m \times y^k \mod p \), where \( m \) is the message.
   - The ciphertext is the pair \( (c_1, c_2) \).

5. Decryption:
   - The receiver computes \( s = c_1^x \mod p \), and then calculates the plaintext message \( m = c_2 \times s^{-1} \mod p \), where \( s^{-1} \) is the modular inverse of \( s \).

6. Security: The security of the ElGamal algorithm relies on the difficulty of solving the discrete logarithm problem in a large prime field, making it secure for encryption.

## Program:
```
#include <stdio.h>
#include <stdlib.h>
#include <gmp.h>
#include <time.h>
#include <string.h>
#define NAME "Meetha"
void mod_exp(mpz_t result, mpz_t base, mpz_t exp, mpz_t mod) {
 mpz_powm(result, base, exp, mod);
}
void generate_random(mpz_t result, mpz_t max) {
 gmp_randstate_t state;
 gmp_randinit_default(state);
 gmp_randseed_ui(state, time(NULL));
 mpz_urandomm(result, state, max);
 gmp_randclear(state);
}
int main() {
 mpz_t p, g, x, y, k, m, c1, c2, s, recovered_m;
 mpz_inits(p, g, x, y, k, m, c1, c2, s, recovered_m, NULL);
 printf("********** ELGAMAL ENCRYPTION AND DECRYPTION *********\n\n");
 mpz_set_str(p, "467", 10);
 mpz_set_ui(g, 2);
 generate_random(x, p);
 mod_exp(y, g, x, p);
 gmp_printf("Private key (x): %Zd\n", x);
 gmp_printf("Public key (y): %Zd\n\n", y);
 printf("Original name: %s\n", NAME);
 int name_len = strlen(NAME);
 int ascii_values[name_len];
 mpz_t c1_array[name_len], c2_array[name_len];
 for (int i = 0; i < name_len; i++) {
 ascii_values[i] = (int)NAME[i];
 printf("ASCII of '%c': %d\n", NAME[i], ascii_values[i]);
 }
 printf("\nEncrypting each ASCII value...\n");
 for (int i = 0; i < name_len; i++) {
 mpz_set_ui(m, ascii_values[i]);
 generate_random(k, p);
 mod_exp(c1, g, k, p);
 mpz_init(c1_array[i]);
 mpz_init(c2_array[i]);
 mpz_set(c1_array[i], c1);
 mpz_t yk;
 mpz_init(yk);
 mod_exp(yk, y, k, p);
 mpz_mul(c2, m, yk);
 mpz_mod(c2, c2, p);
 mpz_set(c2_array[i], c2);
 gmp_printf("Encrypted ASCII for '%c' -> (c1, c2): (%Zd, %Zd)\n", NAME[i], 
c1_array[i], c2_array[i]);
 mpz_clear(yk);
 }
 printf("\nDecrypting each ASCII value...\n");
 for (int i = 0; i < name_len; i++) {
 mpz_set(c1, c1_array[i]);
 mpz_set(c2, c2_array[i]);
 mod_exp(s, c1, x, p);
 mpz_invert(s, s, p);
 mpz_mul(recovered_m, c2, s);
 mpz_mod(recovered_m, recovered_m, p);
 int decrypted_ascii = mpz_get_ui(recovered_m);
 printf("Decrypted ASCII: %d -> '%c'\n", decrypted_ascii, (char)decrypted_ascii);
 }
 mpz_clears(p, g, x, y, k, m, c1, c2, s, recovered_m, NULL);
 for (int i = 0; i < name_len; i++) {
 mpz_clear(c1_array[i]);
 mpz_clear(c2_array[i]);
 }
 return 0;
}
```

## Output:
![image](https://github.com/user-attachments/assets/4069a446-3ec5-4de2-a053-3fd47077c0cc)


## Result:
The program is executed successfully.
