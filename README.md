# OpenWeatherAPIProblem

# Get forecast data for a given city list
get_weather_forecaset_by_cities <- function(city_names) {
  df <- data.frame()
  for (city_name in city_names) {
    # Forecast API URL
    forecast_url <- "https://api.openweathermap.org/data/2.5/forecast"
    # Create query parameters
    forecast_query <- list(q = city_name, appid = "55bcac687734ea585703e90e7dcf8e7e", units = "metric")
    # Make HTTP GET call for the given city
    forecast_response <- GET(forecast_url, query = forecast_query)
    # Note that the 5-day forecast JSON result is a list of lists. You can print the response to check the results
    forecast_json_list <- content(forecast_response, as = "parsed")

    # Loop the json result
    for(result in results) {
      city <- c(city, result$city_name)
      weather <- c(weather, result$weather[[1]]$main)
      visibility <- c(visibility, result$visibility)
      temp <- c(temp, result$main$temp)
      temp_min <- c(temp_min, result$main$temp_min)
      temp_max <- c(temp_max, result$main$temp_max)
      pressure <- c(pressure, result$main$pressure)
      humidity <- c(humidity, result$main$humidity)
      wind_speed <- c(wind_speed, result$wind$speed)
      wind_deg <- c(wind_deg, result$wind$deg)
      forecast_datetime <- c(forecast_datetime, result$dt_text)
      season <- c(season, "winter")
    }
      
    # Add the R Lists into a data frame
    df <- data.frame(
      city = city, 
      weather = weather,
      visibility = visibility,
      temp = temp,
      temp_min = temp_min,
      temp_max = temp_max,
      pressure = pressure,
      humidity = humidity,
      wind_speed = wind_speed,
      wind_deg = wind_deg,
      forecast_datetime,
        season=season)
  }

  # Return a data frame
  return(df)
}
