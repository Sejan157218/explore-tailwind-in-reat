Install Tailwind via npm
Install Tailwind and its peer-dependencies using npm:

npm install -D tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
Create React App doesn’t support PostCSS 8 yet so you need to install the Tailwind CSS v2.0 PostCSS 7 compatibility build for now as we’ve shown above.

Install and configure CRACO
Since Create React App doesn’t let you override the PostCSS configuration natively, we also need to install CRACO to be able to configure Tailwind:

npm install @craco/craco
Once it’s installed, update your scripts in your package.json file to use craco instead of react-scripts for all scripts except eject:

  {
    // ...
    "scripts": {
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test",
     "start": "craco start",
     "build": "craco build",
     "test": "craco test",
      "eject": "react-scripts eject"
    },
  }
Next, create a craco.config.js at the root of our project and add the tailwindcss and autoprefixer as PostCSS plugins:

// craco.config.js
module.exports = {
  style: {
    postcss: {
      plugins: [
        require('tailwindcss'),
        require('autoprefixer'),
      ],
    },
  },
}
If you’re planning to use any other PostCSS plugins, you should read our documentation on using PostCSS as your preprocessor for more details about the best way to order them alongside Tailwind.

Create your configuration file
Next, generate your tailwind.config.js file:

npx tailwindcss-cli@latest init
This will create a minimal tailwind.config.js file at the root of your project:

// tailwind.config.js
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
Learn more about configuring Tailwind in the configuration documentation.

Configure Tailwind to remove unused styles in production
In your tailwind.config.js file, configure the purge option with the paths to all of your components so Tailwind can tree-shake unused styles in production builds:

  // tailwind.config.js
  module.exports = {
   purge: [],
   purge: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
    darkMode: false, // or 'media' or 'class'
    theme: {
      extend: {},
    },
    variants: {
      extend: {},
    },
    plugins: [],
  }
Read our separate guide on optimizing for production to learn more about tree-shaking unused styles for best performance.

Include Tailwind in your CSS
Open the ./src/index.css file that Create React App generates for you by default and use the @tailwind directive to include Tailwind’s base, components, and utilities styles, replacing the original file contents:

/* ./src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
Tailwind will swap these directives out at build-time with all of the styles it generates based on your configured design system.

Read our documentation on adding base styles, extracting components, and adding new utilities for best practices on extending Tailwind with your own custom CSS.

Finally, ensure your CSS file is being imported in your ./src/index.js file:

  // src/index.js
  import React from 'react';
  import ReactDOM from 'react-dom';
 import './index.css';
  import App from './App';
  import reportWebVitals from './reportWebVitals';

  ReactDOM.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>,
    document.getElementById('root')
  );

  // ...
