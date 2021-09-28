<template>
  <div class="content">
    <div class="groups">
      <div v-for="group in groups" :key="group">
        <WaveChart :percentage="59" />
        <Popup
          ref="popup"
          :x="100"
          :y="100"
          theme="linear-gradient(167deg, rgba(212,255,255,1) 0%, rgba(0,249,255,1) 100%)"
        >
          <h2>new group</h2>
          <Input
            class="input"
            label="Group title"
            ref="groupInp"
            color="lightblue"
          />
          <div class="bottom">
            <Input
              class="input limit"
              type="number"
              label="Limit"
              ref="maxInp"
              color="lightblue"
            />
            <button class="btn" @click="addGroupClick()">Add</button>
          </div>
        </Popup>
      </div>
      <div class="addGroup unselectable" @click="$refs.popup[0].show()">+</div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import WaveChart from "../WaveChart";
import Popup from "../Popup";
import Input from "../Input";

export default {
  name: "FinanceWidget",
  components: {
    WaveChart,
    Popup,
    Input,
  },
  data() {
    return {
      groups: [
        {
          name: "expense name",
          maxExpenses: 0,
          currExpenses: 0,
          expenses: [],
        },
      ],
    };
  },
  methods: {
    syncGroups: function () {
      (async () => {
        const res = await fetch("http://dashboard.com/api/finances/finances");
        res.json().then((data) => {
          if (data.groups == null) {
            this.groups.length = 0;
          } else {
            this.groups = data.groups;
          }
        });
      })();
    },
    addGroupClick: function () {
      let name = this.$refs.groupInp[0].value;
      if (name.length < 0 && name.length > 50) {
        //TODO: error
        return;
      }

      let max = this.$refs.maxInp[0].value;
      if (max < 0 && max > 2147483647) {
        //TODO: error
        return;
      }

      this.addGroup(name, max);
    },
    addGroup: function (name, maxExpenses) {
      let group = {
        name: name,
        maxExpenses: maxExpenses,
      };

      (async () => {
        const res = await fetch("http://dashboard.com/api/finances/finances", {
          method: "POST",
          body: JSON.stringify(group),
        });
      })();
      //this.groups.push(group)

      this.syncGroups();
      //TODO: sync with server
    },
  },
  mounted: function () {
    this.syncGroups();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.content::-webkit-scrollbar {
  display: none;
}
.unselectable {
  -webkit-user-select: none; /* Safari */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* IE10+/Edge */
  user-select: none; /* Standard */
}

/* Hide scrollbar for IE, Edge and Firefox */
.content {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}
.content {
  overflow: scroll;
  background: rgb(214, 255, 247);
  background: linear-gradient(
    135deg,
    rgba(214, 255, 247, 1) 0%,
    rgba(102, 227, 255, 1) 36%,
    rgba(214, 255, 247, 1) 100%
  );
}

.groups {
  margin-top: 40px;
  display: grid;
  grid-template-columns: 150px 150px;
  gap: 40px;
  justify-content: center;
  align-content: center;
}
.addGroup {
  border-style: dashed;
  width: 150px;
  height: 150px;
  border-radius: 50%;
  font-size: 40px;
  transform: translate3d(0, 0, 0);
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup h2 {
  margin: 5px;
}
.input {
  border-radius: 20px;
}
.bottom {
  display: flex;
}
.limit {
  width: 60%;
}
.btn {
  flex-grow: 1;
  color: black;
  font-size: 20px;

  border: 0;
  border-radius: 20px;
  background: rgb(212, 255, 255);
  background: linear-gradient(
    167deg,
    rgba(212, 255, 255, 1) 0%,
    rgba(0, 249, 255, 1) 100%
  );
}
.btn:hover {
  cursor: pointer;
  box-shadow: 0px 0px 10px black;
  transition-duration: 0.4s;
}
.btn:active {
  filter: invert(0.3);
}
</style>
