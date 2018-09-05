<template>
  <section class="server-res" v-if="serverRes">
    <h3>Server response</h3>
    <div class="re-header">
      <p>Code</p>
      <p>Details</p>
    </div>
    <div class="response-content">
      <p class="response-code">{{ status }}</p>
      <div class="response-detail">
        <p>Response body</p>
        <div class="response-description">
          <Input 
            :value="response" 
            type="textarea" 
            autosize
            disabled
            class="dark"
          />
        </div>

        <p>Response headers</p>
        <div class="response-description">
          <Input 
            :value="headers" 
            type="textarea" 
            autosize
            disabled
            class="dark"
          />
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  name: "ServerRes",
  computed: {
    ...mapGetters(["serverRes"]),
    status() {
      return this.serverRes ? this.serverRes.status : "";
    },
    response() {
      if (!this.serverRes) return "";

      const { items, error_code, error_msg } = this.serverRes.data;

      return error_code ? error_msg : JSON.stringify(items, null, 2);
    },
    headers() {
      return this.serverRes
        ? JSON.stringify(this.serverRes.headers, null, 2)
        : "";
    }
  }
};
</script>

<style lang="less" scoped>
.server-res {
  margin-top: 40px;

  h3 {
    margin-bottom: 30px;
    padding-left: 16px;
    font-size: 20px;
    font-weight: 500;
    color: #87c5fe;
    border-left: 4px solid #87c5fe;
    line-height: 1;
  }

  .re-header {
    padding-bottom: 16px;
    display: flex;
    align-items: center;
    border-bottom: thin solid #dfdfdf;

    p {
      font-size: 14px;
      color: #727272;

      &:first-of-type {
        flex: 1;
      }

      &:last-of-type {
        width: 650px;
      }
    }
  }

  .response-detail {
    p {
      margin-bottom: 16px;
      font-size: 12px;
      color: #727272;
    }
  }

  .response-content {
    display: flex;
    margin-top: 16px;
  }

  .response-code {
    flex: 1;
  }

  .response-description {
    width: 650px;
    margin-bottom: 16px;

    & /deep/ .ivu-input[disabled] {
      cursor: text;
      color: #fff;
      background: #42444e;
    }
  }
}
</style>