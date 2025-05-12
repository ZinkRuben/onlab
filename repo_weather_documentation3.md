# Repository Documentation

## Table of Contents
1. [Overview](#overview)
2. [Explanation of Payload](#explanation-of-payload)
3. [Successful Request Clarification](#successful-request-clarification)
4. [JSON Response Format and Potential Errors](#json-response-format-and-potential-errors)
5. [get_geo_data Function](#get_geo_data-function)
6. [get_forecast_data Function](#get_forecast_data-function)
7. [print_daily_forecast Function](#print_daily_forecast-function)
8. [print_header Function](#print_header-function)
9. [Command-Line Argument Handling](#command-line-argument-handling)
10. [API Key Validity Check](#api-key-validity-check)
11. [Geographic and Weather Forecast Data Retrieval](#geographic-and-weather-forecast-data-retrieval)

## Overview
The repository provides a command-line weather forecasting application that integrates with external APIs to deliver geographic and weather data. It uses the Google Maps Geocoding API to convert user-specified addresses into geographic coordinates and the DarkSky.net API to fetch detailed weather forecasts based on these coordinates. The application processes command-line inputs to determine the location for which the forecast is needed, checks for the validity of API keys, and handles potential errors in data retrieval. The output includes a formatted weather forecast, displaying daily weather summaries and temperature ranges, aimed at providing users with a clear and concise weather outlook.

## Explanation of Payload
The `payload` is used to include query parameters in the GET request. These parameters specify conditions for the request, such as filters or specifications required by the API. The exact structure and content of the `payload` depend on the API being called, so consult the API documentation for details.

## Successful Request Clarification
A request is considered "successful" if it returns a status code of `200`. This indicates that the server has successfully processed the request and returned the requested data. If the status code is different from `200`, the function will return `None`, indicating failure.

## JSON Response Format and Potential Errors
On success, the function returns a JSON object containing the requested data, with its structure varying based on the API used. Check the API documentation for the expected JSON format. Potential errors may arise from issues such as network problems, invalid URLs, incorrect payload formats, or server/client errors, which result in the function returning `None`.

## get_geo_data Function
This function uses the Google Maps Geocoding API to get geographic coordinates for a specified address.

### Return Format
Returns a dictionary containing:
- `lat`: Latitude of the address.
- `lng`: Longitude of the address.
- `formatted_address`: The formatted representation of the address.

### Response Behavior
If a response is not obtained from the API, the function returns `None`.

## get_forecast_data Function
The `get_forecast_data` function retrieves forecast data from the DarkSky.net API.

### Forecast Data Returned
The function returns a dictionary containing daily forecast attributes, which may include temperature, precipitation, humidity, and other relevant weather information.

### Conditions for Returning None
The function returns `None` if the request to the DarkSky.net API fails, which occurs when the response status code is not 200, indicating an unsuccessful request (e.g., invalid parameters or network issues).

## print_daily_forecast Function

### Forecast Data Format

- **geo**: Dictionary containing geographic information, including:
  - `formatted_address`: The address for which the forecast is generated.

- **forecast**: Dictionary structured as follows:
  - `summary`: String summarizing the weekly weather.
  - `data`: List of daily forecasts, each containing:
    - `time`: Unix timestamp for the forecast day.
    - `summary`: Summary of the weather conditions.
    - `temperatureMin`: Minimum temperature for the day.
    - `temperatureMax`: Maximum temperature for the day.

### Output Information

- Prints the formatted address from the `geo` parameter.
- Prints the weekly summary from the `forecast['summary']`.
- For each day in `forecast['data']`, prints:
  - **Date**: Formatted as `DD/MM/YYYY`.
  - **Day Name**: Name of the day (e.g., "Today" or full weekday name).
  - **Weather Summary**: Brief description of weather.
  - **Temperature Range**: Minimum and maximum temperatures for the day, formatted with °C (e.g., "12°C - 20°C").

## print_header Function

### Specific Content
The header printed by the function is:

```
---------------------------------
     WEATHER FORECAST 1.O       
---------------------------------
```

### Context
This function is utilized in the weather forecasting application to format the output, clearly indicating the start of the weather forecast section.

## Command-Line Argument Handling
The `main` function requires at least one command-line argument, which specifies the location for the weather forecast. If the number of arguments is less than 2 or the `DS_API_KEY` is not set, the program exits with an error message.

## API Key Validity Check
Before proceeding, the function checks if the `DS_API_KEY` environment variable is `None`. If it is not set, the execution is halted with an error indicating that the API key must be available.

## Geographic and Weather Forecast Data Retrieval
The function uses `get_geo_data` to fetch geographic coordinates (latitude and longitude) based on the user-provided location. If the geographic data is invalid, the function exits with an error.

Subsequently, the `get_forecast_data` function retrieves the weather forecast using the obtained latitude and longitude. If the forecast data is invalid or not found, the program halts with an appropriate error message.