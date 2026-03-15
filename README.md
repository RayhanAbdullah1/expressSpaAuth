# expressSpaAuth

A simple full-stack SPA framework built from scratch — file-based routing, 
Express.js backend and MySQL database. No React, no Vue, no overhead.

## Why
Most full-stack frameworks come with too much. This is a clean, minimal alternative 
for small projects that need routing, auth and a database without the complexity.

## Stack
- Node.js + Express.js — server, API & backend logic
- MySQL + Knex.js — database & query builder
- Vanilla JS — custom SPA routing engine

## Features
- File-based routing
- Protected routes
- MySQL database integration
- Full-stack in a single repo
- Zero frontend framework dependencies

## Getting Started
\`\`\`code
git clone https://github.com/RayhanAbdullah1/expressSpaAuth
npm install
cp .env.example .env
# configure your .env with DB credentials
npm start
\`\`\`

## Structure
\`\`\`
├── app/          # backend routes & controllers
├── public/       # frontend SPA pages
├── index.js      # entry point
└── env.js        # environment config
\`\`\`
