# Tugas Akhir Praktikum Pemodelan Oseanografi Kelompok 4
Repositori ini dibuat untuk memenuhi tugas akhir Praktikum Pemodelan Oseanografi 2022. Repositori ini mengandung seluruh _script_ pemrograman _python_ yang digunakan dalam Praktikum Pemodelan Oseanografi. Library yang digunakan pada Praktikum Pemodelan Oseanografi 2022 ini adalah _Numpy_ , _Matplotlib_ , _Sys_ dan _NDBC_ , 
## **MEMBER**
1. Fatiha Hening Prastiwi 26050120130058 A 👩‍💻
2. Laila Atika Putri 26050120130118 A 👩‍💻
3. Rizqi Abdul Aziz 26050120130072 A 🧑‍💻
4. Shafina Amalia Yahya 26050120140169 A 👩‍💻
5. Tsulasa Zuhrotun Nisak 26050120130093 A 👩‍💻
6. Umar Haidar Al Faruq 26050120140069 B
7. Yustinus Adyaksa Indrayana

## **MODUL SERTA METODE PENGERJAAN**
* **MODUL 1 : Persamaan Adveksi Difusi 1D 🌊** 

Pada modul pertama praktikum pemodelan oseanografi mempelajari mengenai persamaan Adveksi Difusi 1 Dimensi. Adveksi adalah mekanisme perpindahan massa suatu materi dari titik ke titik lainnya. Adveksi juga aliran yang berkaitan dengan fluida dan termasuk dalam persamaan hiperbolik yang menggunakan mekanisme transportasi suatu gas atau zat cair dengan arah tertentu. Contoh penerapan di  bidang Oseanografi yaitu persamaan gelombang linear orde satu. Terdapat dua persamaan yaitu eksplisit dan implisit. Persamaan eksplisit terdapat stabilitas hitungan dan hitungannya lebih mudah tetapi membutuhkan proses coding lebih lama. Sedangkan pada persamaan implisit tidak ada stabilitas hitungan, dan hitungannya rumit, tetapi proses codingnya cepat. Penurunan persamaan ini bisa menggunakan metode FTCS yang merupakan gabungan dari selisih maju maju terhadap waktu dan selisih pusat terhadap ruang dengan solusi stabil bersyarat. Selain metode FTCS, dapat juga menggunakan metode Leapfrog (beda hingga) yaitu perluasan dari metode beda tengah terhadap ruang dan waktu. Skema Leapfrog ini didapatkan dari penurunan Deret Taylor. Penurunan persamaan yang lain dapat menggunakan skema Upstream yang digunakan untuk melengkapi ketidaksempurnaan dari metode Leapfrog karena nilai konsentrasi dalam komputer menjadi negatif walaupun konsentrasinya positif. Metode Upstream dibuat untuk model positif dari konsentrasi di alam yang merujuk ke lautan. Difusi adalah sebuah proses suatu zat bergerak dari konsentrasi tinggi ke rendah. Contoh penerapan di bidang Oseanografi yaitu oil spill. Diskritisasi difusi menggunakan metode eksplisit (FTCS) yang continue maupun discontinue. Jika syarat batas terpenuhi maka sama dengan overflow. 

* **MODUL 2 : Persamaan Adveksi Difusi 2D 🌊** 

Pada modul ini praktikan mempelajari tentang persamaan Adveksi Difusi 2D. Pada umumnya, persamaan Adveksi Difusi 2D digunakan untuk mempelajari penyebaran polutan pada perairan dengan domain yang teratur dan menggunakan koefisien yang konstan, sedangkan persoalan yang berada disekitar kita menyatakan sebaliknya. Walaupun demikian, metode beda hingga tetap menyediakan solusi numerik yang akurat dan dapat dipakai untuk mendapatkan solusi numerik yang diinginkan. Selain itu, persamaan ini juga dapat digunakan untuk mengetahui pola sebaran senyawa pada air, distribusi temperatur, tingkat transfer oksigen, dan masih banyak lagi.

