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

Pada modul ini praktikan mempelajari tentang persamaan Adveksi Difusi 2D. Pada umumnya, persamaan Adveksi Difusi 2D digunakan untuk mempelajari penyebaran polutan pada perairan dengan domain yang teratur dan menggunakan koefisien yang konstan, sedangkan persoalan yang berada disekitar kita menyatakan sebaliknya. Walaupun demikian, metode beda hingga tetap menyediakan solusi numerik yang akurat dan dapat dipakai untuk mendapatkan solusi numerik yang diinginkan. Selain itu, persamaan ini juga dapat digunakan untuk mengetahui pola sebaran senyawa pada air, distribusi temperatur, tingkat transfer oksigen, dan masih banyak lagi. Kemudian dari praktikum yang telah dilakukan, praktikan menggunakan persamaan ini untuk mempelajari pergerakan polutan berdasarkan data yang diberikan oleh asisten. Hasil dari praktikum ini didapat bahwa apabila C=0 dan Ad=1 maka polutan akan menyebar di satu titik tanpa bergerak ke arah manapun. Sedangkan jika C=1 dan Ad=0 maka polutan akan bergerak sesuai dengan arah aliran arus, namun penyebarannya akan sangat lambat atau bahkan tidak terjadi penyebaran. Selain itu diketahui juga bahwa arus memberikan peranan dalam proses adveksi difusi 2 dimensi dengan pergerakan polutan yang sesuai dengan arah arus, sedangkan koefisien adveksi difusi memberikan gambaran proses transport konsentrasi polutan ke segala arah. Kecepatan pergerakan dan penyebaran tergantung pada kecepatan arus. 

* **MODUL 3 : Persaman Hidrodinamika 1D**

Pada modul ini praktikan membahas persamaan Hidrodinamika 1D. Hidrodinamika adalah cabang dari mekanika fluida, khususnya zat cair _incompressible_ yang dipengaruhi oleh gaya inernal dan eksternal. Dalam hidrodinamika laut, gaya - gaya terpenting ada gaya gravitasi, gesek dan coriolis. Model Hidrodinamika yang dibahas pada modul ini adalah model yang dibangun dari adanya proses - proses yang mempengaruhi pergerakkan massa air (pasang surut, arus dan gelombang). Model hidrodinamika ini dibangun berdasarkan Hukum Konservasi Massa dan Hukum Momentum.

Untuk contoh script pemodelan Hidrodinamika 1D kali ini merupakan penyelesaian perhitungan untuk mengetahui bagaimana hubungan antara elevasi muka air laut dan juga kecepatan arus terhadap ruang dan waktu yang dibuat menggunakan bahasa pemrograman Phyton dengan menggunakan dua library yaitu Matplotlib yang berfungsi untuk menampilkan visualisasi hasil code dan juga Numpy yang berfungsi untuk perhintungan scientific

`import matplotlib.pyplot as plt`
`import numpy as np`

**Penginputan nilai parameter**
```
Proses Awal
p = 5000         
T = 1200         
A = 0.5         
D = 15           
dt = 2
dx = 100
To = 300         

Parameter Lanjutan
g = 9.8   
pi = np.pi 
C = np.sqrt(g*D) 
s = 2*pi/To      
L = C*To         
k = 2*pi/L       
Mmax = int(p//dx)
Nmax = int(T//dt)`
```

**Penyelesaian persamaan Hidrodinamika 1D**

```
zo = [None for _ in range(Mmax)]
uo = [None for _ in range(Mmax)]

hasilu = [None for _ in range(Nmax)]
hasilz = [None for _ in range(Nmax)]
```
**Penggunaan perintah looping untuk penyelesaian range nilai uo dan zo**

```
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
    zo[p] = zb[p]
```

**Pembuatan Grafik**
>Menggunakan fungsi def yang biasa digunakan untuk menggabung atau menhimpun perintah perhitungan model

```
def rand_col_hex_string():
  return f'#{format(np.random.randint(0,16777215), "#08x")[2:]}

hasilu_np = np.array(hasilu)
hasilz_np = np.array(hasilz)
```

**Plot hasil perhitungan ke dalam Grafik**

```
fig0, ax0 = plt.subplots(figsize=(12,8))
for i in range (1, 16):
  col0 = rand_col_hex_string()
  line, = ax0.plot(hasilu_np[:,i-1], c=col0, label=f'n={i}')
  ax0.legend()

  ax0.set(xlabel='Waktu', ylabel='Kecepatan Arus', title='''Shafina Amalia Yahya_26050120140169
  Perubahan Kecepatan Arus Dalam Grid Tertentu di sepanjang Waktu''')
  ax0.grid()

fig1, ax1 = plt.subplots(figsize=(12,8))
for i in range(1, 16):
  col1= rand_col_hex_string()
  line, = ax1.plot(hasilz_np[:,i-1], c=col1, label=f'n={i}')
  ax1.legend()

  ax1.set(xlabel='Waktu', ylabel='Elevasi Muka Air', 
          title='''Shafina Amalia Yahya_26050120140169
          Perubahan Elevasi Permukaan Air Dalam Grid Tertentu di sepanjang Waktu''')
  ax1.grid()

fig2, ax2 = plt.subplots(figsize=(12,8))
for i in range(1, 16):
  col2= rand_col_hex_string()
  line, = ax2.plot(hasilu_np[i-1], c=col2, label=f't={i}')
  ax2.legend()

  ax2.set(xlabel='Grid', ylabel='Kecepatan Arus', 
          title='''Shafina Amalia Yahya_26050120140169
          Perubahan Kecepatan Arus Dalam Waktu Tertentu di Sepanjang Grid''')
  ax2.grid()

fig3, ax3 = plt.subplots(figsize=(12,8))
for i in range(1, 16):
  col3= rand_col_hex_string()
  line, = ax3.plot(hasilz_np[i-1], c=col3, label=f't={i}')
  ax3.legend()

  ax3.set(xlabel='Grid', ylabel='Elevasi Muka Air', 
          title='''Shafina Amalia Yahya_26050120140169
          Perubahan Elevasi Muka Air Dalam Waktu Tertentu di Sepanjang Grid''')
  ax3.grid()
  ```

**Perintah untuk menampilkan hasil grafik pemodelan Hidrodinamika 1D**

`plt.show()`

**Hasil Grafik Pemodelan Hidrodinamika 1D**

![image](https://user-images.githubusercontent.com/105702150/169676708-1242e839-ffc9-4dd4-bee2-799a45667a36.png)
![image](https://user-images.githubusercontent.com/105702150/169676710-c5abae7c-c7cb-468d-93bb-0c58d45afa13.png)
![image](https://user-images.githubusercontent.com/105702150/169676713-9903b03e-f04e-4f3d-80de-8ef7801307e4.png)
![image](https://user-images.githubusercontent.com/105702150/169676721-38a5c1fa-ac82-48b0-a171-1c0c04342958.png)


* **MODUL 4 : Persamaan Hidrodinamika 2D**

Pada modul ini

## **PENUTUP**

## **UCAPAN TERIMA KASIH**
