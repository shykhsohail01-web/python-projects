# python-projects
#Python CLI that fetches live weather for any city using WeatherAPI.com and reads the temperature aloud with text-to-speech
import os
import requests
import pyttsx3

API_KEY = os.getenv("WEATHER_API_KEY")
BASE_URL = "https://api.weatherapi.com/v1/current.json"

def get_temperature(city):
    r = requests.get(BASE_URL, params={"key": API_KEY, "q": city}, timeout=10)
    r.raise_for_status()
    data = r.json()
    return data["current"]["temp_c"]

def main():
    city = input("Enter City:\n").strip()
    try:
        temp = get_temperature(city)
    except requests.exceptions.RequestException as e:
        print(f"Could not fetch weather: {e}")
        return

    message = f"The current temperature in {city} is {temp} degrees Celsius"
    print(message)

    engine = pyttsx3.init()
    engine.say(message)
    engine.runAndWait()

if __name__ == "__main__":
    main()  
