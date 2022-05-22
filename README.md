# Tugas Akhir Praktikum Pemodelan Oseanografi Kelompok 4
Repositori ini dibuat untuk memenuhi tugas akhir Praktikum Pemodelan Oseanografi 2022. Repositori ini mengandung seluruh _script_ pemrograman _python_ yang digunakan dalam Praktikum Pemodelan Oseanografi. Library yang digunakan pada Praktikum Pemodelan Oseanografi 2022 ini adalah _Numpy_ , _Matplotlib_ , _Sys_ dan _NDBC_ , 
## **MEMBER**
1. Fatiha Hening Prastiwi
2. Laila Atika Putri 26050120130118 A
3. Rizqi Abdul Aziz 26050120130072 A
4. Shafina Amalia Yahya 26050120140169 A
5. Tsulasa Zuhrotun Nisak 26050120130093 A
6. Umar Haidar Al Faruq
7. Yustinus Adyaksa Indrayana

## **MODUL SERTA METODE PENGERJAAN**
* **MODUL 1 : Persamaan Adveksi Difusi 1D** 

Pada modul kali ini 

* **MODUL 2 : Persamaan Adveksi Difusi 2D** 

Pada modul ini 
* **MODUL 3 : Persaman Hidrodinamika 1D**

Pada modul ini praktikan membahas persamaan Hidrodinamika 1D. Hidrodinamika adalah cabang dari mekanika fluida, khususnya zat cair _incompressible_ yang dipengaruhi oleh gaya inernal dan eksternal. Dalam hidrodinamika laut, gaya - gaya terpenting ada gaya gravitasi, gesek dan coriolis. Model Hidrodinamika yang dibahas pada modul ini adalah model yang dibangun dari adanya proses - proses yang mempengaruhi pergerakkan massa air (pasang surut, arus dan gelombang). Model hidrodinamika ini dibangun berdasarkan Hukum Konservasi Massa dan Hukum Momentum.

Untuk contoh script pemodelan Hidrodinamika 1D kali ini merupakan penyelesaian perhitungan untuk mengetahui bagaimana hubungan antara elevasi muka air laut dan juga kecepatan arus terhadap ruang dan waktu yang dibuat menggunakan bahasa pemrograman Phyton dengan menggunakan dua library yaitu Matplotlib dan juga Numpy

`import matplotlib.pyplot as plt
import numpy as np`

Penginputan nilai-nilai parameter

`#Proses Awal
p = 5000         #Panjang Grid
T = 1200         #Waktu Simulasi
A = 0.5          #Amplitudo
D = 15           #Depth/Kedalaman
dt = 2
dx = 100
To = 300         #Periode

#Parameter Lanjutan
g = 9.8          #Koefisien Gravitasi
pi = np.pi 
C = np.sqrt(g*D) #Kecepatan Arus
s = 2*pi/To      #Kecepatan Sudut Gelombang
L = C*To         #Panjang Gelombang
k = 2*pi/L       #Koefisien Panjang Gelombang
Mmax = int(p//dx)
Nmax = int(T//dt)`

Penyelesaian persamaan Hidrodinamika 1D

`zo = [None for _ in range(Mmax)]
uo = [None for _ in range(Mmax)]

hasilu = [None for _ in range(Nmax)]
hasilz = [None for _ in range(Nmax)]

for i in range(1, Mmax+1):
  zo[i-1] = A*np.cos(k*(i)*dx)
  uo[i-1] = A*C*np.cos(k*((i)*dx+(0.5)*dx))/(D+zo[i-1])
for i in range(1, Nmax+1):
  zb = [None for _ in range(Mmax)]
  ub = [None for _ in range(Mmax)]
  zb[0] = A*np.cos(s*(i)*dt)
  ub[-1] = A*C*np.cos(k*L-s*(i)*dt)/(D+zo[-1])
  for j in range (1, Mmax):
    ub[j-1] = uo[j-1]-g*(dt/dx)*(zo[j]-zo[j-1])
  for k in range(2, Mmax+1):
    zb[k-1] = zo[k-1]-(D+zo[k-1])*(dt/dx)*(ub[k-1]-ub[k-2])
    hasilu[i-1] = ub
    hasilz[i-1] = zb
  for p in range(0, Mmax):
    uo[p] = ub[p]
    zo[p] = zb[p]`
 

* **MODUL 4 : Persamaan Hidrodinamika 2D**

Pada modul ini

## **PENUTUP**

## **UCAPAN TERIMA KASIH**
