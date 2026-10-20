# Video lesson library

A library of video lessons for one school. Tutors record a lesson and
upload it. Learners on the course watch it. Everyone else is refused,
even if they have the video's web address.

## What finished looks like

Anyone can open the app, create an account, and ask to join a course.
After that:

- a tutor uploads a lesson of 1 to 2 GB, far too big for a normal web
  request
- if the connection drops halfway, the tutor carries on the next
  morning from where it stopped, not from the start
- the lesson says "Preparing" while it is converted for the web, then
  "Ready"
- an enrolled learner watches it on a phone, and can skip back 30
  seconds
- anyone not enrolled is refused, even holding the video's own address
- nobody has to look after the library by hand to keep all this true

Finished means it is live on the internet, not just on your laptop.
Someone you have never met can upload a lesson, someone else can watch
it, and a third person cannot.

## The road map

Eight sprints. Each sprint delivers one part of the list above. They
must be done in order, because each one builds on the one before.

| # | Sprint | What it delivers |
|---|---|---|
| 1 | Getting in | Accounts, password reset, and tutor accounts that only an administrator can grant |
| 2 | The roll | Courses, learners accepted onto them, and enrolment with a start and an end date |
| 3 | The two gigabyte lesson | A tutor uploads a 2 GB file straight to storage, without it passing through the app |
| 4 | Dropped at ninety percent | An upload that survives a lost connection and a closed laptop |
| 5 | Ready is not uploaded | Lessons are converted for the web, say which state they are in, and then play |
| 6 | A link is not a permission | The video itself refuses anyone who is not enrolled |
| 7 | Work nobody is watching | Failed work is seen and retried, nothing is done twice, and abandoned files are cleared |
| 8 | Put it where the class is | The app is live, and strangers upload, watch and are refused |

## The hard parts

**A video is the wrong shape for a web request.** A 40-minute lesson
is often 1 to 2 GB. At 7 megabits per second, a 2 GB upload takes
about 40 minutes. It cannot go through the app the way a form does. It
goes straight from the tutor's browser to storage, and a dropped
connection must not mean starting again.

**Hiding the player hides nothing.** The video file is served by
something other than the page. That something must refuse people who
are not enrolled. The test: copy a working video address, open it
signed out or as a learner on another course, and be refused.

**Most of the work happens when nobody is watching.** Converting a
video takes minutes and runs after the tutor has gone home. If it
fails at 2am, someone must find out.

## Working on it

The sprints, their briefs and their tickets are in Blacksmith
Experience. Start with sprint 1. Each sprint opens when the one before
it is finished.

Read the documents attached to a sprint before you start its first
ticket. They explain the choices you will make and the options you are
choosing between.

## What this is built with

The whole product: an API and the screens that use it.

- **Express 5** in **TypeScript** for the API.
- **Prisma** for the models and the migrations, against the schema in `prisma/schema.prisma`.
- **Zod** for validating every request, and **zod-to-openapi** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **JWT** for authentication, in `src/modules/auth`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, Prisma client, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the JWT secret, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── prisma/
│   └── schema.prisma    # your models, and the migrations they generate
├── src/
│   ├── config/          # environment, OpenAPI, Zod setup
│   ├── db/              # the Prisma client, made once
│   ├── middleware/      # authentication, validation, error handling
│   ├── modules/
│   │   └── auth/        # register, log in, refresh
│   ├── utils/           # errors, pagination, tokens
│   ├── app.ts           # where routes are mounted
│   └── index.ts
├── package.json
└── tsconfig.json

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a Zod schema or a route. |
| `blacksmith make:resource Post` | Scaffold a Prisma model, Zod schemas, a service, a controller and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run an npm script in the backend, e.g. `blacksmith backend run migrate`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Express and React project. Nothing here is a dependency on us. |
