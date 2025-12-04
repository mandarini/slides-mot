---
theme: seriph
background: https://images.unsplash.com/photo-1518709268805-4e9042af2176?ixlib=rb-4.0.3&auto=format&fit=crop&w=2426&q=80
class: "text-center"
highlighter: shiki
lineNumbers: false
info: |
  ## Stop Mocking Your Backend
  Testing with Real Supabase Backends
drawings:
  persist: false
css: unocss
wakeLock: "build"
---

# Stop Mocking Your Backend

## Katerina Skroumpelou

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    MOT Athens <carbon:arrow-right class="inline"/>
  </span>
</div>

<p><em>Follow along: slides-mot.netlify.app</em></p>

<div class="abs-br m-6 text-xl">
  <carbon-logo-angular class="text-red-500" />
  <span class="mx-2">+</span>
  <logos-supabase-icon />
</div>

<!--
Hi everyone! I'm excited to show you what happens when we combine Angular's reactive power with Supabase's real-time capabilities. We're going to build a multiplayer party game that handles unlimited players, live cursors, and real-time interactions. By the end of this talk, you'll know exactly how to build your own real-time multiplayer experiences.
-->

---

layout: center
class: text-center

---

# About Me

<div class="grid grid-cols-2 gap-12 items-center">
<div>

# Katerina Skroumpelou

- Software Engineer at **Supabase**
- Loves cats and chocolate 🐱🍫
- Loves to be on stage
- **GDE** for Angular & Maps

## psyber.city - @psyber.city

</div>
<div>
<img src="/images/katsup.jpg" class="rounded-full w-64 mx-auto shadow-lg">
</div>
</div>

<!--
Quick intro - I'm Katerina, a Software Engineer at Supabase. I love cats and chocolate, I love being on stage talking to all of you, and I'm a Google Developer Expert for Angular and Maps. Today I want to show you how Angular and Supabase create the perfect stack for real-time applications.
-->

---

layout: center
class: text-center

---

<img src="/images/cats.jpg" class="max-w-[90vw] max-h-[80vh] object-contain mx-auto" alt="My cats">

---

layout: center
class: text-center

---

# The Demo

## 🍪 Cookie Catcher Game

<div class="text-6xl mb-8">🎮</div>

<div class="bg-gray-800 text-white p-6 rounded-lg mt-8">

**Rules:**

- Catch cookies 🍪 = **1 point**
- Catch cats 🐱 = **3 points**
- Compete on **live leaderboard!**

</div>

<div class="mt-8 text-gray-600">
<em>Live link will be shared later</em>
</div>

<!--
[Speaker notes for The Demo slide]
-->

---

layout: center
class: text-center

---

# Let's Play Together!

## 🍪 Cookie Catcher - Live Demo

<div class="text-6xl mb-8">🎮</div>

<div class="flex items-center justify-center gap-12">
  <img src="/images/game-qr.png" class="w-64 h-64" alt="QR Code">
  <h2 class="text-4xl font-bold"><a href="https://ngdemo-sb.netlify.app" target="_blank">ngdemo-sb.netlify.app</a></h2>
</div>

<!--
This is Cookie Catcher - a real-time multiplayer game where everyone in the room can join on their phones right now. You catch falling cookies and cats, compete on live leaderboards, and see each other's cursors moving in real-time. Go ahead, scan this QR code and join the game!

3 top winners get supabase swag
-->

---

layout: center
class: text-center

---

# thankz!

<div class="text-6xl mb-8">🍪</div>

<div class="text-xl space-y-4">

## [ngdemo-sb.netlify.app](https://ngdemo-sb.netlify.app)

**📚 i can has teh codez:** [github.com/mandarini/ac-demo-sb](https://github.com/mandarini/ac-demo-sb)

</div>

<div class="mt-12 text-lg">

**Connect with me:**

- 🐦 [@psybercity](https://x.com/psybercity)
- 🌐 [psyber.city](https://psyber.city)
- 💼 [github.com/mandarini](https://github.com/mandarini)

</div>

<!--
That's how we build real-time multiplayer experiences with Angular and Supabase. The combination of reactive signals and real-time infrastructure makes complex multiplayer features surprisingly simple to implement. Thank you for your attention - I'd love to answer any questions you have!
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