Kemudian dari praktikum yang telah dilakukan, praktikan menggunakan persamaan ini untuk mempelajari pergerakan polutan berdasarkan data yang diberikan oleh asisten. Hasil dari praktikum ini didapat bahwa apabila C=0 dan Ad=1 maka polutan akan menyebar di satu titik tanpa bergerak ke arah manapun. Sedangkan jika C=1 dan Ad=0 maka polutan akan bergerak sesuai dengan arah aliran arus, namun penyebarannya akan sangat lambat atau bahkan tidak terjadi penyebaran. Selain itu diketahui juga bahwa arus memberikan peranan dalam proses adveksi difusi 2 dimensi dengan pergerakan polutan yang sesuai dengan arah arus, sedangkan koefisien adveksi difusi memberikan gambaran proses transport konsentrasi polutan ke segala arah. Kecepatan pergerakan dan penyebaran tergantung pada kecepatan arus.

`import matplotlib.pyplot as plt`

`import numpy as np`

`import sys`

```
def percentage(part, whole):
    percentage = 100 * float(part)/float(whole)
    return str(round(percentage,2)) + "x"
```

**Penginputan Nilai Parameter**
```
Parameter Awal
C = 1.96
ad = 1.96

Arah Arus
theta = 96
#theta = 156
#theta = 231
#theta = 411

Parameter Lanjutan
q = 0.95 
x = 500 
y = 500 
dt = 0.5 
dx = 5 
dy = 5

Lama Simulasi
Tend = 109
#Tend = 1
dt = 0.5

Polutan
px = 250
py = 239
Ic = 1096
```
**Perhitungan Persebaran Polutan**
```
Perhitungan U dan V
u = C * np.sin(theta*np.pi/180)
v = C * np.cos(theta*np.pi/180)
dt_count = 1/((abs(u)/(q*dx))+(abs(v)/(q*dy))+(2*ad/(q*dx**2))+(2*ad/(q*dy*2)))

Nx = int(x/dx)  #number of mesh in x direction
Ny = int(y/dy)  #number of mesh in y direction
Nt = int(Tend/dt)

Perhitungan Titik Polutan Di Buang
px1 = int(px/dx)
py1 = int(py/dy)

Fungsi Disederhanakan
lx = u*dt/dx
ly = v*dt/dy
ax = ad*dt/dx**2
ay = ad*dt/dy**2
cfl = (2*ax + 2*ay + abs(lx) + abs(ly))  #syarat kestabilan CFL

Perhitungan CFL
if cfl >= q:
    print('CFL Violated, please use dt :'+str(round(dt_count,4)))
    sys.exit ()
#%%
```

**Pembuatan Grid Persebaran Polutan**
```
Pembuatan Grid 
x_grid = np.linspace(0-dx, x+dx, Nx+2) #ghostnode boundary
y_grid = np.linspace(0-dx, y+dy, Ny+2) #ghostnode boundary
t = np.linspace(0, Tend, Nt+1)
x_mesh, y_mesh = np.meshgrid(x_grid,y_grid)
F = np.zeros((Nt+1, Ny+2, Nx+2))
```

**Perhitungan Iterasi Hingga Mencapai Syarat Batas**
```
Kondisi Awal
F[0,py1,px1]=Ic

Iterasi
for n in range (0, Nt):
    for i in range (1,Ny+1):
        for j in range (1, Nx+1):
         F[n+1,i,j]=((F[n,i,j]*(1-abs(lx)-abs(ly))) + \
                (0.5*(F[n,i-1,j]*(ly+abs(ly)))) + \
                (0.5*(F[n,i+1,j]*(abs(ly)-ly))) + \
                (0.5*(F[n,i,j-1]*(lx+abs(lx)))) + \
                (0.5*(F[n,i,j+1]*(abs(lx)-lx))) + \
                (ay*(F[n,i+1,j]-2*(F[n,i,j])+F[n,i-1,j])) +\
                (ax*(F[n,i,j+1]-2*(F[n,i,j])+F[n,i,j-1])))
Syarat batas
    F[n+1,0,:] = 0 #bc bawah
    F[n+1,:,0] = 0 #bc kiri
    F[n+1,Ny+1,:] = 0 #bc atas
    F[n+1,:,Nx+1] = 0 #bc kanan
```

