<template>
  <div id="sync">
    <br /><br />
    <spinner :spin="fetching"></spinner>
    <v-button v-if="usable" @click="refresh" type="refresh">
      {{ $t("app.refreshbutton.label" /* Fetch updates */) }}
    </v-button>
    <v-button v-else @click="gotoconfig" type="issue">
      {{ $t("app.refreshbutton.finishsetup" /* Finish Setup */) }}
    </v-button>
    <div id="lastupdate">
      {{ $t("app.refreshbutton.lastupdate" /* Last update: */) }}
      {{ lastUpdate | fromNow($i18n.locale) }}
    </div>
  </div>
</template>

<script>
import moment from "moment";
import Spinner from "./Spinner.vue";
import Button from "./Button.vue";

export default {
  name: "refreshButton",
  props: ["usable", "fetching", "lastUpdate"],
  data() {
    return {
      lastUpdateTimer: null
    };
  },
  components: { Spinner, "v-button": Button },
  methods: {
    refresh() {
      this.$parent.compareSVwithWago();
    },
    gotoconfig() {
      this.$parent.configStep = 1;
    },
    scheduleTimer() {
      if (this.lastUpdateTimer) clearInterval(this.lastUpdateTimer);
      this.lastUpdateTimer = setInterval(() => {
        this.$forceUpdate();
      }, 1000 * 60);
    }
  },
  filters: {
    fromNow: (value, locale) => {
      if (!value) return "n/a";
      return moment(value)
        .locale(locale)
        .fromNow();
    }
  },
  mount() {
    this.scheduleTimer();
  },
  watch: {
    fetching() {
      this.scheduleTimer();
    }
  },
  beforeDestroy() {
    clearInterval(this.lastUpdateTimer);
  }
};
</script>

<style scoped>
#sync {
  flex: 1;
  text-align: center;
}
#lastupdate {
  margin-top: 5px;
  font-size: small;
}
</style>
