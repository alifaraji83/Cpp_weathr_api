# Cpp_weathr_api#include <iostream>
#include <string>
#include <curl/curl.h>

static size_t WriteCallback(void* contents, size_t size, size_t nmemb, void* userp) {
((std::string*)userp)->append((char*)contents, size * nmemb);
return size * nmemb;
}

void getWeather(const std::string& city) {
std::string apiKey = "a0f6d98e96519128fdd0371dd91b1a93";  
std::string url = "https://api.openweathermap.org/data/2.5/weather?q=" + city + "&appid=" + apiKey + "&units=metric";

CURL* curl;
CURLcode res;
std::string readBuffer;

curl = curl_easy_init();
if (curl) {
curl_easy_setopt(curl, CURLOPT_URL, url.c_str());
curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, WriteCallback);
curl_easy_setopt(curl, CURLOPT_WRITEDATA, &readBuffer);
res = curl_easy_perform(curl);
curl_easy_cleanup(curl);

if (res == CURLE_OK) {
std::cout << "Weather data for " << city << ":\n" << readBuffer << std::endl;
} else {
std::cerr << "Failed to fetch weather data.\n";
}
}
}

int main() {
std::string city;
std::cout << "Enter a city name: ";
std::getline(std::cin, city);

getWeather(city);
return 0;
}
