- lovebabbar mtd:
    - `npm install -D tailwindcss@3.4.17 postcss autoprefixer `
    - `npx tailwindcss init -p`

- For React:
    - `npm install -D tailwindcss@3`
    - `npx tailwindcss init`
    - in tailwind.config.js: content: ["./src/**/*.{js,jsx,ts,tsx}",],
    - in index.css
        @tailwind base;
        @tailwind components;
        @tailwind utilities;
    - then npm run start
    
- For gap: `space-x-6`, `space-y-8`, `gap-4`, `gap-x-0`

- For some text: 
    `text-white font-mullish py-7 hover:text-lightBlue cursor-pointer transition-all duration-200 relative group hidden lg:block`, `font-bold text-[40px] leading-[1.2] opacity-70`

- For some div:
    `absolute bottom-0 w-full h-1 bg-lightBlue hidden group-hover:block transition-all duration-200` , `w-10/12 max-w-[1080px] flex sm:flex-col lg:flex-row justify-between items-center mx-auto ` , 

- Padding, Margin: `py-3 px-5 pt-4`, `mt-4 mb-6`, `py-[14px] px-[18px]`

- Height, Width: `w-[14px] h-[14px]` , `w-full max-w-[680px]`, `w-[103%]`, `min-h-[15rem]`

- For some img: `loading="lazy" width="233" height="167" class="absolute top-[3rem] right-0 inline-block rotate-180"`

- Flex: `flex flex-col items-center justify-between justify-evenly flex-col-reverse`

- translate: `transition-all duration-200`

- Responsiveness: `hidden md:block`

- when you have to fix posi of something: `"max-w-[600px] absolute right-0 bottom-0 hidden md:max-w-[400px] lg:max-w-[600px] md:block lg:block`

- grid eg: `w-full grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 mt-10`

- z-position: `z-[8]`, 