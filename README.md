# EXP.NO.10-Simulation-of-Error-Detection-and-Correction-Algorithms
10.Simulation of Error Detecion and Correction Algorithms

# AIM

# SOFTWARE REQUIRED

# ALGORITHMS

# PROGRAM
   
    import numpy as np

    pb = []           # Parity matrix rows
    Ik = []           # Identity matrix
    p = []
    m = []
    h_dis = []
    r_code = []
    err = []


    col = int(input("Enter the number of parity bits: "))
    row = int(input("Enter the number of message bits: "))

    print("\nEnter the parity matrix rows:")
    for i in range(row):
        p = list(map(int, input(f"Row {i+1}: ").split()))
        if len(p) != col:
            raise ValueError(f"Each row must have {col} elements.")
        pb.append(p)


    p_mat = np.array(pb, dtype=int)
    Ik = np.eye(row, dtype=int)


    g_mat = np.hstack((Ik, p_mat))

    k = g_mat.shape[0]
    n = g_mat.shape[1]


    m = np.array([[1 if (i >> (k - j - 1)) & 1 else 0 for j in range(k)] for i in range(2 ** k)])


    c = np.mod(np.dot(m, g_mat), 2)


    for i, row in enumerate(c):
        h_dis1 = np.sum(row)
        h_dis.append(h_dis1)
    h_mat = np.array(h_dis).reshape(-1, 1)
    d_min = np.min(h_dis[1:])


    p_t = p_mat.T
    h_check = np.hstack((p_t, np.eye(col, dtype=int)))
    ht = h_check.T  # Transpose of H

    print("\n**********")
    print("Generator Matrix [G = I | P]:")
    for row in g_mat:
        print(" ".join(map(str, row)))

    print("\n**********")
    print("Message Bits\tCodeword\tHamming Weight")
    for i in range(len(m)):
        msg_str = " ".join(map(str, m[i]))
        code_str = " ".join(map(str, c[i]))
        print(f"{msg_str}\t{code_str}\t\t{h_dis[i]}")


    print("\n**********")
    print(f"Minimum Hamming Distance: {d_min}")


    print("\n**********")
    print("Parity Check Matrix [H = P^T | I]:")
    for row in h_check:
        print(" ".join(map(str, row)))


    print("\n**********")
    print("Transpose of Parity Check Matrix (H^T):")
    for row in ht:
        print(" ".join(map(str, row)))

    rc = list(map(int, input("\nEnter the received codeword: ").split()))
    if len(rc) != n:
        raise ValueError("Received codeword length must match codeword length n.")
    r_c = np.array([rc])


    e = np.mod(np.dot(r_c, ht), 2).flatten()


    err = np.zeros(n, dtype=int)
    for i in range(n):
        if np.array_equal(e, ht[i]):
            err[i] = 1
            break

    print("\n**********")
    print("Syndrome:", " ".join(map(str, e)))
    print("Error vector:", " ".join(map(str, err)))


    corrected = (r_c.flatten() + err) % 2
    print("Corrected Codeword:", " ".join(map(str, corrected)))


    print("\n**********")
    print("Syndrome Matrix:")
    for i in range(n):
        s = ht[i]
        ev = np.eye(n, dtype=int)[i]
        print(f"{' '.join(map(str, s))}  {' '.join(map(str, ev))}")

    print("**********")

# OUTPUT
![image](https://github.com/user-attachments/assets/f91acc42-2338-40a7-85a5-f3aea8f79c8b)
![image](https://github.com/user-attachments/assets/a2cc109f-c07d-4dce-8208-90a37a028c72)


 
# RESULT / CONCLUSIONS
