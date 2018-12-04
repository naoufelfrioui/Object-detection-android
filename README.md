# Machine Learning Data Management System (MLDMS)

This project helps to manage machine learning data.

## Prerequisites

*  Download and install Node.js (Version 10.14.1 for Windows 10) link: https://nodejs.org/en/

    -To test if node.js is working, open a command line window and type "node". The following output is expected:
    - `C:\Users\YOUR USER NAME>node `
    
* install [Angular CLI](https://cli.angular.io/) 

      - ` C:\Users\YOUR USER NAME> npm install -g @angular/cli `
      
* Download and install mongo database (Version 4.0.4 for Windows 10) link: https://www.mongodb.com/download-center/community?jmp=docs
 
    - Add the directory `C:\Program Files\MongoDB\Server\4.0\bin` to the environment variable PATH so as to get the mongo commands recognized. Open the  `Advance System Settings `, open  `Environment Settings ` and add the path `C:\Program    Files\MongoDB\Server\4.0\bin` for the System Variable PATH
    
    - Create the folder the location where you want to create the database for example  `c:\data\db`
    - start mongod: `C:\Users\YOUR USER NAME>mongod --dbpath`   for example  `--dbpath  "c:\data\db" `
   
    
* Some of the instruction may require you to have admin permissions on your system

## Installation

This will install all the dependencies required prior to running the service.

1.  Start MongoDB :  `C:\Users\YOUR USER NAME>mongod`
2.  Unpack the archive repository
3.  Open the command line window and navigate (using cd) to the repository  `.../machine-learning-data-management-system`
4.  Move into client folder and install dependencies with `.../machine-learning-data-management-system/client>npm install`
5.  Move into server folder and install dependencies with `.../machine-learning-data-management-system/server>npm install`

## Development (locally)

Use this process when in case you are a developer. The process will start two seperate development servers (angular-development-server and nodejs-development-server) which will automatically update upon any code changes you make (hot-reloading). In development mode, cross origin communication between client and is automatically enabled to enable communication between the different ports.

1.  After the initial installation (1 - 5)
2.  Run `npm start` in server folder to start the nodejs server in development mode
3.  Run `npm start` in client folder to start the angular server in development mode
4.  Navigate to http://localhost:4200/ to access the UI

## Production

Use this process when you want to deploy the project. If you intend to run the server in production mode somewhere else than the image server (e.g. Raspberry Pi) you need to change the base url variable in `client/src/environments/environment.prod.ts` to the url of the new device prior to generating the production build of the client for example `http://localhost:8000/api`. This is also necessary, if you intend to test the production build on you local machine.

1.  After the initial installation (1 - 5)
3.  Generate production build of frontend in client folder with `.../machine-learning-data-management-system/client> ng build --prod`
4.  Copy the generated `dist` folder from client folder to server folder or only copy `dist/client` if server already has a dist folder
5.  Generate production build of backend in server folder with `.../machine-learning-data-management-system/server> npm run build:prod`
6.  Start server from server folder in production mode with `.../machine-learning-data-management-system/server> npm run start:prod`
7. Navigate to http://localhost:8000/ to access the UI

## Endpoints

- GET /api/images/archive - Get images archived as zip

- GET /api/images - Get data for images (query option to get f.e. data for the last 10 images)

- GET /api/images/:id - Get single image by its id as binary

- POST /api/images - Upload images to server (<b>content-type has to be form-data</b>)

- POST /api/register - Register new user

- POST /api/login - Login existing user

- GET /api/profile - Get profile for user

- GET /api/annotation-sets - Get all annotation sets

- POST /api/annotation-sets - Create new annotation set

- GET /api/annotation-sets/:id - Get annotation set by id

- PUT /api/annotation-sets/:id - Update existing annotation set

- DELETE /api/annotation-sets/:id - Delete existing annotation set

- GET /api/annotation-sets/:id/progress - Get progress of annotation set

- GET /api/annotations/ - Get annotations (query option, e.g. annotation set)

- POST /api/annotations/ - Create new annotation

- GET /api/annotations/:id - Get annotation by id

- PUT /api/annotations/:id - Update existing annotation

- DELETE /api/annotations/:id - Delete existing annotation

## Image Upload

If you intend to send images from an external application, use form-data to send the images. You first create a form-data object and append
key/value pairs to it. Use the key `images` to append images (file objects) to the form-data object. Use the key
`scenario` to define the scenario of the uploaded images. Use the key `createdAt` to define the date when the images where created. I suggest to use ISO 8601 as date format (e.g. yyyy-mm-ddThh:mmZ => 2018-07-19T15:18:21.407Z). All of these fields are mandatory for a successful upload.

Here is an example of how it could be done with JavaScript:

```javascript
const createdAt = new Date().toISOString();
const formData = new FormData();

// define scenario for images using the key <scenario>
formData.append('scenario', 'some_scenario');

// define createdAt for images using the key <createdAt>
formData.append('createdAt', createdAt);

// files is a list of file objects (images in this case)
// we append each file to the form-data object by using the key <images>
Array.from(files).forEach(file => formData.append('images', file));

// define appropriate headers for request
const headers = new HttpHeaders();
headers.append('Content-Type', 'application/form-data');

// send request with library of you choice ...
```

Here is another example with Python:

```python
import requests
import datetime
import os

path_to_images = "C:/Users/Public/Pictures/Sample Pictures"
image_name = "Jellyfish.png"
image_name2 = "Lighthouse.jpg"
scenario = "some_scenario"
createdAt = '2018-07-19T15:18:21.407+02:00'

# path is path to image
image = open(os.path.join(path_to_images, image_name), 'rb').read()
image2 = open(os.path.join(path_to_images, image_name2), 'rb').read()

# use key images to define image file
# request lib requires additional file_info
files=[('images', (image_name, image)), ('images', (image_name2, image2))]

# request sends this as form-data because we use files
url = "http://de-muczarin01:8000/api/images"
r = requests.post(url, files=files, data={"scenario": scenario, "createdAt": createdAt})
print(r)
```

## Error Messages

- "connection refused" in client when accessing rest api -> indicates that client/environments/environment.prod.ts has not been changed to the ip of the rest api





