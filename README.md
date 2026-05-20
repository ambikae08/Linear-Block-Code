# Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
Google colab
# Program
```
import itertools
import numpy as np

p = int(input("Enter the Parity bits : "))
m = int(input("Enter the Message bits : "))

rows = []

for i in range(m):
    r = list(map(int, input(f"Enter the row values : {i+1} (Separated by space) : ").split()))
    rows.append(r)

n = m + p

# Generator Matrix
G = []

for i in range(m):

    row = rows[i] + [0] * m
    row[p + i] = 1
    G.append(row)

print("\nGenerator Matrix G\n")

for i in G:
    print(*i)

G = np.array(G)

print("\nMessage Bits   Codeword   Hamming Weight")

codewords = []
weights = []          # <-- Added this line

for msg in itertools.product([0,1], repeat=m):

    msg = np.array(msg)

    c = np.mod(np.dot(msg, G), 2)

    codewords.append(c)

    weight = sum(c)   # <-- Added this line
    weights.append(weight)

    print(*msg, "   ", *c, "   ", weight)

# Minimum Hamming Distance
non_zero_weights = [w for w in weights if w != 0]

dmin = min(non_zero_weights)

print("\nMinimum Hamming Distance =", dmin)

# Parity Check Matrix
P = G[:,0:p]

H = np.concatenate((np.identity(p, dtype=int), P.T), axis=1)

print("\nParity Check Matrix H\n")

for i in H:
    print(*i)

# Syndrome Table
print("\nError Pattern   Syndrome")

for i in range(n):

    e = [0] * n
    e[i] = 1

    s = np.mod(np.dot(H, np.array(e).T), 2)

    print(*e, "   ", *s)

# Error Detection
r = np.array(list(map(int, input(f"\nEnter Received Codeword (length {n}, separated by space) : ").split())))

s = np.mod(np.dot(H, r.T), 2)

print("Syndrome :", *s)

if np.all(s == 0):

    print("No Error")
    print("Correct Codeword :", *r)

else:

    print("Error Detected")

    error_pos = -1

    for i in range(n):

        if np.array_equal(H[:, i], s):

            error_pos = i
            break

    if error_pos != -1:

        print("Error Position :", error_pos + 1)

        r[error_pos] = (r[error_pos] + 1) % 2

        print("Correct Codeword :", *r)
```
# Output 
<img width="537" height="778" alt="image" src="https://github.com/user-attachments/assets/ab7e9ed3-cbae-48ca-bc3f-064344f78417" />
# Verification

<img width="1084" height="1600" alt="image" src="https://github.com/user-attachments/assets/8857356a-8426-4844-b1cf-543c3bb0b8f7" />

<img width="811" height="1600" alt="image" src="https://github.com/user-attachments/assets/b161a158-e4cd-49eb-bd45-1e5a8932faf7" />

<img width="1926" height="3257" alt="image" src="https://github.com/user-attachments/assets/41cdab84-f2c7-4584-932d-ef72c952a6c5" />


# Results

Thus linear block code operation for the given input is successfully verified.

