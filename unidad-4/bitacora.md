# Unidad 4

## Bitácora de proceso de aprendizaje
### Actividad 5

estuve explorando cómo salir de la rigidez de las coordenadas cartesianas para trabajar con ángulos de una forma más natural usando coordenadas polares. Al principio, la idea de pasar de un sistema de cuadrícula (x y y) a uno basado en la distancia desde el centro (r) y la dirección (θ) parece compleja, pero al ver el código todo cobra sentido. En la simulación inicial, entendí que la relación matemática fundamental para que la computadora dibuje en la pantalla es que x es el radio multiplicado por el coseno del ángulo y y es el radio por el seno del ángulo.  Al aplicar esto manualmente en el draw(), logré que el círculo orbitara perfectamente, pero luego quise experimentar con las herramientas nativas de p5.js. Al intentar simplificar el proceso usando p5.Vector.fromAngle(theta), me llevé una sorpresa: el círculo parecía haber desaparecido o quedarse estático en el centro. Lo que ocurrió es que este método, por defecto, crea un vector unitario, lo que significa que el radio es apenas de 1 píxel, haciendo el movimiento casi imperceptible a simple vista. Además, al borrar las variables manuales de x y y, la función line() dejó de funcionar porque ya no encontraba esas referencias. La verdadera revelación llegó con la segunda modificación, donde utilicé p5.Vector.fromAngle(theta, r). Aquí comprendí que p5.js permite pasarle la magnitud como segundo argumento, lo que hace que el vector calcule internamente toda la trigonometría por nosotros. Al usar las propiedades v.x y v.y del objeto vector, el código se volvió mucho más limpio y profesional. Aprendí que las coordenadas polares son la clave para cualquier movimiento circular o espiral, y que usar vectores no solo ahorra líneas de código, sino que hace que la lógica sea mucho más fácil de leer y escalar para proyectos más complejos

## Bitácora de aplicación 



## Bitácora de reflexión
