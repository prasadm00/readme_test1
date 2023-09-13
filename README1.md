
## Intro
​
This repository is structured around the principle of separation of concerns, containing two distinct endpoints. The first endpoint is an open graph image generator, and the second is a highchart image generator. Each endpoint is responsible for a specific functionality, ensuring that the codebase is modular and maintainable.
​
​
​
## OG Image Generator
​
The main functionality of og image generator is to be generating an image response based on certain parameters such as name, credentials, type, specialties, imageUrl, and snippet of the clinician.
​
The image response is created in the GET function in src/app/og/route.tsx. This function fetches the parameters from the URL's search parameters, and uses them to construct an image with various elements, including icons and text. 
​
The application is styled with Tailwind CSS, and the styles are applied using the tw prop, which is a feature of the Twin library that allows you to use Tailwind classes as component props.
​
suppose this
​
```bash
## API Endpoint: GET /og
​
This endpoint generates an open graph image. It requires the following query parameters:
​
### Request Query Parameters
​
name: The name of the clinician
credentials: The credentials of the clinician
specialties: The specialties of the clinician
type: The type of clinician
imageUrl: The URL of the clinician's image
snippet: A snippet about the clinician
​
### Sample Request
​
https://localhost:3000/og?name=Dr.+Adio+Mangrum&credentials=PT%2C+DPT&specialties=Orthopedics%2C+Sports+Therapy%2C+Vestibular+Disorders&type=Physical+Therapist&imageUrl=https%3A%2F%2Fwww.citypt.com%2Fstatic%2F846b1a63d162d1405da241ccb7474cf5%2Fb7804%2Fheadshot.png&snippet=Dr.+Mangrum+is+a+sport-+and+fitness-oriented+physical+therapist+treating+athletes+of+all+ages+and+genders.
​
```
​
## Highchart image generator
​
This app uses Highcharts to create charts and Puppeteer for screenshotting. The main component, App in src/app/chart/page.tsx, uses Next.js's useSearchParams hook to parse URL data into a Highcharts chart. This chart is converted to an SVG and set as an image source.
​
The POST function in src/app/api/chart/route.ts is a Next.js serverless function that generates webpage screenshots. It takes a request with a JSON body containing a url and data. The url is the target webpage, and the data constructs the URL's query parameters.
​
If the url is missing, it returns a JSON error message with a 400 status code. Otherwise, it launches a Puppeteer browser, sets the viewport size, constructs the full URL, and navigates to it. After a second, it screenshots the page in PNG format and returns it as a binary response with the "image/png" content type.
​
Errors are logged and returned as a JSON response with a 200 status code. The Puppeteer browser is always closed at the end.
​
​
```bash
## API Endpoint: POST /api/chart
​
This endpoint generates a webpage screenshot. It requires a JSON body with the following structure:
​
### Request Headers
​
Content-Type: application/json
​
### Request Body
​
{
    "url": "http://localhost:3000/chart",  // The target webpage for chart
    "data": {                             // The data to construct the URL's query parameters
        "title": {
            "text": "Render Test"         // The title of the chart
        },
        "series": [
            {
                "data": [1, 2, 3]         // The data for the chart
            }
        ]
    }
}
​
### Response
​
If the url is missing, the server will return a JSON error message with a 400 status code. 
​
If the request is successful, the server will return the screenshot of the page in PNG format as a binary response with the "image/png" content type.
​
If there is an error during the process, the server will log the error and return it as a JSON response with a 200 status code.
​
```
​
## Development
​
First, run the development server:
​
```bash
yarn dev
```
​
### Testing and Deployment
​
Our deployment process utilizes render.com to deploy this repository. The following steps outline our testing and deployment process:
​
1. **Local Testing**: Begin by testing the functionality on your local machine. This is an important step to ensure that the changes made are working as expected before they are merged into the main branch.
​
2. **Merge Request (MR)**: If you are satisfied with your changes after local testing, create a Merge Request (MR) to merge your changes into the main branch. This MR will be reviewed and, if approved, the changes will be merged into the main branch.
​
3. **Staging Deployment**: Once the MR is approved and merged into the main branch, it will be automatically deployed to our staging environment at images-staging.citypt.com. This allows for further testing in a production-like environment before the changes are deployed to the live site.
​
4. **Production Deployment**: For the changes to be deployed to the live site, merge the main branch into the develop branch. This triggers the deployment process and your changes will be live shortly.
​
Please note that it's crucial to thoroughly test your changes at each stage of this process to ensure smooth deployment and operation of our services.
​
​
