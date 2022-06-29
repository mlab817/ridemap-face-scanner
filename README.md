# RIDEMAP: Passenger Count through Face Detection using Smartphone Camera

## Description

This mobile app allows registered smartphones to upload data
of passengers' faces counted through face detection. 

`Note: This app does not collect data on faces of passengers, it 
relies instead upon the faceID created when a face is detected by 
the app's camera.`

## Requirements

1. [Free Expo account](https://expo.dev/signup)
2. Laravel for backend development
3. React Native and Expo for mobile app development
4. Smartphones with camera


The [Laravel backend for the Ridemap](https://github.com/mlab817/ridemap-php)
already supports this app. The endpoint for submission of face detection is `/api/faces`. 
The backend accepts an array of `faces` with the following attributes: `face_id`,
`station_id`, and `scanned_at`. `user_id` is extracted from a token
used to authenticate the device.

## Getting Started

Install the necessary tools:

1. Any IDE (Webstorm is preferred but you can also use Atom and VS Code)
2. Install Nodejs
3. Install Expo
4. Install Expo Go in IOS/Android
5. Install Git

Follow the following steps to get set up:

1. Clone this repository

```console
git clone https://github.com/mlab817/ridemap-counter.git
```

2. Change directory to ridemap-counter

```console
cd ridemap-counter
```

3. Install dependencies


```console
npm install
```

4. Update the API endpoint in utils.js file

```javascript
export const api = axios.create({
   baseURL: 'https://ridemap-php.herokuapp.com/api'
})
```

5. Start expo dev server


```console
expo start
```

Or

```console
expo r -c
```

The second command is used when you cannot connect your device to the webserver. 

Follow the instructions in the CLI message to connect your simulator or physical device.

## Building and Distributing the App

To build the app, install the Expo Application Services.

1. Create a free account in [Expo](https://expo.dev).
2. Install eas-cli to use eas in command prompt and/or terminal.

```console
npm i -g eas-cli
```

3. From the root directory, run:

```console
eas build
```

Follow the on-screen instructions. You will find the android/ios bundles in your Expo account under Build menu, e.g. https://expo.dev/accounts/{accountName}/projects/ridemap-counter/builds. You may also integrate submission to Play Store
and App Store.

> Note: Unfortunately, to build IOS applications, you will need to apply and register to Apple Developer Program which 
> costs $99 yearly. Android build is free and can be downloaded for distribution.

## Workflow

### Device Authentication

To use this app, devices must be registered in the server. To do
this, follow the instructions below:

1. Download the app
2. Open the app to connect to the server for the first time
3. The app will show an invalid device message along with the device ID.
Use this device ID to register the device in the server.

`
Note: The device ID does not refer to the actual device ID of the
device but rather the device ID of the app tied to the server. This
is unique for every app that is installed in the device.`

Read more here: [Android](https://docs.expo.dev/versions/v45.0.0/sdk/application/#applicationandroidid)
and [iOS](https://docs.expo.dev/versions/v45.0.0/sdk/application/#applicationgetiosidforvendorasync)

### Using the App

1. Open app
2. Device connects to server
3. Server verifies that device is registered
   1. If device is not registered, display invalid device message along with the device ID. Note that the device ID is not the actual device ID but rather the ID of the device tied with the app.
   2. If device is registered, proceed
4. Prompt user to select station where it is located
5. Place the smartphone in the best position
6. Start scanning

> The app will send data for every batch of faces detected.

## Table Structure

### - users

Stores data of users of the system

| Attribute | Type      | Description                                            |
|-----------|-----------|--------------------------------------------------------|
| id        | int autoincrement  | Primary key of the table |
| name      | varchar   | Name of the user                 |
| email     | varchar   | Email of the user               |
| email_verified_at     | timestamp   | Timestamp when the user validated their email              |
| password  | varchar   | Hashed password of the user     |
| device_id | varchar   | Device ID of the user               | |
| created_at| timestamp | Timestamp when the record was saved in the database    |
| updated_at| timestamp | Timestamp when the record was updated in the database  |

### - stations

Stores data of stations in EDSA Busway

| Attribute | Type               | Description              |
|-----------|--------------------|--------------------------|
| id        | int autoincrement  | Primary key of the table |
| name      | varchar            | Name of the station      |
| created_at| timestamp | Timestamp when the record was saved in the database    |
| updated_at| timestamp | Timestamp when the record was updated in the database  |

### - faces

Stores data on scanned QR codes

| Attribute | Type      | Description                                            |
|-----------|-----------|--------------------------------------------------------|
| id        | int autoincrement | Primary key of the table                 |
| face_id | int       | Face ID auto-generated by the device when the face was detected            |
| station_id| int       | Foreign key referencing stations table            |
| scanned_at| timestamp | Timestamp when the entry was generated |
| user_id   | int       | Foreign key referencing users table (user that submitted the data)   |
| created_at| timestamp | Timestamp when the record was saved in the database    |
| updated_at| timestamp | Timestamp when the record was updated in the database  |

## Screenshots

| ![Splash Screen](https://user-images.githubusercontent.com/29625844/176331478-e042c273-ffab-4603-ad00-693382aa546d.png) | ![Device ID](https://user-images.githubusercontent.com/29625844/176331526-db90c6f2-a3cc-48e8-8fe2-ad5bf46f146f.png) |
|:-------------:|:------------:|
| ![Request Permission](https://user-images.githubusercontent.com/29625844/176331417-ba934b85-d0cd-4374-948d-7e13d8bcda61.png) | ![Select Station](https://user-images.githubusercontent.com/29625844/176332218-291c28f1-666c-4fdf-88b6-259fd7178f55.png) |
| ![Scan Faces](https://user-images.githubusercontent.com/29625844/176331451-16f39acf-f98d-4ec7-b1c9-5dbef5183d67.png) | |

1. Screen 1 - Splash screen
2. Screen 2 - QR Code for the Device ID which can be scanned to easily copy the device ID
3. Screen 3 - Request permission to use camera
4. Screen 4 - Select station where device is located
5. Screen 5 - Camera

> The app submits data every after detection of new face ids.

## Limitations

The accuracy of this app will depend on at least the following factors:

1. Camera quality of the device
2. Quality of the environment (placement of the device, lighting)
3. Quality of targets (angle of the face, obstructions such as face masks/caps)

There is also the issue of duplication since the app does not use a
backend to analyze facial features and eliminate duplicates. So if a 
person takes too long in the queueing line, they can be scanned more than
once.
