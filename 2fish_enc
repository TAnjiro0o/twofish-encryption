from collections import deque
RS_matrix=[[0x01,0xA4,0x55,0x87,0x5A,0x58,0xDB,0x9E],[0xA4,0x56,0x82,0xF3,0x1E,0xC6,0x68,0xE5],[0x02,0xA1,0xFC,0xC1,0x47,0xAE,0x3D,0x19],[0xA4,0x55,0x87,0x5A,0x58,0xDB,0x9E,0x03]]
tq0=[[0x8,0x1,0x7,0xD,0x6,0xF,0x3,0x2,0x0,0xB,0x5,0x9,0xE,0xC,0xA,0x4],[0xE,0xC,0xB,0x8,0x1,0x2,0x3,0x5,0xF,0x4,0xA,0x6,0x7,0x0,0x9,0xD],[0xB,0xA,0x5,0xE,0x6,0xD,0x9,0x0,0xC,0x8,0xF,0x3,0x2,0x4,0x7,0x1],[0xD,0x7,0xF,0x4,0x1,0x2,0x6,0xE,0x9,0xB,0x3,0x0,0x8,0x5,0xC,0xA]]
tq1=[[0x2,0x8,0xB,0xD,0xF,0x7,0x6,0xE,0x3,0x1,0x9,0x4,0x0,0xA,0xC,0x5],[0x1,0xE,0x2,0xB,0x4,0xC,0x3,0x7,0x6,0xD,0xA,0x5,0xF,0x9,0x0,0x8],[0x4,0xC,0x7,0x5,0x1,0x6,0x9,0xA,0x0,0xE,0xD,0x8,0x2,0xB,0x3,0xF],[0xB,0x9,0x5,0x1,0xC,0x3,0xD,0xE,0x6,0x4,0x7,0xF,0x2,0x0,0x8,0xA]]
MDS=[[0x01,0xEF,0x5B,0x5B],[0x5B,0xEF,0xEF,0x01],[0xEF,0x5B,0x01,0xEF],[0xEF,0x01,0xEF,0x5B]]
gf_mod = 2**8 + 2**6 + 2**5 + 2**3 + 1
rs_mod = 2**8 + 2**6 + 2**3 + 2**2 + 1
t=[tq0,tq1]
S0=[]
S1=[]
def gf2n_multiply(a, b,modulus):
    overflow = 0x100
    sum1 = 0
    while (b > 0):
        if (b & 1):
            sum1 = sum1 ^ a
        b = b >> 1
        a = a << 1
        if (a & overflow):
            a = a ^ modulus
    return sum1
def PHT(a,b):
    num1=(a+b)%(pow(2,32))
    num2=(a+2*b)%pow(2,32)
    return num1,num2
def ROL(num,rot,bits):
    num=bin(num)[2:]
    num=num.zfill(bits)
    num=[int(i) for i in num]
    items=deque(num)
    items.rotate(-rot)
    num=list(items)
    num=''.join([str(i) for i in num])
    num=int(num,2)
    return num
def ROR(num,rot,bits):
    num=bin(num)[2:]
    num=num.zfill(bits)
    num=[int(i) for i in num]
    items=deque(num)
    items.rotate(rot)
    num=list(items)
    num=''.join([str(i) for i in num])
    num=int(num,2)
    return num
def q0(inp):
    t0=t[0][0]
    t1=t[0][1]
    t2=t[0][2]
    t3=t[0][3]
    inp=bin(inp)[2:]
    inp=inp.zfill(8)
    a0=int(inp[:4],2)
    b0=int(inp[4:],2)
    a1=a0^b0
    b1=a0^(ROR(b0,1,4))^((8*a0)%16)
    a2=t0[a1]
    b2=t1[b1]
    a3=a2^b2
    b3=a2^(ROR(b2,1,4))^((8*a2)%16)
    a4=t2[a3]
    b4=t3[b3]
    y=16*b4+a4
    return y
def q1(inp):
    t0=t[1][0]
    t1=t[1][1]
    t2=t[1][2]
    t3=t[1][3]
    inp=bin(inp)[2:]
    inp=inp.zfill(8)
    a0=int(inp[:4],2)
    b0=int(inp[4:],2)
    a1=a0^b0
    b1=a0^(ROR(b0,1,4))^((8*a0)%16)
    a2=t0[a1]
    b2=t1[b1]
    a3=a2^b2
    b3=a2^(ROR(b2,1,4))^((8*a2)%16)
    a4=t2[a3]
    b4=t3[b3]
    y=16*b4+a4
    return y
def g_function(inp_r):
    global S0,S1
    S_0=S0
    S_1=S1
    arr=[]
    h=hex(inp_r)[2:].zfill(8)
    for i in range(0,len(h),2):
        tmp=int(h[i:i+2],16)
        arr.append(tmp)
    arr=arr[::-1]
    inp0=arr[0]
    inp1=arr[1]
    inp2=arr[2]
    inp3=arr[3]
    output=[0,0,0,0]
    output[0] = q1(q0(q0(inp0) ^ S_0[0]) ^ S_1[0])
    output[1] = q0(q0(q1(inp1) ^ S_0[1]) ^ S_1[1])
    output[2] = q1(q1(q0(inp2) ^ S_0[2]) ^ S_1[2])
    output[3] = q0(q1(q1(inp3) ^ S_0[3]) ^ S_1[3])
    output=mat_mul(MDS,output,gf_mod)
    output=output[::-1]
    output=int(''.join([bin(i)[2:].zfill(8) for i in output]),2)
    return output
