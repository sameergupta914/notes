`npm install -D tailwindcss@3.4.17 postcss autoprefixer `
`npx tailwindcss init -p`

for react:
    - `npm install -D tailwindcss@3`
    - `npx tailwindcss init`
    - in tailwind.config.js: content: ["./src/**/*.{js,jsx,ts,tsx}",],
    - in index.css
        @tailwind base;
        @tailwind components;
        @tailwind utilities;
    then npm run start
    