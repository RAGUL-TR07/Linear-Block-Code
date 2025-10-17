# Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
C0-Lab
# Program
```
import numpy as np

# --- INPUT SECTION ---
parity_bits = int(input("Enter the number of parity bits (r): "))
message_bits = int(input("Enter the number of message bits (k): "))

# Parity matrix input
print("\nEnter the Parity matrix (P) values row-wise:")
P = []
for i in range(message_bits):
    row = list(map(int, input(f"Row {i+1}: ").split()))
    P.append(row)
P = np.array(P, dtype=int)

# --- GENERATOR MATRIX ---
I_k = np.eye(message_bits, dtype=int)
G = np.hstack((P, I_k))
n, k = G.T.shape

print("\nGenerator Matrix (G = [P | Iₖ]):")
for r in G:
    print(" ".join(map(str, r)))

# --- MESSAGE AND CODEWORD GENERATION ---
messages = np.array([[1 if (i >> (k - j - 1)) & 1 else 0 for j in range(k)] for i in range(2 ** k)])
codewords = np.mod(np.dot(messages, G), 2)

# Hamming weight and minimum distance
weights = [np.sum(row) for row in codewords]
d_min = np.min(weights[1:])

print("\nMessage Bits\tCodeword\tHamming Weight")
for i in range(len(messages)):
    print(f"{''.join(map(str, messages[i]))}\t\t{''.join(map(str, codewords[i]))}\t\t{weights[i]}")

print(f"\nMinimum Hamming Distance (d_min): {d_min}")

# --- PARITY CHECK MATRIX ---
H = np.hstack((np.eye(n - k, dtype=int), P.T))
Ht = H.T

print("\nParity Check Matrix (H = [I | Pᵗ]):")
for r in H:
    print(" ".join(map(str, r)))

print("\nTranspose of Parity Check Matrix (Hᵗ):")
for r in Ht:
    print(" ".join(map(str, r)))

# --- RECEIVED CODEWORD AND SYNDROME CALCULATION ---
r_code = list(map(int, input("\nEnter the received (possibly erroneous) codeword: ").split()))
r_code = np.array(r_code).reshape(1, -1)

syndrome = np.mod(np.dot(r_code, Ht), 2)
print(f"\nSyndrome: {' '.join(map(str, syndrome[0]))}")

# --- ERROR DETECTION AND CORRECTION ---
error = np.zeros(n, dtype=int)
for i in range(n):
    if np.array_equal(syndrome[0], Ht[i, :]):
        error = np.eye(n, dtype=int)[i, :]
        break

if np.any(error):
    print(f"Error detected at bit position: {np.where(error == 1)[0][0] + 1}")
else:
    print("No error detected.")

corrected_codeword = np.mod(r_code + error, 2)
print(f"Corrected Codeword: {' '.join(map(str, corrected_codeword[0]))}")

```
# Output
![linear block code](https://github.com/user-attachments/assets/71f9cc59-2364-4b1c-a22f-2f9d5ad39b24)
![WhatsApp Image 2025-09-28 at 22 07 50_8d4285d2](https://github.com/user-attachments/assets/ee8489a7-7d86-459e-b678-beabd9dfc653)
![WhatsApp Image 2025-09-28 at 22 07 50_f9aeba5d](https://github.com/user-attachments/assets/aff05993-c1ef-4d11-989b-12ea0af0188f)
# Results

Thus linear block code operation for the given input is successfully verified.

