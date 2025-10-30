import requests
import datetime

# Function to get weather data
def get_weather(city, api_key):
    base_url = "http://api.openweathermap.org/data/2.5/weather?"
    complete_url = f"{base_url}q={city}&appid={api_key}&units=metric"
    
    try:
        response = requests.get(complete_url)
        data = response.json()
        
        if data["cod"] != 200:
            print("City not found. Please try again.")
            return None
        
        # Extract weather details
        main = data["main"]
        weather = data["weather"][0]
        wind = data["wind"]

        weather_info = {
            "City": city.title(),
            "Temperature": main["temp"],
            "Feels Like": main["feels_like"],
            "Humidity": main["humidity"],
            "Weather": weather["description"].title(),
            "Wind Speed": wind["speed"],
            "Date": datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        }
        return weather_info

    except requests.exceptions.RequestException as e:
        print("Network error:", e)
        return None


# Function to display weather data nicely
def display_weather(info):
    print("\n--- WEATHER REPORT ---")
    print(f"City: {info['City']}")
    print(f"Date & Time: {info['Date']}")
    print(f"Temperature: {info['Temperature']}°C")
    print(f"Feels Like: {info['Feels Like']}°C")
    print(f"Humidity: {info['Humidity']}%")
    print(f"Weather: {info['Weather']}")
    print(f"Wind Speed: {info['Wind Speed']} m/s")
    print("-----------------------\n")


# Main program loop
def main():
    api_key = "YOUR_API_KEY"  # Replace with your actual API key
    print("=== Welcome to the Weather Detection App ===")

    while True:
        city = input("Enter city name (or 'exit' to quit): ").strip()
        if city.lower() == "exit":
            print("Thank you for using the app. Goodbye!")
            break
        
        weather_info = get_weather(city, api_key)
        if weather_info:
            display_weather(weather_info)


if _name_ == "_main_":
    main()