def f_function(r_array,k1,k2):
    r0=r_array[0]
    r1=r_array[1]
    r1=ROL(r1,8,32) 
    t0=g_function(r0)
    t1=g_function(r1)
    t0,t1=PHT(t0,t1)
    f0=(t0+k1)%pow(2,32)
    f1=(t1+k2)%pow(2,32)
    return f0,f1
def helper_h(inp1,M1,M2):
    output=[0,0,0,0]
    output[0] = q1(q0(q0(inp1) ^ M1[0]) ^ M2[0])
    output[1] = q0(q0(q1(inp1) ^ M1[1]) ^ M2[1])
    output[2] = q1(q1(q0(inp1) ^ M1[2]) ^ M2[2])
    output[3] = q0(q1(q1(inp1) ^ M1[3]) ^ M2[3])
    output=mat_mul(MDS,output,gf_mod)
    return output
def h_function(M_even,M_odd):
    M0=M_even[0]
    M2=M_even[1]
    M1=M_odd[0]
    M3=M_odd[1]
    K_keys=[]
    for i in range(0,40,2):
        inp1=i
        inp2=i+1
        key1=helper_h(inp1,M2,M0)
        key2=helper_h(inp2,M3,M1)
        fin_key1=[]
        fin_key2=[]
        for i in range(4):
            fin_key1.append(bin(key1[i])[2:].zfill(8))
            fin_key2.append(bin(key2[i])[2:].zfill(8))
        fin_key1=fin_key1[::-1]
        fin_key2=fin_key2[::-1]
        key1=int(''.join(fin_key1),2)
        key2=int(''.join(fin_key2),2)
        key2=ROL(key2,8,32)
        key1,key2=PHT(key1,key2)
        key2=ROL(key2,9,32)
        K_keys.append(key1)
        K_keys.append(key2)
    return K_keys
def mat_mul(mat1,mat2,modulus):
    row1=len(mat1)
    col1=len(mat1[0])
    fin=[]
    for i in range(row1):
        val=0
        for j in range(col1):
            tmp1=gf2n_multiply((mat1[i][j]),mat2[j],modulus)
            val=val^tmp1
        fin.append(val)
    return fin
def key_schedule(key):
    global S0,S1
    m_array=[] 
    for i in range(0,len(key),2):
        tmp=int(key[i:i+2],16)
        m_array.append(tmp)
    S0=mat_mul(RS_matrix,m_array[:8],rs_mod)
    S1=mat_mul(RS_matrix,m_array[8:16],rs_mod)
    M_even=[]
    M_odd=[]
    val=0
    for i in range(0,len(m_array),4):
        tmp=m_array[i:i+4]
        if(val%2==0):
            M_even.append(tmp)
        else:
            M_odd.append(tmp)
        val+=1
    K_keys=h_function(M_even,M_odd)
    return K_keys
def whitening(plaintext,white_keys):
    plain=[]
    new_key=[]
    val=0
    for i in range(0,len(plaintext),2):
        tmp=int(plaintext[i:i+2],16)
        plain.append(tmp)
    arr2=[]
    for i in range(0,len(plain),4):
        tmp=plain[i:i+4]
        tmp=tmp[::-1]  
        arr2+=tmp
    plain=arr2
    for j in range(len(white_keys)):
        x=hex(white_keys[j])[2:].zfill(8)
        for k in range(0,len(x),2):
            tmp=int(x[k:k+2],16)
            new_key.append(tmp)
    r_array=[]
    for i in range(len(plain)):
        r_array.append(new_key[i]^plain[i])
    r0=r_array[:4]
    r1=r_array[4:8]
    r2=r_array[8:12]
    r3=r_array[12:16]
    r_array=[r0,r1,r2,r3]
    return r_array
def encrypt(plaintext,key):
    # Making the required keys
    round_keys=key_schedule(key)
    white_keys=round_keys[:4]
    output_keys=round_keys[4:8]
    # Whitening the Input
    r1_array=whitening(plaintext,white_keys)
    r_array=[]
    # Converting the array to a 16 8-bit numbers from 4 32-bit number
    for i in r1_array:
        num=int("".join([bin(j)[2:].zfill(8) for j in i]),2)
        r_array.append(num)
    # looping 16 time for each round
    for r in range(16):
        # Calling F function
        f0,f1=f_function(r_array,round_keys[2*r+8],round_keys[2*r+9])
        c2=f0^r_array[2]
        c2=ROR(c2,1,32)
        r3=r_array[3]
        c3=ROL(r3,1,32)
        c3=f1^c3
        r_array=[c2,c3,r_array[0],r_array[1]]
    # undo the steps
    r_array=[r_array[2],r_array[3],r_array[0],r_array[1]]
    # printing the output
    ciphertext=[]
    for i in range(len(output_keys)):
        ciphertext.append(hex(output_keys[i]^r_array[i])[2:].zfill(8))
    # converting
    output=""
    for i in ciphertext:
        ans=[i[j:j+2] for j in range(0,len(i),2)]
        ans=ans[::-1]
        output+=''.join(ans)
    return(output)
key=input("Enter the key 128 bit (Hexadecimal) : ")
key=key.zfill(32)
plaintext=input("Enter the plaintext 128 bit (Hexadecimal) : ")
plaintext=plaintext.zfill(32)[:32]
#plaintext=plaintext.encode('utf-8').hex()
#plaintext=plaintext.zfill(32)[:32]
print("The Ciphertext is : ")
print(encrypt(plaintext,key))
