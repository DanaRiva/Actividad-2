# Actividad 2

Dana Marian Rivera Oropeza - A00830027 
Daniela Berenice Hernández de Vicente - A01735346 
Alejandro Armenta Arellano - A01734879

## Robot 1 Péndulo 1 GDL

Después de limpiar el entorno, creamos las variables simbólicas para realizar el cálculo de las masas y matrices de inercia en este caso por ser un robot tipo péndulo se requieren los ángulos, de igual manera tenemos que l1=longitud de eslabones y lc=distancia al centro de masa de cada eslabón

``` matlab
syms th1(t)  t  %Angulos de cada articulación
syms m1 m2 m3 Ixx1 Iyy1 Izz1  %Masas y matrices de Inercia
syms t1  %Tiempos
syms l1 lc1 %l=longitud de eslabones y lc=distancia al centro de masa de cada eslabón
syms pi g
```
