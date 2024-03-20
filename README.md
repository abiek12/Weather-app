# Weather Application

A simple weather application developed using Vue.js and Tailwind CSS. It fetches weather information from the OpenWeatherMap API and displays it in a user-friendly interface. Users can search for weather by city name, save multiple locations, and view weather forecasts.

## Features

- Weather data retrieval: Fetch weather information based on city name.
- Save locations: Users can save multiple locations for easy access.
- Responsive UI: Mobile-friendly design using Tailwind CSS.
- Graphical representation: Display weather data graphically.
- Error handling: Proper handling of network requests and errors.

## Technologies Used

- Vue.js
- Tailwind CSS
- Axios

## Getting Started

1. Clone the repository:

```bash
git clone https://github.com/your-username/weather-application.git
```

2. Navigate to the project directory:

```bash
cd weather-application
```

3. Install dependencies:

```bash
npm install
```

4. Create a `.env` file in the root directory and add your OpenWeatherMap API key:

```
VUE_APP_API_KEY=your-api-key
```

5. Run the development server:

```bash
npm run serve
```

6. Open your browser and navigate to `http://localhost:3000` to view the application.

I've removed the section about user authentication and updated the features accordingly. If you have any further modifications or additions, let me know!

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```
