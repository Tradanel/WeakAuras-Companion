<template>
  <label class="file-select" @click="handleInputClick">
    <span class="select-button">
      <p class="configlabel">
        {{ $t("app.fileselect.wowfolder" /* World of Warcraft folder */) }}
      </p>
      <div class="fakeinput pointer">
        <span class="wow-path">{{ path }}&nbsp;</span>
      </div>
      <div class="pointer-icon" />
    </span>
  </label>
</template>

<script>
import { remote } from "electron";

export default {
  data() {
    return {
      dialogOpen: false
    };
  },
  props: {
    path: null
  },
  methods: {
    handleInputClick() {
      if (!this.dialogOpen) {
        this.dialogOpen = true;

        remote.dialog.showOpenDialog(
          { properties: ["openDirectory"] },
          paths => {
            this.dialogOpen = false;

            if (paths && paths.length) {
              this.$emit("update:path", paths[0]);
            }
          }
        );
      }
    }
  }
};
</script>

<style scoped>
/* Don't forget to hide the original file input! */
.file-select > input[type="file"] {
  display: none;
}
.pointer-icon {
  display: inline-block;
  border-radius: 2px;
  cursor: pointer;
  vertical-align: top;
  background-image: url("~@/assets/folder-icon.png");
  background-size: contain;
  height: 27px;
  width: 27px;
}
.pointer-icon:hover {
  background-image: url("~@/assets/folder-icon-active.png");
}
.fakeinput {
  background-color: #eee;
  width: 270px;
  height: 27px;
  font-size: 13px;
  white-space: nowrap;
  display: inline-table;
  table-layout: fixed;
  color: black;
  border-radius: 2px;
  padding: 0 5px;
}
.btn {
  display: inline-block;
  cursor: pointer;
}
.configlabel {
  color: #eee;
  margin: 3px 0 3px;
  font-size: 15px;
}
.wow-path {
  display: table-cell;
  vertical-align: middle;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
