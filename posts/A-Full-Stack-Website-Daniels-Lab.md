Daniel’s Lab was the first full stack website I built. Firstly, it’s my **personal portfolio website**, a place to show what I’ve been working on. Also, it’s a **central hub** where I keep all of my projects in one place.

On top of that, I wanted somewhere to share what I’ve learned along the way. So I put together a **blog management system**.

---

![Screenshot](https://github.com/danielxfeng/daniels_lab_frontend_react/raw/main/public/Screenshot.jpg)

---

[Click to visit Daniel's Lab](https://danielslab.dev)
[Link to github repo](https://github.com/danielxfeng/daniels_lab)

---

## ✨ **Features**

### Frontend

- **Particle Gravity**
  To keep the site from feeling too boring, I added a lightweight particle system, basically a tiny background “mini‑game” on the homepage.

- **Seamless Authentication**
  Sign‑ins use silent token refresh, and refresh tokens are bound to a device fingerprint for extra security. Sessions stay active without constant logins. (And still without cookies)

- **Full‑Text Search with Local Suggestions**
  The search lets users do full text queries and also suggests results based on local search history for a more personal touch.

- **Blog Filtering with Debounced Auto‑Submit**
  Blog filters auto‑submit when users input stops changing for a moment (no extra button clicks needed).

- **Animations**
  Some animations make the site feel alive, hopefully not too much, though. 🙂

- **Tag Input with Throttle and Drag‑to‑Delete**
  When creating or updating posts, the tag selector shows real time suggestions with throttling and debouncing, plus responsive drag‑to‑delete interactions. (Unfortunately, this one’s admin‑only — just for me 🙂)

### Backend

- **Modular and Stateless Design**
  The backend is split into independent modules, authentication and blog APIs can be separated and fully stateless. This makes it easy to scale or even split them into microservices in the future if needed.

- **Authentication from Scratch**
  Built a custom authentication module from scratch, supporting both password login and multiple OAuth providers (Google, GitHub, LinkedIn).
  Implemented a dual‑token system with refresh tokens following a rotation policy: each refresh token is valid for a single use only.

- **Pluggable Full‑Text Search (Elasticsearch / Meilisearch)**
  Full text search is abstracted behind a unified interface, so the engine can be swapped out easily. (I first built it with Elasticsearch, but my tiny VPS couldn’t handle its memory usage.)

- **User Data Validation Middleware**
  Requests are validated upfront with a validation middleware, ensuring consistent and secure data handling across all API endpoints.

### Deployment

- **CI/CD Pipeline**
  Pushing to `main` runs the full test suite and, if all checks pass, deploys automatically. This keeps releases frequent and low‑risk — no manual steps, no surprises.

- **Hybrid SaaS + VPS Architecture**
  The frontend runs on **Vercel**, the database lives on **Neon**, and the backend plus search services are deployed to a lightweight **Azure VPS**. Everything sits behind **Cloudflare**, giving unified routing and security without extra complexity.

- **One‑Command VPS Provisioning**
  Built a custom script to configure the VPS from scratch: install dependencies, set up services, and even switch providers or scale up with a single command.

## 🔧 Tech Stack

**Frontend**
- React 19 with React Router 7
- Zustand for state management
- React Hook Form + Zod for form handling and validation
- Tailwind CSS 4 for styling and theming
- ShadCN UI component library
- Framer Motion for animations
- Custom particle system built with Three.js and React Three Fiber (R3F)
- Drag‑and‑drop interactions with @dnd-kit

**Backend**
- Node.js
- PostgreSQL
- Custom Auth with JWT and OAuth2 (Google/GitHub/LinkedIn)
- Full‑text search with pluggable engines (Elasticsearch / Meilisearch)
- Middleware‑based request validation

**Infrastructure**
- Vercel (frontend hosting)
- Neon (PostgreSQL database)
- Azure VPS (backend & search)
- Cloudflare (routing & security)
- GitHub Actions (CI/CD with submodule auto‑update)

---

## 🧠 Some Thoughts

I started this project in late April 2025 and, working on and off, finished it in early August, roughly 370 hours in total:

- **~100 hours on the backend**, about half of which was spent on **Custom Authentication**.
- **~230 hours on the frontend**, which was far more than I expected.
- **~40 hours on DevOps**, my first real hands‑on experience with deployment pipelines.

Building a real world website turned out to be much harder than I imagined. Compared to the educational projects I had worked on before, there were lots of differences:

- **Evolving requirements**
  In educational projects, requirements are fixed and the complexity is predefined and bounded, so developers can follow a clean, well‑planned roadmap.
  In real‑world projects, however, both features and appearance evolve with user feedback. Requirements shift, designs sometimes fall short of expectations, and user testing can change the development path entirely.

  An example was the full‑text search feature. I initially implemented it with Elasticsearch, since it’s widely used in production systems. But after the first round of tests, I found it was consuming over 2 GB of memory, far beyond my 1 GB Azure VPS. So I had to switch to another approach. Fortunately, I had modularized the search layer from the start, methods like `upsertIndex` and `search` were abstracted behind an interface. So replacing Elasticsearch with Meilisearch only required implementing that interface instead of rewriting the entire feature.

  These experiences taught me an important lesson: a “perfect design” rarely exists from the start, and new features often need to be integrated without a full rewrite. Here, **modularity**, **flexibility in implementation**, and **timely local refactoring** were crucial to keeping the project evolvable.

- **AI was both helpful and limited**
  At the beginning, I relied heavily on tools like Copilot to get up to speed with TypeScript syntax — especially coming from a dynamic typing background. The shift to static typing, with all the `union` and `exclude` type gymnastics, completely confused me at first. AI suggestions helped me go through that phase quickly. And more importantly, AI often gave me quick examples to try out, which was especially useful when some libraries lacked examples or had too complex API docs (like figuring out how to set up Google OAuth, the docs were scattered across different articles).

  But later, most of that AI‑generated code got rewritten as I refactored and understood the architecture better. The main issue was that AI code often felt like a “toy version”, it might run, but lacked proper error handling and was hard to integrate with the design or style of other modules. Once I was more familiar with TypeScript, I just turned the AI assistant off, because most of the time its suggestions were rarely helpful.


- **Plenty of over‑engineering — on purpose**
  I intentionally explored concepts I’d only learned in theory, like stateless design, pluggable search engines, silent token refresh, even if they weren’t strictly necessary.
  It slowed things down but taught me a lot, and it’s satisfying to see those ideas work in practice.

