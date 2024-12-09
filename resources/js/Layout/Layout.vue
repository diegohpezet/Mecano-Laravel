<script setup>
import { Link } from '@inertiajs/vue3';
</script>

<template>
  <header class="bg-light text-center shadow mb-5">
    <div class="py-2">
      <Link class="display-5" href="/">Mecano</Link>
    </div>
    <nav class="d-flex justify-content-center py-2">
      <ul class="list-group list-group-horizontal-sm">
        <li class="list-group-item"><Link href="/">Play</Link></li>
        <li class="list-group-item"><Link href="/leaderboard">Leaderboard</Link></li>
        <li class="list-group-item"><Link href="/login">Sign in!</Link></li>
      </ul>
    </nav>
  </header>

  <main class="container my-5">
    <slot>
      <!-- Content gets rendered here -->
    </slot>
  </main>

  <footer class="border-top pt-3">
    <p class="text-center">2025 Mecano. All rights reserved.</p>
    <p class="text-center">
      <a href="">Source code</a>
    </p>
  </footer>
</template>