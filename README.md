# Actividad2

Dana Marian Rivera Oropeza - A00830027 Daniela Berenice Hernández de Vicente - A01735346 Alejandro Armenta Arellano - A01734879

## Robot 1 Péndulo 1 GDL

Después de limpiar el entorno, creamos las variables simbólicas para realizar el cálculo de las masas y matrices de inercia en este caso por ser un robot tipo péndulo se requieren los ángulos, de igual manera tenemos que l1=longitud de eslabones y lc=distancia al centro de masa de cada eslabón, de igual manera declaramos los tiempos.

``` matlab
syms th1(t)  t  
syms m1 m2 m3 Ixx1 Iyy1 Izz1  
syms t1  
syms l1 lc1 
syms pi g
``` 

Se crea el vector de coordenadas articulares.
``` matlab
  Q= [th1];
``` 
 
 Se derivan los ángulos con respecto al tiempo para obtener  vector de velocidades y aceleraciones articulares  
 ``` matlab
  Qp= diff(Q, t);
  Qpp= diff(Qp, t);
``` 

Se genera la configuración del robot con 0 porque es un movimiento rotacional 
 ``` matlab
RP=[0];
 ```
 
Se establece el número de grado de libertad del robot siendo en este caso 1.
 ``` matlab
GDL= size(RP,2);
GDL_str= num2str(GDL);
 ``` 
 Se realizan los vectores con las funciones para modelar las posiciones y la rotación de cada articualación y por el tipo de movimiento son las mismas para las tres articulaciones, la diferencia es la indexación, que corresponde a la articulación que toca 
 
En caso 
 ``` matlab
P(:,:,1)= [l1*cos(th1);
           l1*sin(th1);
                    0];
R(:,:,1)= [cos(th1) -sin(th1)  0; 
           sin(th1)  cos(th1)  0;
           0         0         1];
 ``` 
 
Posteriormente se crea un vector inicializado en ceros, nuestras matrices globales y locales.
 ``` matlab
Vector_Zeros= zeros(1, 3);

%Inicializamos las matrices de transformación Homogénea locales
A(:,:,GDL)=simplify([R(:,:,GDL) P(:,:,GDL); Vector_Zeros 1]);
%Inicializamos las matrices de transformación Homogénea globales
T(:,:,GDL)=simplify([R(:,:,GDL) P(:,:,GDL); Vector_Zeros 1]);
%Inicializamos las posiciones vistas desde el marco de referencia inercial
PO(:,:,GDL)= P(:,:,GDL); 
%Inicializamos las matrices de rotación vistas desde el marco de referencia inercial
RO(:,:,GDL)= R(:,:,GDL); 
 ``` 
 
 En el ciclo for ue se muestra a continuación se ingresan las matrices de cada una de las articulaciones de manera
global y local.
  ``` matlab
 for i = 1:GDL
    i_str= num2str(i);
   %disp(strcat('Matriz de Transformación local A', i_str));
    A(:,:,i)=simplify([R(:,:,i) P(:,:,i); Vector_Zeros 1]);
   %pretty (A(:,:,i));

   %Globales
    try
       T(:,:,i)= T(:,:,i-1)*A(:,:,i);
    catch
       T(:,:,i)= A(:,:,i);
    end
%     disp(strcat('Matriz de Transformación global T', i_str));
    T(:,:,i)= simplify(T(:,:,i));
%     pretty(T(:,:,i))

    RO(:,:,i)= T(1:3,1:3,i);
    PO(:,:,i)= T(1:3,4,i);
    %pretty(RO(:,:,i));
    %pretty(PO(:,:,i));
   
end
  ``` 
 Se obtienen submatrices de Jacobianos, siendo Jv_a el Jacobiano lineal obtenido de forma analítica y el Jw_a el Jacobiano ángular obtenido de forma analítica.
  ```matlab 
Jv_a= simplify (Jv_a);
Jw_a= simplify (Jw_a);
  ```
  
Posteriormente pasamos a obtener la matriz del Jacobiano completa, siendo una mezcla del ángular y lineal.
  ```matlab
Jac= [Jv_a;
      Jw_a];
Jacobiano= simplify(Jac);
  ```
Se obtienen los vectores de Velocidades lineales y angulares (V), al igual que la velocidad ángular obtenida mediante el Jacobiano ángular (W).
  ```matlab
V=simplify (Jv_a*Qp);

W=simplify (Jw_a*Qp);
  ```
Para la energía cinética se realiza otra función donde se basa en las velocidades lineales y angulares que se obtuvieron en la primera parte del código, para crear las matrices de inercia en las que se analiza la energía cinética en cada uno de los ejes según sea el caso del robot, finalmente se realizan los cálculos finales para presentar la energía en cada uno de los eslabones.

``` matlab
V1_Total= V1+cross(W1,P01);
K1= (1/2*m1*(V1_Total))'*(1/2*m1*(V1_Total)) + (1/2*W1)'*(I1*W1);
disp('Energía Cinética en el Eslabón 1');
K1= simplify (K1);
pretty (K1);
```

## Resultados:
### Robot 1
![image](https://user-images.githubusercontent.com/100874942/223322495-1f97e5ea-5d22-45f6-95ae-889187cc593b.png)

### Robot 2
![image](https://user-images.githubusercontent.com/100874942/223322518-69ecef16-b003-428c-89a4-a35b54bdd72a.png)

### Robot 3
![image](https://user-images.githubusercontent.com/100874942/223322565-8e2d0441-2026-4982-a99b-4444b0054047.png)
![image](https://user-images.githubusercontent.com/100874942/223322597-49d5f6bf-e23a-4d32-9a45-93afa6d15eac.png)


