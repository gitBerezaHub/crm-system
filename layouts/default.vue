<script lang="ts" setup>
import {account} from '@/utils/appwrite'
import {onMounted} from "vue";

const authStore = useAuthStore()
const isLoadingStore = useIsLoadingStore()
const router = useRouter()

onMounted(async () => {
  try {
    const user = await account.get()
    if (user) authStore.set(user)
  } catch (error) {
    router.push('/login')
  } finally {
    isLoadingStore.set(false)
  }
})

</script>

<template>
  <LayoutLoader v-if="isLoadingStore.isLoading" />
  <section v-else :class="{grid: authStore.isAuth}">
    <LayoutSidebar v-if="authStore.isAuth"/>
    <div>
      <slot/>
    </div>
  </section>
</template>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: 1fr 5fr;
  min-height: 100vh;
}
</style>
