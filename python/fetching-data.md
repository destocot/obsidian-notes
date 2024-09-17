---
title: "Fetching Data"
sidebar:
  order: 15
---

## Fetching Data with urllib

```py
from urllib import request
import json
from rich import print_json

def get_posts():
    url = "https://jsonplaceholder.typicode.com/posts"

    try:
        with request.urlopen(url) as response:
            data = response.read()

            parsed_json = json.loads(data)
            return parsed_json
    except:
        print("could not fetch the data.")
        return None

def main():
    posts = get_posts()
    print_json(data=posts)

if __name__ == "__main__":
    main()
```

## Fetching Data using the Requests Package

The **requests** package makes it easier to with HTTP requests and responses.

```py
import requests
from rich import print_json

def get_posts():
    url = "https://jsonplaceholder.typicode.com/posts"

    response = requests.get(url)
    parsed_json = response.json()
    return parsed_json

def main():
    posts = get_posts()
    print_json(data=posts)

if __name__ == "__main__":
    main()
```

## Adding Query Parameters

```py
import requests
from rich import print_json

def get_posts(user_id=1):
    url = "https://jsonplaceholder.typicode.com/posts"
    params = {"userId": user_id}

    response = requests.get(url, params=params)
    parsed_json = response.json()
    return parsed_json

def main():
    posts = get_posts(user_id=3)
    print_json(data=posts)

if __name__ == "__main__":
    main()
```

## POST Requests

```py
import requests
from rich import print_json

def save_post(data):
    url = "https://jsonplaceholder.typicode.com/posts"

    response = requests.post(url, data=data)
    parsed_json = response.json()
    return parsed_json

def main():
    response = save_post({"userId": 1, "title": "foo", "body": "bar"})
    print_json(data=response)

if __name__ == "__main__":
    main()
```

## Handling Errors

```py
import requests
from rich import print_json

def get_something():
    url = "https://jsonplaceholder.typicode.com/xyz"

    try:
        response = requests.get(url)

        response.raise_for_status()

        parsed_json = response.json()
        return parsed_json
    except requests.exceptions.HTTPError as error:
        print(f"there was an HTTP error: {error}")
    except requests.exceptions.RequestException as error:
        print(f"there was an error: {error}")

def main():
    data = get_something()
    print_json(data=data)

if __name__ == "__main__":
    main()
```

## Using the Open Weather API

[forecast5](https://openweathermap.org/forecast5)

[geocoding-api](https://openweathermap.org/api/geocoding-api)

```py
import requests
from rich import print_json

key = "28d4883cb180676d90561d43c5caa22f"
geo_url = "http://api.openweathermap.org/geo/1.0/direct"
weather_url = "https://api.openweathermap.org/data/2.5/forecast"

def get_geo_location(city):
    params = {"q": city, "appid": key}
    response = requests.get(geo_url, params=params)
    parsed_data = response.json()
    return parsed_data

def get_forecast(lat, lon):
    params = {"lat": lat, "lon": lon, "appid": key}
    response = requests.get(weather_url, params=params)
    parsed_data = response.json()
    return parsed_data

def main():
    _coords = get_geo_location("London")
    coords = _coords[0]

    _forecast = get_forecast(coords["lat"], coords["lon"])
    forecast = _forecast["list"]

    print(len(forecast))
    print_json(data=forecast)

if __name__ == "__main__":
    main()
```

## CHALLENGE - Weather Report

```py
import requests
import csv
from pathlib import Path
import pendulum

BASE_API_URL = "http://api.openweathermap.org"
APP_ID = "28d4883cb180676d90561d43c5caa22f"


def get_geo_location(city):
    url = f"{BASE_API_URL}/geo/1.0/direct"
    params = {"q": city, "appid": APP_ID}

    response = requests.get(url, params=params)

    parsed_data = response.json()
    return parsed_data


def get_forecast(lat, lon):
    url = f"{BASE_API_URL}/data/2.5/forecast"
    params = {"lat": lat, "lon": lon, "appid": APP_ID}

    response = requests.get(url, params=params)
    parsed_data = response.json()
    return parsed_data


def create_csv_report(city, forecast_data):
    file_name = city.lower().replace(" ", "_")
    file_path = Path(__file__).parent / f"{file_name}.csv"

    with file_path.open("w", newline="") as file:
        csv_writer = csv.writer(file)
        csv_writer.writerow(["Date / Time", "Weather Description"])

        rows = [
            (
                pendulum.from_timestamp(entry["dt"]).to_day_datetime_string(),
                entry["weather"][0]["description"],
            )
            for entry in forecast_data["list"]
        ]

        csv_writer.writerows(rows)

    print(f"CSV weather report for { city } was created.")


def main():
    # CHALLENGE 1 - get city coordinates and forecast
    city = input("Create weather report for which city? ").strip()
    _coords = get_geo_location(city)

    if not _coords:
        print(f"Could not find coordinates for {city}")
        return

    coords = _coords[0]
    forecast_data = get_forecast(coords["lat"], coords["lon"])

    # CHALLENGE 2 - Create a CSV weather report from forecast_data
    # --> Call create_csv_report with city & forecast_data
    # --> Use pathlib to create a file path with the city name
    # --> Loop through forecast_data['list'] to write 'dt_txt' and 'weather description'
    # --> Extract 'dt' and 'weather description' for each entry
    # --> Bonus: use pendulum to format 'dt_txt'

    create_csv_report(city, forecast_data)


if __name__ == "__main__":
    main()
```
