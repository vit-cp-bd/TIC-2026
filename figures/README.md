# Figures

Las figuras utilizadas en el manuscrito se encuentran en este directorio. Aquí se incluyen los archivos fuente en formato PNG de cada figura, para cada técnica de ajuste aplicada al modelo de clasificación de imágenes (ViT-Base/16). Las figuras están organizadas de la siguiente manera:

`accuracy_curves_*.png`: curvas de precisión para cada técnica de ajuste durante las fases de entrenamiento, validación y prueba.
`f1_curves_*.png`: curvas de F1-score para cada técnica de ajuste durante las fases de entrenamiento, validación y prueba.
`loss_curves_*.png`: curvas de pérdida durante el entrenamiento, la validación y la prueba.
`confusion_matrix_*.png`: matrices de confusión para cada técnica de ajuste durante la fase de prueba.
`test_*.png`: resultados de prueba para la técnica de transferencia de aprendizaje (TL) y el ajuste fino parcial (FTP).
`comparative_test_tl_ftp.png`: comparación de las técnicas de ajuste fino (TL y FTP) en términos de precisión, exactitud, pérdida, F1-score y sensibilidad durante la fase de prueba.
`eigen-*`: gráficos relacionados con la técnica de Eigen-CAM para cada técnica de ajuste, por variedad de tubérculo y por clase.