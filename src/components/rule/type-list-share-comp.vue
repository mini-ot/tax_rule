<template>
  <div v-cloak>
    <div flex v-if="data.conditions.length>1">
      <div style="margin-right:5px">{{data.name}}</div>
      <div class="b-tree">
        <div class="b-tree-item">
          <Poptip padding="0" trigger="hover" placement="bottom-start">
            <span class="cursor blue">规则：</span>
            <div slot="content">
              <ul class="side-small-list">
                <li v-for="vo in ruleList" :key="vo.id" @click="rule(vo)">
                  {{vo.sign}}
                 <span style="color:#999">[{{vo.title}}]</span>
                </li>
              </ul>
            </div>
          </Poptip>

          <span class="green">{{data.rule.sign||'______'}}</span>
        </div>
        <div
          class="b-tree-item"
          v-for="(vo,index) in data.conditions"
          :key="index"
          flex="cross:center"
        >
          <Icon
            @click="del(index)"
            color="#999"
            size="14"
            class="cursor"
            type="ios-remove-circle-outline"
            style="margin-right:5px"
          />

          <span class="green">{{vo.name||'___'}}</span>
        </div>

        <div class="b-tree-item">
          <typeChange @on-change="getVariable($event)" type="var" position="bottom-start">
            <span class="cursor blue" slot="button" @click="add" style="margin-left:3px">+</span>
          </typeChange>
        </div>
      </div>
    </div>

    <div flex="cross:center" v-if="data.conditions.length<=1">
      <span style="margin-right:5px">{{data.name}}（</span>

      <Poptip padding="0" trigger="hover" placement="bottom-start">
        <span class="cursor blue">规则：</span>
        <div slot="content">
          <ul class="side-small-list" v-if="ruleList.length">
            <li v-for="vo in ruleList" :key="vo.id" @click="rule(vo)">
              {{vo.sign}}
                 <span style="color:#999">[{{vo.title}}]</span>
            </li>
          </ul>
          <div v-else style="padding:10px 20px;color:#999">没有专用规则</div>
        </div>
      </Poptip>

      <span class="green">{{data.rule.sign||'______'}}</span>

      ，
      <template v-if="data.conditions.length">
        <span flex="cross:center" v-for="(vo,index) in data.conditions" :key="index">
          <span class="green">{{vo.name||'___'}}</span>

          <Icon
            @click="del(index)"
            color="#999"
            size="14"
            class="cursor"
            type="ios-remove-circle-outline"
          />，
        </span>
      </template>

      <span v-if="!data.conditions.length" class="hui" style="margin-right:5px">请添加变量</span>
      <typeChange @on-change="getVariable($event)" type="var" position="bottom-start">
        <span class="cursor blue" slot="button" @click="add">+</span>
      </typeChange>

      <span style="margin-left:5px">）</span>
    </div>
  </div>
</template>

<script>
import typeChange from "@/components/type-change.vue";
export default {
  components: { typeChange },
  props: ["data"],
  computed:{
      ruleList(){
        var _this = this,data = [];
        _.each(this.$store.state.ruleData, function(vo) {
          _.each(vo.items, function(v) {
            if (v.isList) {
              data.push(v);
            }
          });
        });
        return data
      }
  },
  methods: {
    getVariable(res) {
      this.data.conditions.push(res);
    },
    add() {
      this.data.conditions.push();
    },
    del(i) {
      this.data.conditions.splice(i, 1);
    },
    rule(val) {
      var data = JSON.parse(JSON.stringify(val));
      this.data.rule = {
        sign: data.sign
      };
    }
  }
};
</script>
<style scoped>
[v-cloak] {
  display: none;
}
.side-small-list {
  list-style: none;
  padding: 0 15px;
}
.side-small-list li {
  border-bottom: #eee 0.5px solid;
  padding: 10px 0;
  cursor: pointer;
}
.side-small-list li:nth-last-child(1) {
  border-bottom: none;
}
</style>