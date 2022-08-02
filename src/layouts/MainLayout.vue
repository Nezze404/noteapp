<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
      </q-toolbar>
      <div class="q-px-lg q-pt-md q-mb-sm">
        <div class="flex column q-pb-md">
          <div class="text-caption q-pb-sm">{{ todaysPhrase.quote }}</div>
          <div class="text-h6 text-capitalize self-end">
            - {{ todaysPhrase.author }}
          </div>
        </div>
        <div class="flex row justify-between q-px-sm">
          <div class="text-subtitle1">{{ todaysDate }}</div>
          <div class="text-subtitle1">Pendientes: 0</div>
        </div>
      </div>
      <q-img
        src="https://source.unsplash.com/daily?nature"
        class="header-image absolute-top"
      />
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="250"
      :breakpoint="600"
    >
      <q-scroll-area
        style="
          height: calc(100% - 192px);
          margin-top: 192px;
          border-right: 1px solid #ddd;
        "
      >
        <q-list padding>
          <q-item clickable to="/" exact v-ripple>
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>

            <q-item-section> Tareas </q-item-section>
          </q-item>
          <q-item clickable to="/help" exact v-ripple>
            <q-item-section avatar>
              <q-icon name="help" />
            </q-item-section>

            <q-item-section> Notas </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>
      <q-img
        class="absolute-top"
        src="https://source.unsplash.com/daily?code"
        style="height: 192px"
      >
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img src="" />
          </q-avatar>
          <div class="text-weight-bold"></div>
          <div></div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <router-view v-slot="{ Component }">
        <keep-alive>
          <component :is="Component" />
        </keep-alive>
      </router-view>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import { date } from "quasar";

function todaysDate() {
  const timeStamp = Date.now();
  const formattedString = date.formatDate(timeStamp, `DD MMMM [del] YYYY`);

  return formattedString;
}

export default defineComponent({
  name: "MainLayout",

  setup() {
    const leftDrawerOpen = ref(false);

    return {
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
      todaysPhrase: ref({}),
    };
  },
  async created() {
    const response = await fetch(
      "https://goquotes-api.herokuapp.com/api/v1/random/1?type=tag&val=money"
    );
    const data = await response.json();

    const phrase = {
      quote: data.quotes[0].text,
      author: data.quotes[0].author,
    };

    this.todaysPhrase = phrase;
  },
  computed: {
    todaysDate,
  },
});
</script>

<style lang="scss">
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.2;

  filter: grayscale(100%);
}
</style>
