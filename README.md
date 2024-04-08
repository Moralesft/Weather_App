

root = Tk()  # Crea una instancia de la clase Tk
root.title("weather app")  # Establece el título de la ventana
root.geometry("900x500+300+200")  # Establece el tamaño y la posición de la ventana
root.resizable(False, False)  # Hace que la ventana no sea redimensionable

def getWeather():  # Define una función llamada getWeather
    try:  # Inicia un bloque de código donde se manejarán las excepciones
        city = textfield.get()  # Obtiene el texto ingresado en el campo de texto

        geolocator = Nominatim(user_agent="mi_aplicacion", timeout=20)  # Crea una instancia de Nominatim con un tiempo de espera de 20 segundos
        location = geolocator.geocode(city)  # Obtiene las coordenadas geográficas de la ciudad ingresada
        obj = TimezoneFinder()  # Crea una instancia de TimezoneFinder
        result = obj.timezone_at(lng=location.longitude, lat=location.latitude)  # Obtiene la zona horaria de las coordenadas geográficas
        home = pytz.timezone(result)  # Obtiene el objeto de zona horaria
        local_time = datetime.now(home)  # Obtiene la hora local
        current_time = local_time.strftime("%I:%M:%p")  # Formatea la hora local
        clock.config(text=current_time)  # Configura el texto del reloj con la hora local
        name.config(text="CURRENT WEATHER")  # Configura el texto del nombre como "CURRENT WEATHER"

        # Realiza una solicitud a la API del clima
        api = "https://api.openweathermap.org/data/2.5/weather?q=" + city + "&appid=ee7a187962a1dfd80f2dbd03b15a819e"
        json_data = requests.get(api).json()  # Obtiene los datos en formato JSON
        # Extrae los datos del clima
        condition = json_data['weather'][0]['main']
        description = json_data['weather'][0]['description']
        temp = int(json_data['main']['temp'] - 273.15)
        pressure = json_data['main']['pressure']
        humidity = json_data['main']['humidity']
        wind = json_data['wind']['speed']

        # Actualiza las etiquetas de texto con los datos del clima
        t.config(text=(temp, "°"))
        c.config(text=(condition, "|", "FEELS", "LIKE", temp, "°"))
        w.config(text=wind)
        h.config(text=humidity)
        d.config(text=description)
        p.config(text=pressure)
    except Exception as e:  # Captura cualquier excepción y muestra un mensaje de error
        messagebox.showerror("WeatherApp", "invalid entry!!")

# Define la imagen de búsqueda
Search_image = PhotoImage(file="search.png")
myimage = Label(image=Search_image)  # Crea una etiqueta con la imagen de búsqueda
myimage.place(x=20, y=20)  # Coloca la etiqueta en la ventana

textfield = tk.Entry(root, justify="center", width=17, font=("poppins", 25, "bold"), bg="#404040", border=0, fg="white")  # Crea un campo de texto
textfield.place(x=50, y=40)  # Coloca el campo de texto en la ventana
textfield.focus()  # Hace que el campo de texto tenga el foco

# Define el icono de búsqueda
Search_icon = PhotoImage(file="search_icon.png")
myimage_icon = Button(image=Search_icon, borderwidth=0, cursor="hand2", bg="#404040", command=getWeather)  # Crea un botón con el icono de búsqueda
myimage_icon.place(x=400, y=34)  # Coloca el botón en la ventana

# Define el logo
logo_image = PhotoImage(file="logo.png")
logo = Label(image=logo_image)  # Crea una etiqueta con el logo
logo.place(x=150, y=100)  # Coloca el logo en la ventana

# Define el marco de los botones
Frame_image = PhotoImage(file="box.png")
frame_myimage = Label(image=Frame_image)  # Crea una etiqueta con la imagen del marco
frame_myimage.pack(padx=5, pady=5, side=BOTTOM)  # Empaqueta la etiqueta en la ventana

# Define las etiquetas de tiempo y clima
name = Label(root, font=("arial", 15, "bold"))  # Crea una etiqueta para el nombre
name.place(x=30, y=100)  # Coloca
