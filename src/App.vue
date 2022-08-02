<template>
  <router-view />
</template>
<script>
import { defineComponent } from "vue";
import { useQuasar } from "quasar";

export default defineComponent({
  name: "App",
  setup() {
    const $q = useQuasar();
    const isNew = $q.cookies.get("r-noteapp-key");

    if (!isNew) {
      $q.cookies.set("r-noteapp-key", "true");
    } else {
    }
  },
});
</script>
