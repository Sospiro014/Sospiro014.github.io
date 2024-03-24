---
layout: project_layout
title: Basic Weather App
date: 2024-03-17
---


This program utilizes the WeatherAPI and OpenWeatherMap APIs to fetch weather information for a specified location provided by the user. After obtaining the location input from the user, weather data is fetched from both APIs. Upon successful retrieval of data, the program presents the user with information including location, country, local time, weather conditions, temperature, humidity, and wind speed. The program facilitates quick and easy access to weather information for a given location.


```python
import requests
import pyowm


# https://www.weatherapi.com/signup.aspx

API_KEY = "API_KEY"

#=================================================

# https://home.openweathermap.org/users/sign_up

OWM_API_KEY = "API_KEY"

owm = pyowm.OWM(OWM_API_KEY)

print('''
█░█░█ █▀▀ █░░ █▀▀ █▀█ █▀▄▀█ █▀▀
▀▄▀▄▀ ██▄ █▄▄ █▄▄ █▄█ █░▀░█ ██▄

█░█░█ ▄▀█ ▀█▀ █░█ █▀▀ █▀█   ▄▀█ █▀█ █▀█
▀▄▀▄▀ █▀█ ░█░ █▀█ ██▄ █▀▄   █▀█ █▀▀ █▀▀
''')

print('Enter your Location:')
location_name = input('Location: ').strip()

try:
    observation = owm.weather_manager().weather_at_place(location_name)
    temperature = observation.weather.temperature("celsius")["temp"]
    humidity = observation.weather.humidity
    wind_speed = observation.weather.wind()["speed"]

    response = requests.get(f'https://api.weatherapi.com/v1/current.json?key={API_KEY}&q={location_name}&aqi=no')

    if response.status_code == 200:
        data = response.json()
        location = data["location"]["name"]
        country = data["location"]["country"]
        localtime = data["location"]["localtime"]
        condition = data["current"]["condition"]["text"]

        print('====================================')
        print(f'Location: {location}' + ' && '+f'{location_name}' )
        print(f'Country: {country}')
        print(f'Local Time: {localtime}')
        print(f'Weather: {condition}')
        print(f'Temperature: {temperature}°C')
        print(f'Humidity: {humidity}%')
        print(f'Wind Speed: {wind_speed} m/s')
        print('====================================')
    else:
        print('Failed to retrieve weather data. Please check your input or try again later.')

except Exception as e:
    print(f'An error occurred: {e}. Please try again later.')


```