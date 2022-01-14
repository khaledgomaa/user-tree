<template>
  <table>
    <tbody>
      <tr>
       <td :colspan="datasource.children && datasource.children.length ? datasource.children.length*2 : null">
        <div class="node" :id="datasource.Id" @click.stop="handleClick(datasource)">
          <slot :node-data="datasource" class="node_item">
            <div class="container">
              <img v-bind:src="datasource.photo" v-bind:alt="datasource.fname" width="60px" height="60px"/>
              <div class="bottom-right">+{{datasource.children.length}}</div>
            </div>
            <div class="title">
              <i class="fa fa-users symbol"></i>
              {{ datasource.fname }}
            </div>
            <div class="content">{{ datasource.UserCode }}</div>
            <div class="content">{{ datasource.UserPoints }}</div>
            <div class="content">{{ datasource.UserLevel }}</div>
          </slot>
        </div>
       </td>
      </tr>
      <template v-if="datasource.children && datasource.children.length">
        <tr class="lines">
          <td :colspan="datasource.children.length*2">
            <div class="downLine"></div>
          </td>
        </tr>
        <tr class="lines">
          <td class="rightLine"></td>
         <template v-for="n in (datasource.children.length-1)">
            <td v-bind:key="n.id" class="leftLine topLine"></td>
            <td v-bind:key="n.id" class="rightLine topLine"></td>
         </template>
          <td class="leftLine"></td>
        </tr>
        <tr class="nodes">
          <td colspan="2" v-for="child in datasource.children" :key="child.id">
            <node :datasource="child" :handle-click="handleClick">
              <template v-for="slot in Object.keys($scopedSlots)" :slot="slot" slot-scope="scope">
                <slot :name="slot" v-bind="scope"/>
              </template>
            </node>
          </td>
        </tr>
      </template>
    </tbody>
  </table>
</template>

<script>
export default {
  name: "node",
  props: {
    datasource: Object,
    handleClick: Function,
  },
  methods: {},
};
</script>

<style scoped>
img {
  border-radius: 50%;
}
.container {
  position: relative;
  text-align: center;
  color: white;
  border-radius: 50%;
}
.bottom-right {
  position: absolute;
  bottom: 8px;
  right: 33px;
  background-color: #ffffff;
  border-radius: 80%;
  color: rgb(0, 0, 0);
  display: table;
  font-size: 0.1em;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  margin: 0 auto;
  width: 20px;
}
</style>