**Pembuatan Output Gambar**
```
    plt.clf()
    plt.pcolor(x_mesh, y_mesh, F[n+1, :, :], cmap = 'jet',shading='auto',edgecolor='k')
    cbar=plt.colorbar(orientation='vertical',shrink=0.95,extend='both')
    plt.clf()
    plt.pcolor(x_mesh,y_mesh,F[n+1,:,:],cmap='jet',shading='auto',edgecolor='k')
    cbar = plt.colorbar(orientation='vertical',shrink=0.95,extend='both')
    cbar.set_label(label='Concentration',size = 8)
    #plt.clim(0,100)
    plt.title('Umar Haidar Al Faruq_26050120140069 \n t='+str(round(dt*(n+1),3))+ ', Initial condition='+str(Ic),fontsize=10)
    plt.xlabel('x_grid',fontsize=9)
    plt.ylabel('y_grid',fontsize=9)
    plt.axis([0, x, 0, y])
    #plt.pause(0.01)
    plt.savefig(str(n+1)+'.jpg', dpi=300)
    plt.pause(0.01)
    plt.close()
    print('running timestep ke:' +str(n+1) + ' dari:' +str(Nt) + '('+ percentage(n+1,Nt)+')')
    print('Nilai CFL:' +str(cfl) + 'dengan arah: ' +str(theta))
```

* **MODUL 3 : Persaman Hidrodinamika 1D 🌊**

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
**Penggunaan perintah looping untuk penyelesaian range untuk nilai uo dan zo**

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
>Menggunakan fungsi def yang biasa digunakan untuk menggabung atau menghimpun perintah perhitungan model

```
def rand_col_hex_string():
  return f'#{format(np.random.randint(0,16777215), "#08x")[2:]}

hasilu_np = np.array(hasilu)
hasilz_np = np.array(hasilz)
```

>Plot hasil perhitungan ke dalam Grafik

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

>Grafik tersebut memiliki pola grafik sinusoidal dengan interval waktu dari 0-600 detik dan juga interval grid sebanyak 0-50 menunjukkan perubahan elevasi permukaan air dan kecepatan arus dalam grid dan waktu. Hal ini sesuai dengan teori gelombang amplitudo kecil yang menyatakan bahwa fluktuasi muka air periodik terhadap jarak dan waktu dan komponen kecepatan akan berkurang dengan bertambahnya jarak ke bawah dihitung dari muka air. Elevasi dan kecepatan arus masih berfluktuasi secara tetap dan teratur di waktu awal menunjukkan bahwa pada model ini tidak ada gangguan. Akan tetapi, mulai berantakan di waktu selanjutnya, hal ini karena nilai elevasi dan kecepatan arus yang semakin besar dan terdapat pula gangguan yang berasal dari faktor eksternal yang mana ada beberapa yang tidak sesuai dengan syarat kestabilannya.

* **MODUL 4 : Persamaan Hidrodinamika 2D 🌊**

