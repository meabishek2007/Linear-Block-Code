# Linear-Block-Code
# Aim

Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 

# Tools required:

Python IDE with Numpy and Scipy

# Program

```
import itertools
import numpy as np
p=int(input("Enter the Parity bits : "))
m=int(input("Enter the Message bits : "))
rows=[]
for i in range(m):
    r=list(map(int,input(f"Enter the row values : {i+1} (Separated by space) : ").split()))
    rows.append(r)
n=m+p
# Generator Matrix
G=[]
for i in range(m):
    row=rows[i]+[0]*m
    row[p+i]=1
    G.append(row)
print("\nGenerator Matrix G\n")
for i in G:
    print(*i)
G=np.array(G)
print("\nMessage Bits  Codeword  Hamming Weight")
codewords=[]
for msg in itertools.product([0,1],repeat=m):
    msg=np.array(msg)
    c=np.mod(np.dot(msg,G),2)
    codewords.append(c)
    print(*msg," ",*c," ",sum(c))
# Minimum Hamming Distance
non_zero_weights=[w for w in weights if w!=0]
dmin=min(non_zero_weights)
print("\nMinimum Hamming Distance =",dmin)
# Parity Check Matrix
P=G[:,0:p]
H=np.concatenate((np.identity(p,dtype=int),P.T),axis=1)
print("\nParity Check Matrix H\n")
for i in H:
    print(*i)
# Syndrome Table
print("\nError Pattern   Syndrome")
for i in range(n):
    e=[0]*n
    e[i]=1
    s=np.mod(np.dot(H,np.array(e).T),2)
    print(*e,"   ",*s)
# Error Detection
r=np.array(list(map(int,input("\nEnter Received Codeword : ").split())))
s=np.mod(np.dot(H,r.T),2)
print("Syndrome :",*s)
if np.all(s==0):
    print("No Error")
    print("Correct Codeword :",*r)
else:
    print("Error Detected")
    error_pos=-1
    for i in range(n): 
        if np.array_equal(H[:,i],s):
            error_pos=i
            break
    if error_pos!=-1:
        print("Error Position :",error_pos+1)
        r[error_pos]=(r[error_pos]+1)%2
        print("Correct Codeword :",*r)
```
# Verifiaction

<img width="931" height="1599" alt="image" src="https://github.com/user-attachments/assets/069dd594-84d3-42c1-9b52-16d8b4a78d37" />

<img width="820" height="1280" alt="image" src="https://github.com/user-attachments/assets/fb666e9d-cf87-4d73-969a-69806285ff51" />



# Output Waveform
```
Enter the Parity bits : 3
Enter the Message bits : 3
Enter the row values 1 (Separated by space) : 1 1 0
Enter the row values 2 (Separated by space) : 0 1 0
Enter the row values 3 (Separated by space) : 1 0 1

Generator Matrix G

1 1 0 1 0 0
0 1 0 0 1 0
1 0 1 0 0 1

Message Bits   Codeword   Hamming Weight
0 0 0     0 0 0 0 0 0     0
0 0 1     1 0 1 0 0 1     3
0 1 0     0 1 0 0 1 0     2
0 1 1     1 1 1 0 1 1     5
1 0 0     1 1 0 1 0 0     3
1 0 1     0 1 1 1 0 1     4
1 1 0     1 0 0 1 1 0     3
1 1 1     0 0 1 1 1 1     4

Minimum Hamming Distance = 2

Parity Check Matrix H

1 0 0 1 0 1
0 1 0 1 1 0
0 0 1 0 0 1

Error Pattern   Syndrome
1 0 0 0 0 0     1 0 0
0 1 0 0 0 0     0 1 0
0 0 1 0 0 0     0 0 1
0 0 0 1 0 0     1 1 0
0 0 0 0 1 0     0 1 0
0 0 0 0 0 1     1 0 1

Enter Received Codeword : 1 0 1 0 0 0
Syndrome : 1 0 1
Error Detected
Error Position : 6
Correct Codeword : 1 0 1 0 0 1

```
# Results

Thus linear block code operation for the given input is successfully verified.
