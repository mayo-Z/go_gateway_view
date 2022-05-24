<template>
  <div class="mixin-components-container">
    <el-row>
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span v-if="isEdit == true">修改HTTP服务</span>
          <span v-if="isEdit == false">创建HTTP服务</span>
        </div>
        <div style="margin-bottom: 50px">
          <el-form ref="form" :model="sizeForm" label-width="140px" size="mini">
            <el-form-item label="服务名称" class="is-required">
              <el-input
                v-model="sizeForm.service_name"
                placeholder="6~128位字母数字下划线"
                :disabled="isEdit == true"
              ></el-input>
            </el-form-item>
            <el-form-item label="服务描述" class="is-required">
              <el-input
                v-model="sizeForm.service_desc"
                placeholder="最多255字符，必填"
              ></el-input>
            </el-form-item>
            <el-form-item label="接入类型" class="is-required">
              <el-input
                :disabled="isEdit == true"
                placeholder="路径格式: /user/,域名格式: ww.test.com"
                v-model="sizeForm.rule"
                class="input-with-select"
              >
                <template #prepend>
                  <el-select
                    :disabled="isEdit == true"
                    v-model="sizeForm.rule_type"
                    placeholder="请选择"
                    style="width: 80px"
                  >
                    <el-option label="路径" :value="0"></el-option>
                    <el-option label="域名" :value="1"></el-option>
                  </el-select>
                </template>
              </el-input>
            </el-form-item>

            <el-form-item label="支持https">
              <el-switch
                v-model="sizeForm.need_https"
                :active-value="1"
                :inactive-value="0"
              >
              </el-switch>
              <span style="font-weight: 700"
                >&nbsp;&nbsp;&nbsp;&nbsp;支持strip_uri &nbsp;&nbsp;</span
              >
              <el-switch
                v-model="sizeForm.need_strip_uri"
                :active-value="1"
                :inactive-value="0"
              >
              </el-switch>
              <span style="font-weight: 700"
                >&nbsp;&nbsp;&nbsp;&nbsp;支持need_websocket &nbsp;&nbsp;</span
              >
              <el-switch
                v-model="sizeForm.need_websocket"
                :active-value="1"
                :inactive-value="0"
              >
              </el-switch>
            </el-form-item>

            <el-form-item label="URL重写">
              <el-input
                v-model="sizeForm.url_rewrite"
                type="textarea"
                autosize
                placeholder="格式: ^/gatekeeper/test_service(.*) $1 多条换行"
              ></el-input>
            </el-form-item>
            <el-form-item label="Header转换">
              <el-input
                v-model="sizeForm.header_transfor"
                type="textarea"
                autosize
                placeholder="header转换支持增加(add)、删除(del)、修改(edit)格式: add headname headvalue 多条换行"
              ></el-input>
            </el-form-item>
            <el-form-item label="开启验证">
              <el-switch
                v-model="sizeForm.open_auth"
                :active-value="1"
                :inactive-value="0"
              >
              </el-switch>
            </el-form-item>
            <el-form-item label="IP白名单">
              <el-input
                v-model="sizeForm.white_list"
                type="textarea"
                autosize
                placeholder="格式: 127.0.0.1 多条换行，白名单优先级高于黑名单"
              ></el-input>
            </el-form-item>
            <el-form-item label="IP黑名单">
              <el-input
                v-model="sizeForm.black_list"
                type="textarea"
                autosize
                placeholder="格式: 127.0.0.1 多条换行"
              ></el-input>
            </el-form-item>
            <el-form-item label="客户端限流">
              <el-input
                v-model="sizeForm.clientip_flow_limit"
                placeholder="0表示无限制"
              ></el-input>
            </el-form-item>
            <el-form-item label="服务端限流">
              <el-input
                v-model="sizeForm.service_flow_limit"
                placeholder="0表示无限制"
              ></el-input>
            </el-form-item>
            <el-form-item label="轮询方式">
              <el-radio-group v-model="sizeForm.round_type">
                <el-radio :label="0">random</el-radio>
                <el-radio :label="1">round-robin</el-radio>
                <el-radio :label="2">weight_round_robin</el-radio>
                <el-radio :label="3">ip_hash</el-radio>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="IP列表" class="is-required">
              <el-input
                v-model="sizeForm.ip_list"
                type="textarea"
                autosize
                placeholder="格式: 127.0.0.1:80 多条换行"
              ></el-input>
            </el-form-item>
            <el-form-item label="权重列表" class="is-required">
              <el-input
                v-model="sizeForm.weight_list"
                type="textarea"
                autosize
                placeholder="格式50 多条换行"
              ></el-input>
            </el-form-item>
            <el-form-item label="建立连接超时">
              <el-input
                v-model="sizeForm.upstream_connect_timeout"
                placeholder="单位s，0表示无限制"
              ></el-input>
            </el-form-item>
            <el-form-item label="获取header超时">
              <el-input
                v-model="sizeForm.upstream_header_timeout"
                placeholder="单位s，0表示无限制"
              ></el-input>
            </el-form-item>
            <el-form-item label="链接最大空闲时间">
              <el-input
                v-model="sizeForm.upstream_idle_timeout"
                placeholder="单位s，0表示无限制"
              ></el-input>
            </el-form-item>
            <el-form-item label="最大空闲链接数">
              <el-input
                v-model="sizeForm.upstream_max_idle"
                placeholder="0表示无限制"
              ></el-input>
            </el-form-item>

            <el-form-item size="large">
              <el-button
                type="primary"
                :disabled="submitButDisabled"
                @click="handleSubmit"
                >立即提交</el-button
              >
            </el-form-item>
          </el-form>
        </div>
      </el-card>
    </el-row>
  </div>