Pada modul ini praktikan melakukan pemodelan data gelombang NDBC (National Buoy Data Center) menggunakan miniconda 3, kemudian menginstall siphon dan membuka jupyter notebook pada miniconda 3 tersebut. Praktikan juga mengakses website NDBC NOOA untuk mengetahui lebih jelas lokasi stasiun dari gelombang yang akan dianalisis praktikan menggunakan persamaan hidrodinamika 2D. Penyebab utama terjadinya gelombang ialah angin yang bertiup di atas permukaan laut. Terdapat 3 faktor angin yang sangat berpengaruh dalam pembentukan gelombang, yaitu kecepatan angin, lamanya angin bertiup dan jarak rintangan diman angin bertiup atau fetch. Umumnya, makin kencang angin bertiup, maka makin besar gelombang yang terbentuk dan gelombang ini mempunyai kecepatan yang tinggi dengan panjang gelombang yang besar. Ditambah lagi, apabila gelombang yang akan dianalisis termasuk dalam perairan bebas, maka besar kemungkinan memiliki panjang gelombang sampai beberapa ratus meter.  

**Command untuk menggunakan data dari NDBC dalam menjalankan program ini**
```
Copyright (c) 2018 Siphon Contributors. 
Distributed under the terms of the BSD 3-Clause License. 
SPDX-License-Identifier: BSD-3-Clause 
```

```
NDBC Bouy Meteorological Data Request 
=====================================
The NDBC keeps a 45-day recent rolling file for each bouy. This examples shows how to access
the basic meteorogical data from a buoy and make a simple plot.
```
```
import matplotlib.pyplot as plt
from siphon.simplewebservice.ndbc import NDBC
```
**Get a pandas data frame of all of the observations, meteorogical data is the default
observation set to query**
```
df = NDBC.realtime_observations('46029') #Station ID
df.head()
```
**Command Buat Grafik dengan data NDBC**
```
Let's make a simple times series plot to checkout what the data look like.
fig, (ax1, ax2, ax3) = plt.subplots(3, 1, figsize=(12, 10))
ax2b = ax2.twinx() 
```

**Perhitungan Tekanan**
```
ax1.plot(df['time'], df['pressure'], color='black')
ax1.set_ylabel('Pressure [hPa]')
fig.suptitle('Fatiha Hening P_26050120130058_A', fontsize=18)
```

**Perhitungan Kecepatan Angin serta arahnya**
```
ax2.plot(df['time'], df['wind_speed'], color='tab:orange')
ax2.plot(df['time'], df['wind_gust'], color='tab:olive', linestyle='--')
ax2b.plot(df['time'], df['wind_direction'], color='tab:blue', linestyle='-')
ax2.set_ylabel('Wind Speed [m/s]')
ax2b.set_ylabel('Wind Direction')
```

**Perhitungan Temperatur air**
```
ax3.plot(df['time'], df['water_temperature'], color='tab:brown')
ax3.set_ylabel('Water Temperature [degC]')
```

**Perintah untuk menampilkan hasil grafik modul 2D**

`plt.show()`

**Hasil Grafik Pemodelan Hidrodinamika 2D**

## **PENUTUP 🙌**

## **UCAPAN TERIMA KASIH 🙇‍♂️ 🙇‍♀️**
Demikianlah _repository_ tugas akhir Praktikum Pemodelan Oseanografi 2022. Para _author_ memohon maaf jika ada kesalahan dalam penulisan maupun penjelasan materi pada _repository_ ini. Kami juga ingin berterima kasih kepada :
1. Dr. Aris Ismanto, S.Si., M.Si. selaku dosen pengampu matakuliah Pemodelan Oseanografi.
2. Prof. Dr. Denny Nugroho Sugianto, S.T., M.Si. selaku dosen pengampu matakuliah Pemodelan Oseanografi.
3. Dr. Elis Indrayanti, S.T., M.Si. selaku dosen pengampu matakuliah Pemodelan Oseanografi.
4. Rikha Widhiaratih, S.Si., M.Si selaku dosen pengampu matakuliah Pemodelan Oseanografi.
5. Kakak - kakak asisten Praktikum Pemodelan Oseanografi 2022 yang sudah mendampingi dan juga mengajarkan selama keberjalanan praktikum.
6. Teman - teman Oseanografi 2020 yang membantu atas tersusunnya _repository_ ini.
