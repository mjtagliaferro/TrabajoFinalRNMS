## Regularización

- ¿Qué efecto tuvo BatchNorm en la estabilidad y velocidad del entrenamiento?
  
Al agregar BatchNorm, la velocidad del entrenamiento disminuyó de 3 minutos a 2.9 minutos. En cuanto a la estabilidad del entrenamiento, se puede ver que la curva de pérdida (loss) desciende de forma más suave hacia el mínimo. La curva de accuracy también se ve más regular, con menos picos y caídas repentinas. De esta manera, ambas métricas muestran menor variabilidad. Cabe aclarar que la sesión luminous-croc-1000 se corresponde con la aplicación de BatchNorm.
<img width="933" height="370" alt="Captura de pantalla 2025-11-25 a la(s) 3 18 38 p  m" src="https://github.com/user-attachments/assets/23309abe-4734-4a41-bedc-bf17cdc86c57" />

- ¿Cambió la performance de validación al combinar BatchNorm con Dropout?

Implementando solo BatchNorm, durante la validación, el accuracy resultó igual al 53,04%, mientras que la pérdida (loss) se igualó a 1,23. Al agregar Dropout, el accuracy de validación ascendió a 55,8% y la pérdida (loss) disminuyó a 1,09. En los gráficos, al comparar, puede observarse una mayor estabilidad tanto en el accuracy como en la loss a lo largo de las diferentes épocas. Las curvas presentan menos picos bruscos. Cabe destacar que agreeable-ape-655 es la sesión que combina BatchNorm y Dropout.
<img width="694" height="611" alt="Captura de pantalla 2025-11-25 a la(s) 3 27 06 p  m" src="https://github.com/user-attachments/assets/7c5494e6-4ec9-4844-8888-621f743c0a48" />


- ¿Qué combinación de regularizadores dio mejores resultados en tus pruebas?
  
Utilizando BatchNorm y Dropout se obuvo un accuracy en validación de 55,80%, mientras que el de entrenamiento resultó igual a 55,95%. Esta mínima diferencia indica que el modelo generaliza de manera consistente: no hay problema de overfitting. Asimismo, la loss en validación fue igual a 1,09.
Al modificar el optimizador con Weight Decay (L2), el accuracy en validación aumentó a 56,35% y en entrenamiento, a 56,81%; manteniendose una baja diferencia entre los valores. La loss en validación resultó igual a 1,10, mostrando que la regularización adicional ayuda a controlar los pesos sin afectar negativamente la estabilidad del entrenamiento. De esta manera, BatchNorm con Dropout y Weight Decay se posicionó como la mejor alternativa.

- ¿Notaste cambios en la loss de entrenamiento al usar BatchNorm?
  
Si, al usar BatchNorm la curva correspondiente a la loss de entrenamiento en función de las épocas se suavizó, eliminando los ascensos y descensos bruscos. Además, se redujo la variabilidad de la loss entre batches consecutivos. 

## Inicialización de Parámetros

- ¿Qué diferencias notaste en la convergencia del modelo según la inicialización?
  
Para analizar la convergencia del modelo, se observó la pérdida (loss) durante el entrenamiento y validación. Para la inicialización Xavier (celeste), la curva de la loss en el entrenamiento muestra una convergencia muy lenta (por una pendiente pequeña), aunque alcanza la mínima pérdida entre todas las técnicas aplicadas. 
La inicialización He (rosa) presentó la convergencia más rápida, siendo que la pérdida disminuye de forma veloz pero estable. Esto resulta coherente porque He es la inicialización más utilizada para redes con activacioenes ReLU. 
La inicialización Uniforme (amarillo), por su parte, tuvo un desempeño similar al de He en el entrenamiento aunque la loss incrementó durante los últimos batches. Su curva durante la validación tuvo alta variabilidad y aumentó conforme incrementaron la cantidad de epochs a partir del batch número 4. De esta manera, no se esperaría una convergencia adecuada para esta inicialización. 

<img width="887" height="480" alt="Captura de pantalla 2025-11-24 a la(s) 7 42 05 p  m" src="https://github.com/user-attachments/assets/f86c8691-e8ea-4dfc-b207-b36888a12938" />
<img width="881" height="475" alt="Captura de pantalla 2025-11-24 a la(s) 7 42 09 p  m" src="https://github.com/user-attachments/assets/a23428a6-b9b5-4f30-a764-a0b5dbd70f7d" />

- ¿Alguna inicialización provocó inestabilidad (pérdida muy alta o NaNs)?
  
No, ninguna de las inicializaciones provocó inestabilidad. La mayor pérdida estuvo dada por la inicialización Uniforme durante la validación, alcanzando una loss de 1,4. 

- ¿Qué impacto tiene la inicialización sobre las métricas de validación?
  
En la validación, las inicializaciones Xavier y He obtuvieron un accuracy del 57,45%. La inicialización Uniforme resultó en un accuracy igual al 51,93%, por lo que fue notablemente menor. Ahora bien, en la pérdida (loss), la inicialización Xavier obtuvo el menor valor (1,15), seguida por He (1,31) y por último, Uniforme (1,41).

- ¿Por qué `bias` se suele inicializar en cero?
  
Los bias se inicializan en cero porque no contribuyen a la amplificación o atenuación del gradiente porque no se multiplican como los pesos. Además, establecerlos en cero evita introducir un desplazamiento arbitrario en las activaciones antes de que el modelo empiece a aprender. 




