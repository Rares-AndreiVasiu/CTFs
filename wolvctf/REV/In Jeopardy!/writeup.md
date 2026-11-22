# Writeup 

## General info stage

**Target**:

-  in-jeopardy (ELF x86-64, ET_EXEC) 

**Behavior**: 

- Jeopardy-style checker. 
- Expects an exact input; on success prints “Correct!” and reveals the flag.

## **Recon and static analysis** 

-  Strings reconnaissance exposed interesting values
- “What is ” (0x55a08c)
- “Incorrect…” (0x55a098)
- “Correct!” (0x55a02a)
- and an obfuscated blob “zfwi~Fk744Hqj680i3UbwZ30KXQGU6G” (0x55a008)
- in main() at 0x404cc4 it Reads a line from stdin. 
- Calls key_abi_cxx11_ (0x404a55), which constructs the token by appending characters one-by-one.
-  Compares the user input to “What is ” + that token (“What is the flag?!”)
- On match, calls print_key_message (0x404b3e); otherwise prints “Incorrect …” (branch depends on whether the input starts with “What is ”).
- print_key_message at 0x404b3e it prints “Correct!”.  - Loads the ciphertext string at 0x55a008 (“zfwi~Fk744Hqj680i3UbwZ30KXQGU6G”) and decodes it by iterating each byte and applying a Caesar shift of −3 
-  then prints the decoded plaintext (the flag)
- Flag: ~~wctf{Ch411Eng35-f0R_tW0-HUNDR3D}~~