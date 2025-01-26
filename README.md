# Page-Speed-Insights
To use Google PageSpeed Insights in Python to get the performance scores for both mobile and desktop versions of a webpage, you will need to interact with the PageSpeed Insights API.

Here’s how you can set it up:
Prerequisites:

    Google Cloud API Key: You’ll need to generate an API key from the Google Cloud Console. After setting up the API and enabling the PageSpeed Insights API, you'll get an API key.

    Install Required Python Libraries: Install the requests library to make HTTP requests to the API.

    pip install requests

Python Code to Fetch PageSpeed Insights Data:

import requests

def get_pagespeed_insights(url, api_key):
    # Google PageSpeed Insights API endpoint
    endpoint = "https://www.googleapis.com/pagespeedonline/v5/runPagespeed"
    
    # Parameters for the request
    params = {
        "url": url,
        "key": api_key,
        "strategy": "mobile"  # mobile or desktop, to switch between the two
    }
    
    # Request for mobile
    response_mobile = requests.get(endpoint, params={**params, "strategy": "mobile"})
    # Request for desktop
    response_desktop = requests.get(endpoint, params={**params, "strategy": "desktop"})
    
    if response_mobile.status_code == 200 and response_desktop.status_code == 200:
        mobile_data = response_mobile.json()
        desktop_data = response_desktop.json()
        
        # Extract and display important details
        mobile_score = mobile_data['lighthouseResult']['categories']['performance']['score'] * 100
        desktop_score = desktop_data['lighthouseResult']['categories']['performance']['score'] * 100

        print(f"Mobile Performance Score: {mobile_score}%")
        print(f"Desktop Performance Score: {desktop_score}%")
        
        # Optionally, print other details like audits or page load time
        print("\nMobile Details:")
        for audit in mobile_data['lighthouseResult']['audits']:
            print(f"{audit}: {mobile_data['lighthouseResult']['audits'][audit]['displayValue']}")
        
        print("\nDesktop Details:")
        for audit in desktop_data['lighthouseResult']['audits']:
            print(f"{audit}: {desktop_data['lighthouseResult']['audits'][audit]['displayValue']}")
    else:
        print("Error fetching data from PageSpeed Insights API.")

# Usage Example
if __name__ == "__main__":
    url = "https://www.example.com"  # Replace with the URL you want to test
    api_key = "YOUR_GOOGLE_CLOUD_API_KEY"  # Replace with your API Key
    get_pagespeed_insights(url, api_key)

How it Works:

    URL & API Key: The function get_pagespeed_insights takes a URL and an API key as input.
    Strategy: The strategy can be set to "mobile" or "desktop" to fetch performance data for either version of the page.
    API Request: Two separate requests are made—one for mobile and one for desktop performance.
    Data Extraction: The performance score (between 0 and 100) is extracted from the response, which corresponds to the PageSpeed score. The response also contains details about various performance audits, such as page load times, which can be printed.
    Error Handling: The status code is checked to ensure the request was successful.

Example Output:

Mobile Performance Score: 70%
Desktop Performance Score: 85%

Mobile Details:
first-contentful-paint: 2.4 s
interactive: 7.2 s
total-blocking-time: 200 ms
...
Desktop Details:
first-contentful-paint: 1.5 s
interactive: 4.3 s
total-blocking-time: 80 ms
...

Notes:

    API Key: Make sure to use your actual API key from Google Cloud.
    Rate Limits: Google PageSpeed Insights API has usage limits. The free tier allows up to 25,000 requests per day. If you're making many requests, consider setting up quota management.
    Strategy: The strategy parameter can be mobile or desktop. mobile is for mobile-specific performance, and desktop is for desktop performance.

This should give you the PageSpeed Insights scores for both mobile and desktop versions of the webpage you're testing.
