<template>
  <div id="app" v-if="Object.keys(ds).length > 0">
    <!-- @node-click="selectNode" -->
    <org-chart :datasource="ds"  :pan="true" :zoom="true" :zoomout-limit="1.2" style="width:100%;display: inline;" class="child">
<!--       <template slot-scope="{ nodeData }">
        <b @click="selectNode(nodeData)">{{ nodeData.name }}</b>
      </template> -->
    </org-chart>
    <!-- <div id="sideBarContainer" class="child" dir="rtl">
      <div id="sideCategDiv">
        <div class="sectionLab">
          اختر المستوى المطلوب
        </div>
        <form action="">
          <label class="form-control">
            <input type="radio" value="عرض الجميع" @click="onChangeRadioLevel" name="level"/>
            عرض الجميع
          </label>

          <label class="form-control">
            <input type="radio" value="6%" @click="onChangeRadioLevel" name="level"/>
            6%
          </label>

          <label class="form-control">
            <input type="radio" value="9%" @click="onChangeRadioLevel" name="level" />
            9%
          </label>

          <label class="form-control">
            <input type="radio" value="12%" @click="onChangeRadioLevel" name="level" />
            12%
          </label>

          <label class="form-control">
            <input value="15%" type="radio" @click="onChangeRadioLevel" name="level" />
            15%
          </label>

          <label class="form-control">
            <input type="radio" value="18%" @click="onChangeRadioLevel" name="level" />
            18%
          </label>

          <label class="form-control">
            <input value="21%" type="radio" @click="onChangeRadioLevel" name="level" />
            21%
          </label>

          <label class="form-control">
            <input value="دون المستوى" type="radio" @click="onChangeRadioLevel" name="level" />
            دون المستوى
          </label>
        </form>
      </div>
    </div> -->
    
  </div>
</template>

<script>
import OrgChart from "./components/OrganizationChartContainer.vue";

export default {
  name: "app",
  components: {
    OrgChart,
  },
  data() {
    return {
      ds: {},
      currentNode: {
        id: "",
        name: "",
        title: "",
      },
      searchVal: "",
    };
  },

  methods: {
    selectNode(nodeData) {
      alert("node " + nodeData.fname + " is selected");
    },
    findNode(item, parent, searchVal) {
      if (item.children.length > 0) {
        for (let i = item.children.length - 1; i >= 0; i--) {
          this.findNode(item.children[i], item, searchVal);
        }
        if (item.children.length == 0 && item.UserLevel != searchVal) {
          this.removeNode(item, parent);
        }
      } else if (item.UserLevel != searchVal) {
        this.removeNode(item, parent);
      }
    },
    removeNode(item, parent) {
      if (parent != null) {
        const index = parent.children.indexOf(item);
        if (index > -1) {
          parent.children.splice(index, 1);
        }
      }
    },
    async getTreeData(userId) {
      let headers = {
        APIKEY: "elyamny491e6fbf1d3b4898abe10f89840f2093",
      };
      const response = await this.$http.get(
        `http://projectegy-001-site41.gtempurl.com/api/UserTree?UserId=${userId}`,
        { headers }
      );
      this.ds = response.data.model;
    },
    clearFilter() {
      this.getTreeData(this.getUserId());
    },
    getUserId() {
      let indexOfUserId = window.location.href.indexOf("userId") + 7;
      return window.location.href.substring(
        indexOfUserId,
        window.location.href.length
      );
    },
    onChangeRadioLevel(event) {
      if (event.target.value == "عرض الجميع") {
        this.getTreeData(this.getUserId());
      } else {
        this.findNode(this.ds, null, event.target.value);
      }
    },
  },
  mounted() {
    this.getTreeData(this.getUserId());
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.child {
  display: inline-block;
  border: 1px solid red;
  padding: 1rem 1rem;
  vertical-align: middle;
}
.sideBarBody {
  font-family: cursive;
  font-size: 15pt;
  margin-left: 3%;
}

.sectionLab {
  margin-bottom: 1%;
  margin-top: 2%;
  color: coral;
  font-size: 25pt;
  font-weight: 500;
}

#priceDiv {
  margin-top: 2%;
  color: coral;
}

#fromtxt,
#totxt {
  height: 25px;
  width: 50%;
}

#toDiv,
#fromDiv {
  display: table-cell;
}
#sideBarContainer {
  line-height: 2.5;
}

form {
  display: grid;
  place-content: center;
}

.form-control {
  font-family: system-ui, sans-serif;
  font-size: 2rem;
  font-weight: bold;
  line-height: 1.1;
  display: grid;
  grid-template-columns: 1em auto;
  gap: 0.5em;
  color: coral;
}

.form-control + .form-control {
  margin-top: 1em;
}

.form-control:focus-within {
  color: var(--form-control-color);
  color: chocolate;
}

input[type="radio"] {
  /* Add if not using autoprefixer */
  -webkit-appearance: none;
  /* Remove most all native input styles */
  -moz-appearance: none;
  appearance: none;
  /* For iOS < 15 */
  background-color: var(--form-background);
  /* Not removed via appearance */
  margin: 0;
  font: inherit;
  color: chocolate;
  width: 1.15em;
  height: 1.15em;
  border: 0.15em solid chocolate;
  border-radius: 50%;
  transform: translateY(-0.075em);
  display: grid;
  place-content: center;
}

input[type="radio"]::before {
  content: "";
  width: 0.65em;
  height: 0.65em;
  border-radius: 50%;
  transform: scale(0);
  transition: 120ms transform ease-in-out;
  box-shadow: inset 1em 1em var(--form-control-color);
  /* Windows High Contrast Mode */
  background-color: chocolate;
  color: chocolate;
}

input[type="radio"]:checked::before {
  transform: scale(1);
  color: chocolate;
}

input[type="radio"]:focus {
  outline: max(2px, 0.15em) solid chocolate;
  outline-offset: max(2px, 0.15em);
  color: chocolate;
}
</style>
