# 🔓 Many-Time Pad Attack Tool

This Python script demonstrates a **cryptanalysis technique** for breaking ciphertexts encrypted with the **same one-time pad key** (known as a *many-time pad attack*). The technique exploits **XOR-based stream ciphers**, like the one-time pad, when **the key is reused**, which violates the fundamental security requirement of the OTP.

---

## 📜 Overview

**One-Time Pad (OTP)** encryption is theoretically unbreakable—but only if each key is used **once** and only once. If a key is reused across multiple messages (a "many-time pad" situation), attackers can exploit statistical properties of plaintext (like the high frequency of spaces and letters) to recover the key and decrypt messages.

This script:
- Analyzes multiple ciphertexts encrypted with the **same key**.
- Performs **frequency analysis** to guess the most likely key bytes.
- Uses the recovered key to **decrypt a target ciphertext**.
- Displays both the **recovered key** and **partial plaintext**.

---

## 🧠 Technique Used

### XOR and Plaintext Inference

- Given `C = P ⊕ K` (ciphertext = plaintext XOR key),
- If `C1 = P1 ⊕ K` and `C2 = P2 ⊕ K` then `C1 ⊕ C2 = P1 ⊕ P2`
- If one plaintext has a space, say `P1[i] = 0x20`, then `C1[i] ⊕ 0x20 = K[i]`.

By guessing where **spaces, letters, or punctuation** are likely, we can reverse-engineer the key with reasonable confidence.

---

## 🗂️ Files

- `many_time_pad.py`: The main script with the attack logic and test data.

---

## 🏁 How to Run

1. **Install Python 3.6+** if not already installed.
2. Save the code as `many_time_pad.py`.
3. Run the script:

```bash
python many_time_pad.py
```

You should see output like:

```
Recovered key (hex): 32510ba9babebbbefd001547a810e671...
Decrypted message: The secret message is: When using a stream cipher, never use the key more than once...
Clean message: The secret message is: When using a stream cipher, never use the key more than once
```

---

## 🔍 Code Explanation

### `many_time_pad_attack()`

- **Inputs**:
  - `ciphertexts`: A list of hex-encoded ciphertexts, all encrypted with the same key.
  - `target_cipher`: A hex-encoded target ciphertext to be decrypted.

- **Steps**:
  1. Converts ciphertexts from hex to byte arrays.
  2. Loops through each byte position.
  3. For each byte in all ciphertexts, tries XORing it with:
     - ASCII space (`0x20`)
     - Letters (`a-z`, `A-Z`)
     - Common punctuation (`,.!?'`:;()[]`)
  4. Scores each resulting possible key byte.
  5. Chooses the most likely key byte (highest score) at each position.
  6. Decrypts the target ciphertext with the recovered key.

- **Outputs**:
  - `decrypted_text`: A human-readable message from the target ciphertext.
  - `recovered_key`: The best-guess bytearray of the encryption key.

---
```
Decrypted message: The secret message is: When using a stream cipher, never use the key more than once
```

---
