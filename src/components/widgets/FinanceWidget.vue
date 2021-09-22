<template>
<div class="content">
<div class="groups">
  <div v-for="group in groups" :key="group">
    <WaveChart :percentage='59'/>
  </div>
  <div class="addGroup unselectable" @click='addGroup("name",100)'>+</div>
</div>
</div>
</template>

<script>
/* eslint-disable */
import WaveChart from '../WaveChart'

export default {
  name: 'FinanceWidget',
  components: {
    WaveChart,
  },
  data () {
    return {
      groups: [
        {
          'name': 'expense name',
          'maxExpenses': 0,
          'currExpenses': 0,
          'expenses': []
        }
      ],
    }
  },
  methods: {
    syncGroups: function () {
      (async() => {
        const res = await fetch("http://dashboard.com/api/finances/finances");
        res.json().then(
          data => {
            if (data.groups == null) {
              this.groups.length = 0;
            }else{
              this.groups = data.groups;
            }
          }
        )
      })();
    },
    addGroup: function (name,maxExpenses) {
        let group = {
          name: name,
          maxExpenses: maxExpenses,
        };

      (async () => {
        const res = await fetch("http://dashboard.com/api/finances/finances",{
          method: 'POST',
          body: JSON.stringify(group)
        });
      })();

        //this.groups.push(group)

        this.syncGroups()
        //TODO: sync with server
    }
  },
  mounted: function () {
    this.syncGroups()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.content::-webkit-scrollbar {
  display: none;
}
.unselectable{
  -webkit-user-select: none; /* Safari */        
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* IE10+/Edge */
  user-select: none; /* Standard */
}

/* Hide scrollbar for IE, Edge and Firefox */
.content{
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}
.content {
  overflow: scroll;
 background: rgb(214,255,247);
background: linear-gradient(135deg, rgba(214,255,247,1) 0%, rgba(102,227,255,1) 36%, rgba(214,255,247,1) 100%); 
}

.groups{
  margin-top: 40px;
  display: grid;
  grid-template-columns: 150px 150px;
  gap: 40px;
  justify-content: center;
  align-content: center;
}
.addGroup{
    background-color: grey;
    width: 150px;
    height: 150px;
    border-radius: 50%;
    font-size: 40px;
    transform: translate3d(0, 0, 0);
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>
