# Código preguntados


#### **1. Diseño de la interfaz de usuario**
Primero, en la interfaz de usuario, deberás agregar los siguientes componentes:

- **Label** para mostrar la pregunta (`LabelPregunta`).
- **Buttons** para las opciones de respuesta (4 botones como `ButtonRespuesta1`, `ButtonRespuesta2`, `ButtonRespuesta3`, `ButtonRespuesta4`).
- **Label** para el puntaje (`LabelPuntaje`).
- **Label** para el tiempo restante (`LabelTiempo`).
- **TextBox** para que el usuario ingrese su nombre (`TextBoxNombre`).
- **ListPicker** para seleccionar la categoría de preguntas (`ListPickerCategoria`).
- **Timer** para controlar el tiempo (`Timer1`).
- **Label** para mostrar mensajes como "Fin del juego" o "Respuesta correcta" (`LabelMensaje`).

#### **2. Definir las variables**

Definiremos algunas variables clave para gestionar el juego:

- **`puntaje`**: Para llevar el puntaje del jugador.
- **`tiempo_restante`**: Para llevar el control del tiempo.
- **`preguntas`**: Una lista de preguntas.
- **`respuestas_correctas`**: Una lista de respuestas correctas.
- **`opciones`**: Una lista de opciones de respuesta para cada pregunta.
- **`categoria_seleccionada`**: La categoría que el jugador selecciona.
- **`usuario_nombre`**: El nombre del jugador.

#### **3. Bloques de código**

### **a. Inicialización del juego**
Cuando la pantalla inicializa, configuramos las variables y la interfaz de usuario.

```blocks
when Screen1.Initialize do
    // Configuración inicial
    set puntaje to 0
    set tiempo_restante to 10  // Tiempo de respuesta en segundos
    set LabelPuntaje.Text to "Puntaje: " + puntaje
    set LabelTiempo.Text to "Tiempo: " + tiempo_restante
    set LabelMensaje.Text to ""
    
    // Definir categorías disponibles
    set ListPickerCategoria.Elements to ["General", "Historia", "Ciencia", "Deportes"]
    
    // Definir preguntas iniciales
    set preguntas to [
        "¿Cuál es la capital de Francia?", 
        "¿En qué año llegó Cristóbal Colón a América?", 
        "¿Cuántos planetas hay en el sistema solar?", 
        "¿Quién ganó el Mundial 2018?"
    ]
    set respuestas_correctas to [
        "París", 
        "1492", 
        "8", 
        "Francia"
    ]
    set opciones to [
        ["París", "Madrid", "Roma", "Londres"], 
        ["1492", "1500", "1488", "1475"], 
        ["8", "9", "10", "7"], 
        ["Francia", "Brasil", "Alemania", "España"]
    ]
end
```

### **b. Selección de categoría**
Cuando el jugador selecciona una categoría desde el `ListPicker`, se actualizan las preguntas y las respuestas.

```blocks
when ListPickerCategoria.AfterPicking do
    set categoria_seleccionada to ListPickerCategoria.Selection
    
    // Cambiar las preguntas dependiendo de la categoría seleccionada
    if categoria_seleccionada == "Historia" then
        set preguntas to ["¿En qué año se firmó la Declaración de Independencia de EE.UU?", "¿Quién fue el primer emperador de Roma?"]
        set respuestas_correctas to ["1776", "Augusto"]
        set opciones to [
            ["1776", "1789", "1799", "1800"], 
            ["Augusto", "Julio César", "Nerón", "Tiberio"]
        ]
    else if categoria_seleccionada == "Ciencia" then
        set preguntas to ["¿Quién desarrolló la teoría de la relatividad?", "¿Cuál es el elemento químico con símbolo O?"]
        set respuestas_correctas to ["Einstein", "Oxígeno"]
        set opciones to [
            ["Einstein", "Newton", "Galileo", "Darwin"], 
            ["Oxígeno", "Hidrógeno", "Carbono", "Nitrógeno"]
        ]
    else if categoria_seleccionada == "Deportes" then
        set preguntas to ["¿Quién ganó el Mundial 2018?", "¿Cuántos jugadores tiene un equipo de fútbol?"]
        set respuestas_correctas to ["Francia", "11"]
        set opciones to [
            ["Francia", "Brasil", "Alemania", "Argentina"], 
            ["10", "11", "12", "13"]
        ]
    end
    
    // Mostrar la primera pregunta
    set LabelPregunta.Text to select list item list preguntas index 1
    set tiempo_restante to 10
    set LabelTiempo.Text to "Tiempo: " + tiempo_restante
    Timer1.Enabled to true
end
```

### **c. Control del temporizador**
El temporizador cuenta hacia atrás y termina el tiempo cuando llega a 0.

```blocks
when Timer1.Timer do
    if tiempo_restante > 0 then
        set tiempo_restante to tiempo_restante - 1
        set LabelTiempo.Text to "Tiempo: " + tiempo_restante
    else
        // Si el tiempo se acaba, mostrar respuesta correcta y avanzar a la siguiente
        set LabelMensaje.Text to "¡Tiempo agotado! La respuesta correcta era: " + select list item list respuestas_correctas index 1
        Timer1.Enabled to false
        // Esperar 2 segundos y pasar a la siguiente pregunta
        Timer2.Enabled to true
    end
end
```

### **d. Respuesta seleccionada**
Cuando el jugador selecciona una respuesta, verificamos si es correcta y actualizamos el puntaje.

```blocks
when ButtonRespuesta1.Click do
    verificar_respuesta(ButtonRespuesta1.Text)
end

when ButtonRespuesta2.Click do
    verificar_respuesta(ButtonRespuesta2.Text)
end

when ButtonRespuesta3.Click do
    verificar_respuesta(ButtonRespuesta3.Text)
end

when ButtonRespuesta4.Click do
    verificar_respuesta(ButtonRespuesta4.Text)
end

to verificar_respuesta(respuesta_seleccionada)
    if respuesta_seleccionada == select list item list respuestas_correctas index 1 then
        set puntaje to puntaje + 1
        set LabelMensaje.Text to "¡Correcto!"
    else
        set LabelMensaje.Text to "Incorrecto! La respuesta correcta era: " + select list item list respuestas_correctas index 1
    end
    set LabelPuntaje.Text to "Puntaje: " + puntaje
    Timer1.Enabled to false
    Timer2.Enabled to true
end
```

### **e. Pasar a la siguiente pregunta**
Una vez que se haya mostrado la respuesta, se cambia a la siguiente pregunta.

```blocks
when Timer2.Timer do
    set Timer2.Enabled to false
    // Cambiar la pregunta
    if (index de la pregunta actual < longitud de preguntas) then
        set LabelPregunta.Text to select list item list preguntas index siguiente
        set tiempo_restante to 10
        set LabelTiempo.Text to "Tiempo: " + tiempo_restante
        Timer1.Enabled to true
    else
        // Fin del juego, mostrar puntaje final
        set LabelPregunta.Text to "¡Fin del juego!"
        set LabelMensaje.Text to "Puntaje final: " + puntaje
    end
end
```
