<template>
  <div id="open" class="modal fade">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="close" data-dismiss="modal">
            <span>&times;</span>
          </button>
          <div class="h3 modal-title">Open from URL</div>
        </div>
        <div class="modal-body">
          <form class="form-horizontal">
            <div class="form-group">
              <label for="url" class="col-sm-2 control-label">url</label>
              <div class="col-sm-10">
                <input type="text" id="url" name="url" v-model="url" class="form-control" />
              </div>
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-success" @click="importFromURL()">
            Import from URL
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="js">
/* @flow */


export default {
  name: 'OpenModal',
  data(): any {
    return {
      selectMessage: 'Do you want to import a diagram from this URL? The existing diagram will be discarded.',
      url: ''
    }
  },
  mounted() {
    this.setEvent()
  },
  methods: {
    setEvent() {
      window.$('#open').on('shown.bs.modal hidden.bs.modal', () => {
        this.url = ''
      })
    },
    submit() {
      // 誤送信を防ぐために無効化する
    },
    importFromURL() {
      if (this.url.length && window.confirm(this.selectMessage)) {
        var components: [string] = this.url.split("/");
        var filename: string = components[components.length - 1];
        var encodedString: string = filename.split(".")[0];
        this.$store.dispatch('plantumlEditor/importUML', encodedString)
      }
      window.$('#open').modal('hide');
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
#open {
  .theme {
    width: 220px;
  }
}
</style>
