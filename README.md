# Current COVID Data

[![Laravel Forge Site Deployment Status](https://img.shields.io/endpoint?url=https%3A%2F%2Fforge.laravel.com%2Fsite-badges%2F6071bc7f-6d51-4d7f-b836-07138f141326%3Fdate%3D1&style=plastic)](https://forge.laravel.com/servers/501534/sites/1583742)

This app was created to track COVID data in my county.
It fetches county cdata and compares it to the data from it's state.

The Application uses the [COVID ACt Now API](https://apidocs.covidactnow.org/) as the source of its data.

### Local Development
`npm install` to install dependencies

`npm run dev` to start the development server

### Deployment
point the webserver to the `/dist` directory