# DANIEL ROMERO BRAVO
## Practica Flask-movil-openweathermap
```text
Asistencia de IA: El usuario fue apoyado con la asistencia para mejoras en el diseño 
del dashboard, correcciones de errores en el código.
Herramienta: ChatGPT (GPT-5)
Fecha: 18 de septiembre de 2025
Plataforma de hardware utilizada: Teléfono Android (Termux) y PC con acceso vía Tailscale
```
En esta práctica se desarrolló un servidor web en Python utilizando Flask, el cual consulta datos en tiempo real de la API de OpenWeatherMap y los presenta en un dashboard web atractivo con estilos personalizados.
El proyecto se ejecuta en un teléfono Android mediante Termux, y gracias a Tailscale VPN es posible acceder al dashboard desde cualquier otro dispositivo conectado a la misma red privada.

## Codigo utilizado.

```python
# ╔═══════════════════════════════════════════════════════════════╗
# ║        🌤️ Mini Dashboard del Clima con Flask + Tailscale       ║
# ║        Lenguajes de Interfaz - TECNM / ITT - 2025              ║
# ║        Autor: Daniel Romero Bravo                              ║
# ║        Descripción: Servidor Flask consultando OpenWeatherMap  ║
# ╚═══════════════════════════════════════════════════════════════╝

from flask import Flask
import requests
import datetime

app = Flask(__name__)

API_KEY = "aaa89a4cc3a5b42030d9813aead955b7"  # Tu API Key
CITY = "Tijuana"  # Cambia la ciudad si quieres

@app.route("/")
def clima():
    # Consumir API
    url = f"http://api.openweathermap.org/data/2.5/weather?q={CITY}&appid={API_KEY}&units=metric&lang=es"
    data = requests.get(url).json()

    # Extraer datos
    temp = data["main"]["temp"]
    feels = data["main"]["feels_like"]
    humidity = data["main"]["humidity"]
    weather = data["weather"][0]["description"].capitalize()
    icon = data["weather"][0]["icon"]  # ícono oficial de OpenWeatherMap
    now = datetime.datetime.now().strftime("%d/%m/%Y %H:%M")

    # Seleccionar color de fondo según clima
    if "lluvia" in weather.lower():
        bg_color = "#87CEFA"  # azul lluvia
    elif "nube" in weather.lower():
        bg_color = "#B0C4DE"  # gris azulado
    elif temp < 10:
        bg_color = "#ADD8E6"  # azul frío
    elif temp < 25:
        bg_color = "#90EE90"  # verde templado
    else:
        bg_color = "#FF7F7F"  # rojo calor

    # HTML con estilo mejorado
    return f"""
    <html>
    <head>
        <title>Clima en {CITY}</title>
        <style>
            body {{
                background: {bg_color};
                font-family: Arial, sans-serif;
                display: flex;
                justify-content: center;
                align-items: center;
                height: 100vh;
                margin: 0;
            }}
            .card {{
                background: white;
                border-radius: 20px;
                box-shadow: 0px 4px 12px rgba(0,0,0,0.2);
                padding: 30px;
                text-align: center;
                max-width: 400px;
                width: 90%;
                animation: fadeIn 1.2s ease-in-out;
            }}
            h1 {{
                margin-bottom: 10px;
                color: #333;
            }}
            .icon {{
                width: 100px;
                height: 100px;
            }}
            p {{
                font-size: 18px;
                margin: 8px 0;
                color: #555;
            }}
            .footer {{
                margin-top: 15px;
                font-size: 14px;
                color: #777;
            }}
            @keyframes fadeIn {{
                from {{opacity: 0; transform: translateY(20px);}}
                to {{opacity: 1; transform: translateY(0);}}
            }}
        </style>
    </head>
    <body>
        <div class="card">
            <h1>🌤️ Clima en {CITY}</h1>
            <img class="icon" src="http://openweathermap.org/img/wn/{icon}@2x.png" alt="icono clima">
            <p><b>🌡️ Temperatura:</b> {temp} °C</p>
            <p><b>🤔 Sensación térmica:</b> {feels} °C</p>
            <p><b>💧 Humedad:</b> {humidity}%</p>
            <p><b>☁️ Condiciones:</b> {weather}</p>
            <div class="footer">Actualizado: {now}</div>
        </div>
    </body>
    </html>
    """

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```
Adjunto link del video en Loop: https://www.loom.com/share/627404a6136142b7bca2b67109dc4fdb?sid=9f4aa69d-aa5c-4169-81cc-9c208a9e9bae

Adjunto Link del video en Asciinema: https://asciinema.org/a/QwYITZrUQmXuPCXPBJ8ByYx23

