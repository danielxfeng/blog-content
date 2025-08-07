At Hive Helsinki, students need to request meeting room bookings through staff members, a process that was manual that's time-consuming. As students ourselves, we saw an opportunity to apply what we learned in school to solve a real-world problem.

**Book Me** was born from this need, [Abdul](https://github.com/IbnBaqqi) and me built a streamlined room booking system that eliminates the manual overhead while providing a smooth, modern user experience. And big thanks to [Jordane](https://github.com/jgengo), former CTO of Hive Helsinki, for his in-depth code reviews and many valuable suggestions via PRs and Discord.More importantly, he encouraged us to develop our own design ideas and was patient with many of our mistakes along the way.

![Booking Calendar Interface](https://github.com/danielxfeng/blog-content/blob/main/media/Book-Me-Meeting-Room-Booking-Calendar/Book-Me-Meeting-Room-Booking-Calendar.cover.jpg?raw=true)

- **Live Demo (Hive account required):** [room.hive.fi](https://room.hive.fi)
- **Frontend Repo:** [github.com/danielxfeng/booking_calendar](https://github.com/danielxfeng/booking_calendar)
- **Backend Repo:** [github.com/IbnBaqqi/book-me](https://github.com/IbnBaqqi/book-me)

---

## 🧪 Features

- 📅 **Weekly Calendar View** - Scrollable timeline showing all room bookings
- ✨ **One-Click Booking** - Add new reservations with intuitive time slot selection
- 🗑️ **Smart Deletion** - Role-based access control for booking management
- 📱 **Mobile-First** - Responsive design optimized for all devices
- 🔒 **Conflict Prevention** - Built-in validation prevents double-bookings
- 🎨 **Modern UI** - Clean, accessible interface with smooth animations

### Role-Based Access Control
- **Staff** can manage all bookings across the system
- **Students** can only modify their own reservations

---

## 🔧 Tech Stack

- **Frontend:** React + TypeScript + Vite
- **UI Components:** ShadCN/UI + Tailwind CSS
- **State Management:** Jotai for lightweight, reactive state
- **Data Fetching:** TanStack Query for intelligent caching
- **Form Handling:** React Hook Form + Zod validation
- **HTTP Client:** Axios with automatic token refresh
- **Testing:** Vitest for unit testing

---

## 🧠 My Thoughts

- **Complexity vs. Delivery**
  In both this booking system and my [Daniel's Lab](https://danielslab.dev/blog/posts/a-full-stack-website-daniels-lab) project, I realized that backend complexity often depends on concrete project requirements. But on the frontend, there’s almost no ceiling when it comes to polish and user experience. In [Daniel's Lab](https://danielslab.dev/blog/posts/a-full-stack-website-daniels-lab) project, for instance, I spent a lot of time on fine-tuning animations.

  However, during this project, Jordan reminded us of the importance of delivery speed. That feedback stuck with me. In today's [agile](https://en.wikipedia.org/wiki/Agile_software_development) development world, it's often more valuable to ship a working version quickly and then iterate based on user feedback, rather than trying to perfect every detail upfront based on [assumptions](https://www.interaction-design.org/literature/topics/assumptions#:~:text=Assumptions%20can%20influence%20the%20entire,that%20assumptions%20are%20not%20facts.).

  This really changed the way I think about balance. Great UX matters — but getting real feedback early matters more. The trick is finding the right trade-off between thoughtful design and timely delivery.

- **Exploring: Breaking Out of the Grid**
  At the beginning, I thought this would be a quick two-day frontend task, a simple calendar UI, nothing too complicated. By the second night, I had built a full grid system with over 600 cells on the screen (one for each 15-minute slot).

  But as I wired up `onHover`, `onClick`, and passed props to each slot, I noticed a significant drop in performance due to excessive re-renders. And another problem was for example when a single two-hour booking had to span 8 consecutive slots, which caused that the rendering logic became tightly coupled and hard to manage.

  While I could have optimized the performance (and yes, it's definitely possible to make it fast), I felt the design went against the core philosophy of React. A two-hour booking spans multiple grid cells, but it was still managed and controlled as if by a single grid cell, that just didn’t feel very "React-like" to me.

  That’s when I moved on to a second version: a design that **broke out of the grid**. The base grid was no longer interactive; it simply rendered static lines for visual reference. On top of that, I added a separate interaction layer. All interactions like `onHover` and `onClick` were handled through dynamic `pointer` based calculations, rather than per-slot bindings. Bookings were also rendered by dynamic calculation based on their start and end times, which are independent of any individual grid cell.

  I’m not sure yet if this is the best approach, but personally I prefer it because each DOM element only manages its own render scope, which feels much cleaner to me. I also used a pure function to prevent overlapping bookings, which made the logic easy to control.

- **Exploring: Breaking Out of the Grid**
  During development, I asked a few friends how they like a room booking interface to work. Most of them wanted to see multiple days of bookings at once, they could adjust their meeting plans based on availability across multiple days.

  So I initially planned a week view layout that aimed to reduce the need for clicking and scrolling by presenting more information to users. But we actually ran into a tradeoff: as we increased information density, we lost usability. The individual slots and grid cells became smaller, which made it harder to tap, especially on mobile devices.

  In the end, we settled on a multi-day view for mobile and a full week view for desktop. I divided the calendar layout into four parts:
  - the fixed top-left corner cell
  - a scrollable horizontal date row (top right)
  - a scrollable vertical time column (bottom left)
  - the main booking grid, which scrolls in both directions

  To keep everything in sync, I implemented a `scrollInSync` strategy: whenever the data grid scrolls, the horizontal offset is applied to the date row, and the vertical offset is applied to the time column. I’m not sure if this is the perfect solution (To be honest, even Google Calendar’s week view isn’t so great on mobile), but personally I quite like that. It allows us to present more information while still preserving a usable DOM with tappable elements.