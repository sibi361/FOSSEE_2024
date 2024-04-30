# FOSSEE Fellowship 2024 Submission

This repository contains my submission for the screening task of [FOSSEE Fellowship 2024](https://fossee.in/fellowship/2024). It involved porting a given static [website](https://iot-gis-hackathon.fossee.in/) to the [Astro framework](https://astro.build/) while utilising it's content-driven features such as components and layouts.

## Usage

All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev -- --host`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |


## Project Structure

```text
/
├── public/
├── src/
│   └── components/
│       └── homepage/
│           └── About.astro
│           └── FAQs.astro
│           └── Introduction.astro
│           └── Partners.astro
│           └── Register.astro
│           └── TeamAndContact.astro
│       └── Footer.astro
│       └── Meta.astro
│       └── Navigation.astro
│       └── Poster.astro
│       └── SocialMediaStrip.astro
│       └── TopBanner.astro
│   └── data/
│       └── faqs.json
│       └── resources.json
│       └── scheduleWeeks.json
│       └── team.json
│   └── layouts/
│       └── BaseLayout.astro
│   └── pages/
│       └── index.astro
│       └── resources.astro
│       └── schedule.astro
└── package.json
```

## Porting Documentation

1. The first step was to instal the Astro framework locally and create a new project:

```sh
npm create astro@latest
```

2. Upon running the start command (`npm run dev`) given in the [Astro docs](https://docs.astro.build/en/install/auto/) the CLI reported the server to be running but it wasn't loading upon visiting http://localhost:4321. After some reading it was found that the server wasn't listening on all network interfaces. Hence the following command was used to expose the server on all local interfaces:

```sh
npm run dev -- --host
```

3. As the source code for the target website wasn't provided, we utilise [`wget`](https://www.gnu.org/software/wget/) to mirror the website:

```sh
wget --mirror https://iot-gis-hackathon.fossee.in/
```

4. We were able to load the downloaded `index.html` file through the Astro server by replacing the default `src/pages/index.astro` file with it.

5. As the next step we modularised broad sections of HTML such as the navigation, social-media links, footer etc into [Astro components](https://docs.astro.build/en/basics/astro-components/).

6. Then we imported these components into the base layout file [`src/layouts/BaseLayout.astro`](src/layouts/BaseLayout.astro). This base layout was further used for all three pages; [`index`](src/pages/index.astro), [`resources`](src/pages/resources.astro) and [`schedule`](src/pages/schedule.astro) thereby reducing the amount of redundant code.

7. Upon noticing the repetitive nature of content in the Resources and Schedules page, we used Astro's feature of being able to parse javascript within the HTML (like ReactJS) to dynamically generate such content's HTML.

This approach was initially giving no output, i.e. the contents of the `<div class="main-timeline bg-light">` was blank. Upon referring to online tutorials we realised that we were using an incorrect syntax: the anonymous function call must use `(` instead of the conventional`{`.

Incorrect syntax:

```js
{
    weeksJSON.map((week) => {
        <>
            <div class="week">
                <h2>{week.title}</h2>
            </div>
        </>
    })
}
```

Correct syntax:

```js
{
    weeksJSON.map((week) => (
        <>
            <div class="week">
                <h2>{week.title}</h2>
            </div>
        </>
    ))
}
```

8. Lastly we further broke down the homepage's sections into reusable components at [`src/components/homepage`](src/components/homepage) and wnabled dynamic generation of FAQs' HTML using javascript.

## Legal

This project is licensed under the terms of the [Creative Commons Attribution-ShareAlike 4.0 International License](LICENSE).
