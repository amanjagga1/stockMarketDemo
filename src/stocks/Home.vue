<template>
<div>
    <table v-if="Object.keys(stocks).length" class="table center-table is-fullwidth">
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
          <th>Last Updated</th>
        </tr>
      </thead>
      <tfoot>
        <tr>
          <th>Name</th>
          <th>Price</th>
          <th>Last Updated</th>
        </tr>
      </tfoot>
      <tbody>
        <tr v-for="(value,name) in stocks" :class="{'is-selected': value.className}" :key="name">
          <th :class="value.className">{{name}}</th>
          <td :class="value.className">{{value.price}}</td>
          <td :class="value.className">{{ (update - value.time) | updatedAgo }}</td>
        </tr>
      </tbody>
    </table>
    <div v-else class="select is-loading is-large"></div>
</div>
  
</template>

<script>
export default {
 props : ['stocks','update'],
 filters: {
     updatedAgo: function(time) {
         time = time/1000;
         if (time < 1)
            return "Updated Just now"; 
         else if (time < 60)
            return Math.floor(time) + " seconds ago";
        else
            return Math.floor(time/60) + " minutes ago" 
     }
 }
}
</script>

<style lang="scss">
.center-table th {
    text-align: center;
    vertical-align: middle;
}

.center-table td {
    text-align: center;
    vertical-align: middle;
} 
</style>