</template>


<script>
import {
  serviceAddHttp,
  serviceDetail,
  serviceUpdateHttp,
} from "@/api/service";

export default {
  name: "ServiceCreateHttp",
  data() {
    return {
      isEdit: false,
      submitButDisabled: false,
      sizeForm: {
        service_name: "",
        service_desc: "",
        rule_type: 0,
        rule: "",
        need_https: 0,
        need_websocket: 0,
        need_strip_uri: 1,
        url_rewrite: "",
        header_transfor: "",
        round_type: 2,
        ip_list: "",
        weight_list: "",
        upstream_connect_timeout: "",
        upstream_header_timeout: "",
        upstream_idle_timeout: "",
        upstream_max_idle: "",
        open_auth: 0,
        black_list: "",
        white_list: "",
        clientip_flow_limit: "",
        service_flow_limit: "",
      },
    };
  },
  created() {
    const id = this.$route.params && this.$route.params.id;
    if (id > 0) {
      this.isEdit = true;
      this.fetchData(id);
    }
  },
  methods: {
    fetchData(id) {
      const query = { id: id };
      serviceDetail(query)
        .then((response) => {
          const formData = response.data;
          this.sizeForm.id = formData.info.id;
          this.sizeForm.load_type = formData.info.load_type;
          this.sizeForm.service_name = formData.info.service_name;
          this.sizeForm.service_desc = formData.info.service_desc;
          this.sizeForm.rule_type = formData.http_rule.rule_type;
          this.sizeForm.rule = formData.http_rule.rule;
          this.sizeForm.need_https = formData.http_rule.need_https;
          this.sizeForm.need_websocket = formData.http_rule.need_websocket;
          this.sizeForm.need_strip_uri = formData.http_rule.need_strip_uri;
          this.sizeForm.url_rewrite = formData.http_rule.url_rewrite.replace(/,/g,"\n");
          this.sizeForm.header_transfor =formData.http_rule.header_transfor.replace(/,/g,"\n");
          this.sizeForm.round_type = formData.load_balance.round_type;
          this.sizeForm.ip_list = formData.load_balance.ip_list;
          this.sizeForm.weight_list = formData.load_balance.weight_list.replace(
            /,/g,
            "\n"
          );
          this.sizeForm.round_type = formData.load_balance.round_type;
          this.sizeForm.upstream_connect_timeout =
            formData.load_balance.upstream_connect_timeout;
          this.sizeForm.upstream_header_timeout =
            formData.load_balance.upstream_header_timeout;
          this.sizeForm.upstream_idle_timeout =
            formData.load_balance.upstream_idle_timeout;
          this.sizeForm.upstream_max_idle =
            formData.load_balance.upstream_max_idle;
          this.sizeForm.open_auth = formData.access_control.open_auth;
          this.sizeForm.black_list = formData.access_control.black_list.replace(
            /,/g,
            "\n"
          );
          this.sizeForm.white_list = formData.access_control.white_list.replace(
            /,/g,
            "\n"
          );
          this.sizeForm.clientip_flow_limit =
            formData.access_control.clientip_flow_limit;
          this.sizeForm.service_flow_limit =
            formData.access_control.service_flow_limit;
        })
        .catch(() => {});
    },
    handleSubmit() {
      this.submitButDisabled = true;
      const query = Object.assign({}, this.sizeForm);
      console.log(query);
      query.white_list = query.white_list.replace(/\n/g, ",");
      query.black_list = query.black_list.replace(/\n/g, ",");
      query.url_rewrite = query.url_rewrite.replace(/\n/g, ",");
      query.header_transfor = query.header_transfor.replace(/\n/g, ",");
      query.ip_list = query.ip_list.replace(/\n/g, ",");
      query.weight_list = query.weight_list.replace(/\n/g, ",");

      query.clientip_flow_limit = Number(query.clientip_flow_limit);
      query.service_flow_limit = Number(query.service_flow_limit);
      query.upstream_connect_timeout = Number(query.upstream_connect_timeout);
      query.upstream_header_timeout = Number(query.upstream_header_timeout);
      query.upstream_idle_timeout = Number(query.upstream_idle_timeout);
      query.upstream_max_idle = Number(query.upstream_max_idle);
      if (this.isEdit) {
        serviceUpdateHttp(query)
          .then((response) => {
            this.submitButDisabled = false;
            this.$notify({
              title: "Success",
              message: "修改成功",
              type: "success",
              duration: 2000,
            });
          })
          .catch(() => {
            this.submitButDisabled = false;
          });
      } else {
        serviceAddHttp(query)
          .then((response) => {
            this.submitButDisabled = false;
            this.$notify({
              title: "Success",
              message: "添加成功",
              type: "success",
              duration: 2000,
            });
          })
          .catch(() => {
            this.submitButDisabled = false;
          });
      }
    },
  },
};
</script>

<style scoped>
.mixin-components-container {
  background-color: #f0f2f5;
  padding: 30px;
  min-height: calc(100vh - 84px);
}
.component-item {
  min-height: 100px;
}
</style>
