<script setup lang="ts">
import Card from 'primevue/card'
import type { User } from '@/types/user'
import { PropType } from 'vue'

const props = defineProps({
  user: {
    type: Object as PropType<User>,
    required: true,
  },
})
</script>

<template>
  <Card>
    <template #tittle>User Profile</template>
    <template #content>
      <div class="flex flex-column rom-gap-2">
        <div>
          <i class="pi pi-user" />
          <span class="m-2">{{ user.full_name }}</span>
        </div>

        <div>
          <i class="pi pi-envelope" />
          <span class="m-2">
            <a :href="`mailto:${user.email}`"> { user.email } </a>
          </span>
        </div>
      </div>
    </template>
  </Card>
</template>